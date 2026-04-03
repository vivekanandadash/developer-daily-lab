# 🔐 securing spring boot credentials
**date:** 2026-04-03
**topic:** never push passwords to github

---

## ✍️ what I understood

when you put your database username and password directly in `application.properties` and push it to github, **anyone in the world can see it.**

the fix is simple — split your config into two files:
- one file with **safe config** → goes to github ✅
- one file with **secrets** → stays on your machine only ✅

spring boot automatically combines both files when you run your app.

---

## 🔑 important points

- `application.properties` → push to github → **no passwords**
- `application-local.properties` → never push → **has passwords**
- add `application-local.properties` to `.gitignore` so git never tracks it
- add `spring.profiles.active=local` in `application.properties` so spring boot knows to also read the local file
- when you clone your repo again later → just recreate `application-local.properties` manually (takes 2 minutes)
- your database is **local** — it never goes to github anyway

---

## 💡 example

### `application.properties` (safe to push ✅)
```properties
spring.application.name=product-service
server.port=8082

spring.datasource.url=jdbc:mysql://localhost:3306/shopcore_product_db?createDatabaseIfNotExist=true&useSSL=false&allowPublicKeyRetrieval=true
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
eureka.client.fetch-registry=true
eureka.client.register-with-eureka=true
eureka.instance.prefer-ip-address=true

spring.servlet.multipart.max-file-size=10MB
spring.servlet.multipart.max-request-size=10MB

spring.profiles.active=local
```

### `application-local.properties` (never push ❌)
```properties
spring.datasource.username=root
spring.datasource.password=yourpassword
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
aws.region=ap-south-1
aws.s3.bucket-name=your-bucket-name
```

### `.gitignore` entry
```
application-local.properties
```

---

## ❌ mistake I made

pushed `application.properties` to github with **hardcoded credentials**:

```properties
spring.datasource.username=root
spring.datasource.password=Vivek29082784
```

this exposed my password publicly — anyone who found my repo could see it.

**what a hacker could do with this:**
- delete my entire database
- steal all my data
- run expensive AWS services on my bill
- use my account for illegal activities

---

## ✅ how I fixed it

**step 1** → created `application-local.properties` and moved all secrets there

**step 2** → removed username, password, aws credentials from `application.properties`

**step 3** → added `spring.profiles.active=local` to `application.properties`

**step 4** → added `application-local.properties` to `.gitignore`

**step 5** → pushed clean code to github:
```bash
git add .
git commit -m "hide credentials"
git push origin main
```

---

## 🧠 note to self

> your **code** lives on github.
> your **secrets** live only on your machine.
> never mix the two. ever. 🔒
