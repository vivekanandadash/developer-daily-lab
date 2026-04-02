# 📅 2026-04-02 – Git Mistakes & Learnings

## ❌ What I did wrong

### 1. Created Git repo inside another repo

* I ran `git init` inside `daily-notes`
* But `developer-daily-lab` was already a Git repo
* This created a **nested repository**

👉 Result:

* Git treated `daily-notes` as a **submodule**
* Saw warning: `embedded git repository`
* GitHub showed folder incorrectly

---

### 2. Faced `160000 mode` issue

* Git tracked folder as:

  ```
  create mode 160000 daily-notes
  ```

👉 Meaning:

* It was not a normal folder
* It became a **gitlink/submodule**

---

### 3. Branch confusion (`main` vs `master`)

* Tried pushing to `main`
* But local branch was `master`

👉 Error:

```
src refspec main does not match any
```

---

### 4. Non-fast-forward error

* Remote repo already had commits (README)
* Tried pushing without pulling first

👉 Error:

```
non-fast-forward
```

---

### 5. File naming mistake

* Created file:

  ```
  # @RequestBody.txt
  ```

👉 Problem:

* Special characters (`#`, space) caused Git issues

---

## ✅ How I fixed it

* Removed inner `.git` folder
* Used:

  ```
  git rm --cached daily-notes
  ```
* Re-added folder properly
* Pulled remote changes:

  ```
  git pull origin main --allow-unrelated-histories
  ```
* Renamed branch:

  ```
  git branch -M main
  ```

---

## 🧠 Key Learnings (Never Repeat)

### ✔ One project = one `.git`

Never run `git init` inside subfolders

---

### ✔ Understand Git warnings

* `embedded repository` = nested repo problem
* `160000` = submodule (danger in this case)

---

### ✔ Always check branch before push

```
git branch
```

---

### ✔ Pull before push (if remote has commits)

```
git pull origin main
```

---

### ✔ Use clean file names

❌ `# @RequestBody.txt`
✅ `request-body.md`

---

### ✔ Use Git commands properly

* `git mv` for renaming
* `git rm` for deleting

---

## 🚀 What I improved today

* Understood Git internals (submodule, gitlink)
* Learned how to debug Git errors
* Fixed real-world Git issues
* Improved daily Git workflow

---

## 📌 Next Plan

* Continue daily notes
* Maintain clean Git history
* Learn proper repo structure
* Practice Git commands daily

---

🔥 **Final Thought:**

> Mistakes are part of learning — but repeating the same mistake is not.

---
