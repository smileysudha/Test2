#  GitHub Copilot Coding Agent Lab

## Lab Objective

By the end of this lab, learners will:

- ✅ Understand the **Coding Agent workflow**
- ✅ Use **GitHub Issues** to trigger AI-driven development
- ✅ Observe **autonomous Pull Request creation**
- ✅ Compare **Coding Agent vs Reviewer** roles

---

## 🧰 Prerequisites

Before starting, ensure you have:

| Requirement | Description |
|------------|-------------|
| 🔑 GitHub Account | Active GitHub account with appropriate permissions |
| 🤖 GitHub Copilot | Active subscription with Agent feature enabled |
| 📚 GitHub Knowledge | Basic understanding of repositories, issues, and PRs |

---

## 📋 Lab Steps

### 🧱 STEP 1: Create Repository

1. Navigate to **GitHub** → Click **New Repository**
2. Configure repository:
   - **Name:** `copilot-coding-agent-lab`
   - **Visibility:** ✅ Public
   - **Initialize:** 
     - ✅ Add README file
     - ✅ Add .gitignore → Node
3. Click **Create Repository**

> 💡 **Tip:** Public repositories work best for this lab demonstration

---

### 📁 STEP 2: Add Starter Application

**Option A:** Open web editor (press `.` in your repo)  
**Option B:** Clone locally and edit

#### Create `app.js`

```javascript
const express = require('express');
const app = express();

app.use(express.json());

app.get('/', (req, res) => {
  res.send('Hello World');
});

app.listen(3000, () => console.log('Server running'));
```

#### Create `package.json`

```json
{
  "name": "copilot-coding-agent-lab",
  "version": "1.0.0",
  "main": "app.js",
  "scripts": {
    "start": "node app.js",
    "test": "echo \"No tests yet\""
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

**Commit changes** with message: `Initial Express application setup`

---

### 🐛 STEP 3: Create Issue (Trigger Point)

This is where the magic begins! 🪄

1. Navigate to **Issues** → Click **New Issue**
2. **Title:** `Add JWT Authentication`
3. **Description:**

```markdown
- Implement JWT authentication
- Create login API
- Protect routes using middleware
- Add unit tests using Jest
- Update dependencies if required
```

4. Click **Create Issue**

> 🎯 **Key Concept:** The quality of your issue description directly impacts the AI's output

---

### 🤖 STEP 4: Assign Issue to Copilot

**This step activates the Coding Agent** 🔥

1. Inside the issue page
2. Click **Assignees** (right sidebar)
3. Select **GitHub Copilot**

> ⚡ **What Happens:** This triggers the Coding Agent to start working on your issue asynchronously

---

### ⚙️ STEP 5: Observe Agent Execution

1. Navigate to **Actions** tab
2. You will see:
   - ✅ Workflow triggered
   - ✅ Agent processing task
   - ✅ Real-time logs

*_"Now AI is working in the background like a developer. It's reading the codebase, planning changes, and implementing them autonomously."_*

---

### ⏳ STEP 6: Wait for Completion

**Expected Duration:** ~2–5 minutes

During this time, the Agent will:

| Task | Description |
|------|-------------|
| 🌿 Create Branch | New feature branch for changes |
| 🔐 Add Auth Logic | JWT implementation |
| 🛡️ Add Middleware | Route protection logic |
| ✅ Add Tests | Jest test cases |
| 📦 Update Dependencies | Add required packages |


---

### 🔀 STEP 7: Pull Request Auto-Creation

1. Navigate to **Pull Requests** tab
2. **You will see:** PR automatically created by Copilot

**🎯 Success Indicator:**  
- PR title matches issue topic
- Description includes implementation details
- Commits show structured changes

> 💡 **This confirms:** Your Coding Agent is fully operational!

---

### 🔍 STEP 8: Review the Pull Request

Open the PR and review:

#### Files Added/Modified
- `auth/` directory with authentication logic
- `middleware/auth.js` for route protection
- `tests/` directory with Jest test cases
- `package.json` updated dependencies

#### Code Quality Check
- ✅ Clean, readable code
- ✅ Proper error handling
- ✅ Security best practices
- ✅ Test coverage

*_"Notice how the AI organized the code into logical modules, added error handling, and wrote comprehensive tests—all without human intervention."_*

---

### 🧠 STEP 9: Add Copilot Reviewer (Optional Demo)

**Demonstrate the difference between two Copilot roles:**

1. In the PR, click **Reviewers**
2. Add **GitHub Copilot** as a reviewer
3. Observe review comments and suggestions

#### 🔄 Coding Agent vs Reviewer

| Feature | Coding Agent | Copilot Reviewer |
|---------|-------------|------------------|
| **Trigger** | Assigned to issue | Added as PR reviewer |
| **Action** | Writes code | Reviews code |
| **Output** | Creates PR | Provides feedback |
| **Purpose** | Implementation | Quality assurance |

---

### ✅ STEP 10: Merge Pull Request

1. Review all changes one final time
2. Click **Merge Pull Request**
3. Select merge strategy (Squash recommended)
4. Confirm merge

**🎉 Congratulations!** Your AI developed and deployed a feature!

---

## 🎯 Expected Outcomes

After completing this lab, you should have:

- ✅ JWT authentication fully implemented
- ✅ Protected routes with middleware
- ✅ Comprehensive test suite
- ✅ Updated dependencies
- ✅ Complete PR workflow experience
- ✅ Understanding of autonomous AI development

---

## ⚠️ Troubleshooting Guide

### ❌ Copilot Not Visible in Assignees

**Possible Causes:**
- Subscription issue
- Feature not enabled for account
- Repository settings

**Solutions:**
1. Verify GitHub Copilot subscription status
2. Check feature availability in settings
3. Ensure repository has Copilot access

---

### ❌ No Pull Request Created

**Possible Causes:**
- Workflow failed
- Permission issues
- Agent couldn't complete task

**Solutions:**
1. Check **Actions** tab for error logs
2. Verify repository permissions
3. Review issue description clarity

---

### ❌ Poor Quality Output

**Root Cause:** Vague issue description

**Solution:** 
> 📝 **Rule:** Clear, detailed instructions = Better AI results

**Example - Poor Description:**
```
Add auth
```

**Example - Good Description:**
```
- Implement JWT authentication with token expiration
- Create POST /api/login endpoint accepting email/password
- Add middleware to verify JWT from Authorization header
- Protect all /api/users/* routes
- Add unit tests with Jest
- Use bcrypt for password hashing
```

---

### ❌ Agent Stops Mid-Process

**Solutions:**
1. Check GitHub Actions quota
2. Verify no network issues
3. Re-assign issue to Copilot

---

## 🔗 Resources

- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [GitHub Copilot coding agent](https://docs.github.com/en/copilot/concepts/agents/coding-agent/about-coding-agent)

---

## 💡 Key Takeaways

> **🎯 The Future of Development:** GitHub Copilot Coding Agent demonstrates how AI can handle entire features autonomously—from understanding requirements to implementing, testing, and submitting code for review.

1. **Clear Communication:** Precise issue descriptions yield better results
2. **Asynchronous Workflow:** AI works while you focus on other tasks
3. **Quality Assurance:** Always review AI code before merging
4. **Iterative Improvement:** Refine your prompts based on results
