# 🧾 Git & GitHub Terms

*This reference doc was generated using OpenAI's [ChatGPT](https://chatgpt.com/).*

## 🔹 **Repository (repo)**
- Think of it as a **project folder** with superpowers.
- It stores files _and_ a complete history of changes.
- Two flavors:
  - **Local repo** → on the computer.
  - **Remote repo** → on GitHub (the cloud version).

---

## 🔹 **Commit**
- A **snapshot** of our project at a point in time.
- Like hitting _Save_ in a video game — we can always go back.
- Each commit has:
  - An ID (hash).
  - A message (A note about the change).

---

## 🔹 **Branch**
- A **parallel timeline** of our project.
- Default branch is usually called `main`.
- We make new branches to test features without messing up the main story.

---

## 🔹 **Clone**
- Copying a **remote repo** from GitHub to our local machine.
- We get the whole history, not just the latest files.

---

## 🔹 **Fork**
- **personal copy of someone else’s repo** on GitHub

---

## 🔹 **.gitignore**
- A file that tells Git **what to ignore** (e.g., secrets, log files, cache).

---

## 🔹 **HEAD**
- A pointer to our current branch and commit.
- Think of it as “where we are standing in history.”

---

## 🔹 **Remote**
- A reference to another repo, usually on GitHub.
- Default remote is called origin.

---

## 🔹 **Fetch**
- Downloads changes from the remote repo without merging them yet.
- Lets us review before we decide to merge.

---

## 🔹 **Conflict**
- Happens when two people edit the same line of a file differently.
- Git will ask us to choose which version wins.

---

## 🔹 **Tag**
- A label for a specific commit.
- Often used for versions/releases (e.g., v1.0).

---

## 🔹 **Actions (GitHub-only)**
Automation built into GitHub: run tests, deploy code, etc., every time you push.

---

## So if we piece it together like a story:
- We **clone** a repo.
- We work on a **branch**.
- We **stage** and **commit** changes.
- We **push** them to GitHub.
- We open a **pull** request to merge our work.
- Others review and **merge** it.