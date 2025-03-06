




----------------------------------------------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------------------------------------------
# ✅ Step-by-Step Guide to Push Files to GitHub

Follow these steps to push your files to GitHub.

---

## 🔹 Step 1: Check Your Git Status
Run:
```bash
git status
```
This will show any new or modified files that need to be committed.

---

## 🔹 Step 2: Add All Files for Commit
To stage all files, run:
```bash
git add .
```
If you only want to add specific files, use:
```bash
git add filename.txt
```

---

## 🔹 Step 3: Commit the Changes
Now commit the files with a message:
```bash
git commit -m "Initial commit"
```

---

## 🔹 Step 4: Check If a Remote Repository Is Linked
Run:
```bash
git remote -v
```
- If no remote is set, add your GitHub repository:
  ```bash
  git remote add origin https://github.com/Devops-Preparation-2025/Devops_Preparation.git
  ```

---

## 🔹 Step 5: Push Your Files to GitHub
Now, push your files to the **main** branch:
```bash
git push -u origin main
```
If you are using a different branch (e.g., `Project-1`):
```bash
git push -u origin Project-1
```

---

## 🔹 Step 6: Verify on GitHub
Go to your GitHub repository and check if the files are uploaded:  
🔗 [GitHub Repository](https://github.com/Devops-Preparation-2025/Devops_Preparation)

---

## 🎯 Now Your Files Are on GitHub!
If you face any errors, check:
```bash
git status
git remote -v
git branch -r
```
Let me know if you need any help! 🚀



----------------------------------------------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------------------------------------------

# 🚀 Set Up SSH Authentication for GitHub

To avoid entering credentials (username & password) every time you push code, use **SSH authentication** instead of HTTPS.

---

## ✅ Step 1: Check If You Already Have SSH Keys

```bash
ls -al ~/.ssh
```

- If you see files like **id\_rsa** and **id\_rsa.pub**, you already have an SSH key.
- If not, generate a new one in the next step.

---

## ✅ Step 2: Generate a New SSH Key (If Needed)

```bash
ssh-keygen -t rsa -b 4096 -C "your-email@example.com"
```

- **Press Enter** to save the key in the default location (`~/.ssh/id_rsa`).
- **Enter a passphrase** (or press Enter to leave it empty).

---

## ✅ Step 3: Add Your SSH Key to the SSH Agent

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

---

## ✅ Step 4: Add Your SSH Key to GitHub

1. Copy your **public** SSH key:
   ```bash
   cat ~/.ssh/id_rsa.pub
   ```
2. **Copy the entire output** and go to GitHub:
   - **GitHub → Settings → SSH and GPG keys → New SSH Key**
   - **Paste** the key and click **Add SSH Key**.

---

## ✅ Step 5: Change Git Remote URL to SSH

Check your current remote URL:

```bash
git remote -v
```

If the URL starts with HTTPS (`https://github.com/...`), change it to SSH:

```bash
git remote set-url origin git@github.com:Devops-Preparation-2025/Devops_Preparation.git
```

---

## ✅ Step 6: Test the SSH Connection

```bash
ssh -T git@github.com
```

If successful, you’ll see:

```
Hi Devops-Preparation-2025! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## ✅ Step 7: Push Your Code Without Credentials

Now push your code as usual:

```bash
git push origin main
```

You **won't need to enter your username & password anymore!** 🎉

---

## 🎯 Now Try Pushing Again!

If you face any issues, check the steps again or troubleshoot using:

```bash
git status
git remote -v
```

Happy Coding! 🚀


