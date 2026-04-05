# 🧪 developer-daily-lab

**Date:** 2026-04-05
**Topic:** Spring Boot → S3 Upload + AWS IAM deep dive
**Tags:** #springboot #aws-s3 #iam #java #cloud

---

## ✍️ What I Understood

To push a file from Spring Boot to an S3 bucket, AWS needs to know **who** is making the request and **whether they're allowed**. That's where IAM comes in — it's basically AWS's permission system.

The flow is: my Spring Boot app uses the **AWS SDK for Java v2**. It picks up credentials automatically (via env vars, instance profile, or `~/.aws/credentials`). Those credentials belong to an IAM entity — either a user or a role. The entity has policies attached that say what it can or can't do on S3.

---

## 🔑 Important Points

### Spring Boot — Upload to S3

First, add the dependency in `pom.xml`:

```xml
<!-- AWS SDK S3 -->
<dependency>
  <groupId>software.amazon.awssdk</groupId>
  <artifactId>s3</artifactId>
  <version>2.25.11</version>
</dependency>
```

Then the service class:

```java
@Service
public class S3UploadService {

  private final S3Client s3Client;

  @Value("${aws.bucket.name}")
  private String bucketName;

  public S3UploadService() {
    // SDK auto-detects credentials from env / role
    this.s3Client = S3Client.builder()
        .region(Region.AP_SOUTH_1)
        .build();
  }

  public String uploadFile(MultipartFile file) throws IOException {
    String key = UUID.randomUUID() + "_" + file.getOriginalFilename();

    PutObjectRequest request = PutObjectRequest.builder()
        .bucket(bucketName)
        .key(key)
        .contentType(file.getContentType())
        .build();

    s3Client.putObject(request,
        RequestBody.fromBytes(file.getBytes()));

    return "https://" + bucketName + ".s3.amazonaws.com/" + key;
  }
}
```

The controller just receives `MultipartFile` and calls the service. Credentials I kept in `application.properties` using `aws.accessKeyId` and `aws.secretAccessKey` — but for production, use a **role** instead (see IAM section below).

---

### How IAM Works

IAM = Identity and Access Management. It controls **who** can do **what** in AWS.

| Concept | What it is | Type |
|---------|-----------|------|
| **User** | A real person or app with long-term credentials (access key + secret) | Identity |
| **Group** | A collection of users — attach a policy once, it applies to everyone in the group | Identity |
| **Role** | Like a user but no long-term creds — assumed temporarily by a service (EC2, Lambda) or a user. Best for production | Identity |
| **Policy** | A JSON document that says which actions are allowed/denied on which resources | Permission |
| **Permission** | A single allow/deny rule inside a policy (e.g. `s3:PutObject` on `arn:aws:s3:::my-bucket/*`) | Permission |

---

## 💡 Examples

### Request Flow — Who Gets Access?

```
Spring Boot app  →  assumes IAM Role  →  Role has Policy attached  →  s3:PutObject ✓
```

In dev, I used an IAM user with an access key. In prod, the EC2 instance (or ECS task) is assigned a role directly — no keys stored anywhere. The SDK picks up the role automatically via the instance metadata service.

Groups are useful when my team grows — I create a group called `developers`, attach an S3 read/write policy to it, and anyone added to that group gets the permissions automatically. Much cleaner than attaching policies user by user.

### Minimal S3 Policy for This Use Case

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:DeleteObject"
      ],
      "Resource": "arn:aws:s3:::my-bucket/*"
    },
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::my-bucket"
    }
  ]
}
```

The `Resource` on `PutObject` needs `/*` (the objects inside the bucket), while `ListBucket` points to the bucket itself (no trailing slash). I mixed these up the first time — see mistakes below.

---

## ❌ Mistakes I Made

**Mistake 01**
Used the bucket ARN `arn:aws:s3:::my-bucket` for `PutObject` instead of `arn:aws:s3:::my-bucket/*`. Got `AccessDenied` even though the policy looked right.

→ **Fix:** Object-level actions like `PutObject` and `GetObject` need the `/*` suffix — they operate on objects, not the bucket itself.

---

**Mistake 02**
Hardcoded `Region.US_EAST_1` in the SDK builder but my bucket was in `ap-south-1`. Got a redirect error that took a while to understand.

→ **Fix:** Always match the region in code to where the bucket actually lives, or use `Region.of("ap-south-1")` from a config value.

---

**Mistake 03**
Confused roles and users — tried to "log in" to a role the same way as a user. Roles don't have passwords or access keys directly.

→ **Fix:** Roles are *assumed*, not logged into. A service (EC2, Lambda) or a user with `sts:AssumeRole` permission picks up the role temporarily and gets short-lived credentials.

---

> don't try to be perfect — just show up daily and write ✍️
