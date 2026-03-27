# Complete Setup Guide: GitHub Copilot in VS Code (Including Prerequisites)

This is a simple, end-to-end guide to prepare your system and set up Copilot for trainings or personal use.

---

## Step 0: Prerequisites Setup

### 0.1 Create a GitHub Account

**Steps:**
1. Go to: https://github.com/signup
2. Enter email, password, username
3. Verify your account

---

### 0.2 Enable GitHub Copilot

**Steps:**
1. Go to: https://github.com/settings/copilot
2. Click Start free trial or enable via organization
3. Accept terms

---

### 0.3 Install Visual Studio Code

**Download:**  
https://code.visualstudio.com/download

**Steps:**
1. Download for your OS
2. Install
3. Open VS Code

---

### 0.4 Ensure Internet Connectivity

**Copilot requires:**
- Continuous internet access
- GitHub authentication

---

## Step 1: Install Copilot Extensions

### 👉 Open Extensions Panel

**Shortcut:**  
`Ctrl + Shift + X`

---

### Install:

**1. GitHub Copilot**  
https://marketplace.visualstudio.com/items?itemName=GitHub.copilot

---

**2. GitHub Copilot Chat**  
https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat

---

## Step 2: Sign in to GitHub

**Steps:**
1. Click Accounts (bottom-left corner)
2. Select Sign in with GitHub
3. Browser opens → login → authorize
4. Return to VS Code

---

### Verify
- GitHub username visible
- Copilot activated

---

## ⚙️ Step 3: Enable Copilot Features

**Open Settings:**  
`Ctrl + ,`

**Search:** Copilot

**Enable:**
- Inline Suggestions 
- Copilot Chat 
- Auto Completions 

---

## Step 4: Open Copilot Chat

**Shortcut:**  
`Ctrl + Alt + I`

---

**Test Prompt:**  
What is a REST API?

---

## Step 5: Enable Agent Mode

**Inside Copilot Chat:**
1. Locate mode selector
2. Switch:  
   Ask → Agent

---

## Step 6: Verify Copilot is Working

**Create any file:**

**Example:**  
`test.js`

**Type:**
```javascript
function add(a, b) {
```

✅ Copilot suggests full function  
**Press Tab to accept**
