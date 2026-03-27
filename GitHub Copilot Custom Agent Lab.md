# 🚀 GitHub Copilot Custom Agent Lab

---

## 🎯 Lab Objective

By the end of this lab, learners will:

- ✅ Create a **Custom Agent** in GitHub Copilot
- ✅ Define **custom instructions and rules**
- ✅ Use the custom agent in **Agent Mode**
- ✅ Compare behavior between **default vs custom agent**
- ✅ Understand how to **tailor AI behavior** to project needs

---

## 🧰 Prerequisites

Before starting, ensure you have:

| Requirement | Description |
|------------|-------------|
| 💻 VS Code | Latest version installed |
| 🤖 GitHub Copilot | Active subscription and extension installed |
| 💬 Copilot Chat | Agent Mode feature enabled |
| 📦 Node.js | Installed (version 14+ recommended) |
| 📚 Basic Knowledge | Understanding of JavaScript/Node.js |

> 💡 **Note:** We'll use a *Node.js* Express app for this lab, but the concepts apply to any language

---

## 📋 Lab Steps

### STEP 1: Set Up Project

#### Create New Project

Open terminal and run:

```bash
mkdir custom-agent-lab
cd custom-agent-lab
```
```bash
npm init -y
npm install express
```

#### Create `app.js`

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello World');
});

app.listen(3000);
```

**✅ Verify:** Run `node app.js` and check http://localhost:3000

---

### 🤖 STEP 2: Open Copilot Chat (Agent Mode)

1. In VS Code, open **Copilot Chat**:
   - Click Copilot icon in sidebar, OR
   - Press `Ctrl + Shift + I` (Windows/Linux) or `Cmd + Shift + I` (Mac)

2. **Switch to Agent Mode:**
   - Look for the mode selector at the top
   - Ensure you're in **Agent Mode** (not Ask/Edit)

> ⚠️ **Important:** Agent Mode is required for custom agents to work

---

### ⚙️ STEP 3: Create Custom Agent

There are two ways to create a custom agent:

#### Method 1: Via Copilot Chat UI

1. Click the **Agent dropdown** in Copilot Chat
2. Select **"Create New Agent"** or **"Custom Agent"**
3. Follow the prompts

#### Method 2: Via File (Recommended)

1. In your workspace, create folder structure:
   ```
   .github/
     └── agents/
   ```

2. Create file: `.github/agents/backend-agent.agent.md`

---

### ✍️ STEP 4: Define Agent Instructions

In the custom agent file, paste these instructions:

```markdown
---
name: Backend-Agent
description: Senior Node.js backend developer specializing in clean architecture
---

You are a senior Node.js backend developer.

Follow these rules:
- Use proper folder structure (routes, controllers, services)
- Always add input validation
- Use middleware for error handling
- Write clean and modular code
- Add comments for explanation
- Suggest unit tests using Jest
```

**Save the file** with `Ctrl + S`

---

### 🧠 STEP 5: Baseline Test (Default Agent)

Before using your custom agent, establish a baseline:

1. In Copilot Chat, ensure **default agent** is selected
2. Type this prompt:

```
Create a login API
```

3. **Observe the output:**
   - ❌ Basic code structure
   - ❌ No folder organization
   - ❌ Minimal or no validation
   - ❌ No error handling middleware
   - ❌ No test suggestions

**📢 Instructor Script:**  
_"This is how generic Copilot responds—functional but not following best practices."_

---

### 🔥 STEP 6: Switch to Custom Agent

1. In Copilot Chat, click the **Agent dropdown**
2. Select **Backend-Agent** (your custom agent)
3. You should see the agent name displayed

> ✅ **Confirmation:** The chat interface should indicate the custom agent is active

---

### 🚀 STEP 7: Run Same Prompt with Custom Agent

Now, with your custom agent active, type the **exact same prompt**:

```
Create a login API
```

### 🎯 Observe the Difference

The custom agent will now provide:

| Feature | Default Agent | Custom Agent |
|---------|--------------|--------------|
| **Structure** | Single file | Organized folders |
| **Validation** | Minimal | Express-validator middleware |
| **Error Handling** | Basic try-catch | Dedicated error middleware |
| **Code Quality** | Functional | Clean, modular, commented |
| **Testing** | Not mentioned | Jest test suggestions |
| **Architecture** | Monolithic | Layered (routes/controllers/services) |

#### Expected Output Structure:

```
routes/
  └── auth.routes.js
controllers/
  └── auth.controller.js
services/
  └── auth.service.js
middleware/
  └── validation.js
  └── errorHandler.js
tests/
  └── auth.test.js
```

*_"THIS is your WOW moment! Same prompt, completely different output. The agent now follows our defined best practices."_*

---

### 🔍 STEP 8: Advanced Architecture Demo

Test the agent's understanding with a complex request:

**Prompt:**
```
Refactor this app into a scalable backend structure
```

#### Expected Agent Behavior:

The custom agent will:
- ✅ Analyze current code structure
- ✅ Propose layered architecture
- ✅ Create controllers for business logic
- ✅ Create services for data operations
- ✅ Add route handlers
- ✅ Implement middleware chain
- ✅ Suggest configuration management
- ✅ Recommend environment variables

#### Example Output:

```javascript
// routes/auth.routes.js
const express = require('express');
const router = express.Router();
const authController = require('../controllers/auth.controller');
const { validateLogin } = require('../middleware/validation');

router.post('/login', validateLogin, authController.login);

module.exports = router;

// controllers/auth.controller.js
const authService = require('../services/auth.service');

exports.login = async (req, res, next) => {
  try {
    const { email, password } = req.body;
    const result = await authService.authenticateUser(email, password);
    res.json(result);
  } catch (error) {
    next(error);
  }
};

// services/auth.service.js
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');

exports.authenticateUser = async (email, password) => {
  // Authentication logic here
};
```

---

### 🧪 STEP 9: Add Testing

Push the agent further with testing requirements:

**Prompt:**
```
Add unit tests for login API using Jest
```

#### Expected Output:

```javascript
// tests/auth.test.js
const request = require('supertest');
const app = require('../app');

describe('Auth API', () => {
  describe('POST /api/login', () => {
    test('should return 400 if email is missing', async () => {
      const response = await request(app)
        .post('/api/login')
        .send({ password: 'test123' });
      
      expect(response.status).toBe(400);
      expect(response.body.message).toBe('Email is required');
    });

    test('should return 401 for invalid credentials', async () => {
      const response = await request(app)
        .post('/api/login')
        .send({ email: 'wrong@email.com', password: 'wrong' });
      
      expect(response.status).toBe(401);
    });

    test('should return token for valid credentials', async () => {
      const response = await request(app)
        .post('/api/login')
        .send({ email: 'admin@example.com', password: 'admin123' });
      
      expect(response.status).toBe(200);
      expect(response.body).toHaveProperty('token');
    });
  });
});
```

**Plus package.json update:**
```json
{
  "devDependencies": {
    "jest": "^29.0.0",
    "supertest": "^6.3.0"
  },
  "scripts": {
    "test": "jest"
  }
}
```

---

### STEP 10: Compare Side-by-Side

Create a comparison table for your students:

| Aspect | Default Copilot | Custom Backend-Agent |
|--------|----------------|---------------------|
| **Code Organization** | Single file | Multi-folder structure |
| **Input Validation** | None/Basic | express-validator middleware |
| **Error Handling** | try-catch only | Centralized middleware |
| **Security** | Basic | Bcrypt, JWT, best practices |
| **Testing** | Rarely suggested | Always includes tests |
| **Comments** | Minimal | Comprehensive |
| **Architecture** | Flat | Layered (MVC pattern) |

---

## 🎓 Learning Outcomes

### What Students Will Understand

1. **Customization Power**: AI behavior can be tailored to project/team standards
2. **Consistency**: Custom agents enforce coding standards across team
3. **Productivity**: Less refactoring when AI follows best practices from start
4. **Domain Expertise**: Agents can be specialized (backend, frontend, testing, etc.)

---

## 💡 Advanced Use Cases

### Create Multiple Agents

```
.github/agents/
  ├── backend-agent.agent.md       # API development
  ├── frontend-agent.agent.md      # React/UI components
  ├── testing-agent.agent.md       # Test-first development
  ├── security-agent.agent.md      # Security audits
  └── refactoring-agent.agent.md   # Code optimization
```

### Example: Frontend Agent

```markdown
---
name: Frontend-Agent
description: React specialist with modern best practices
---

You are a senior React developer.

Follow these rules:
- Use functional components with hooks
- Implement proper prop validation with PropTypes
- Add accessibility attributes (ARIA)
- Use CSS modules or styled-components
- Implement error boundaries
- Suggest React Testing Library tests
- Follow atomic design principles
```

---

## ⚠️ Troubleshooting

### ❌ Custom Agent Not Appearing

**Possible Causes:**
- File not in correct location
- YAML frontmatter syntax error
- VS Code restart needed

**Solutions:**
1. Verify file path: `.github/agents/*.agent.md`
2. Check YAML syntax (must have `---` delimiters)
3. Reload VS Code window: `Ctrl+Shift+P` → "Reload Window"

---

### ❌ Agent Not Following Instructions

**Possible Causes:**
- Instructions too vague
- Conflicting rules
- Not actually using custom agent

**Solutions:**
1. Make instructions specific and actionable
2. Verify correct agent is selected in dropdown
3. Use clear directive language ("Always...", "Never...", "Must...")

---

### ❌ Agent Behavior Inconsistent

**Root Cause:** Ambiguous instructions

**Solution - Good vs Bad Instructions:**

❌ **Bad:**
```
Write good code
```

✅ **Good:**
```
- Use descriptive variable names (camelCase)
- Add JSDoc comments for all functions
- Implement error handling in try-catch blocks
- Validate all inputs using express-validator
```

---

## 🚀 Bonus Challenges

### Challenge 1: Security Agent
Create an agent that focuses on security audits:
- Identifies SQL injection risks
- Checks for XSS vulnerabilities
- Validates authentication implementations
- Suggests security headers

### Challenge 2: Performance Agent
Create an agent specialized in optimization:
- Suggests caching strategies
- Identifies N+1 query problems
- Recommends database indexing
- Proposes async/await optimizations

### Challenge 3: Team-Specific Agent
Create an agent matching your organization's coding standards:
- Company naming conventions
- Specific libraries/frameworks
- Internal API patterns
- Documentation requirements

---

## 🎯 Real-World Applications

### Team Standardization
- **Problem:** Different developers write code in different styles
- **Solution:** Custom agent enforces team coding standards
- **Result:** Consistent codebase, easier code reviews

### Onboarding
- **Problem:** New developers unfamiliar with project patterns
- **Solution:** Custom agent guides them with project-specific practices
- **Result:** Faster ramp-up time

### Domain Expertise
- **Problem:** Team lacks expertise in specific area (e.g., security)
- **Solution:** Create specialized agent with security best practices
- **Result:** Better code quality without hiring specialists

---

## 📝 Best Practices for Agent Instructions

### 1. Be Specific
❌ "Write clean code"  
✅ "Use meaningful variable names, add JSDoc comments, extract functions > 20 lines"

### 2. Use Imperative Mood
❌ "It would be nice to have tests"  
✅ "Always create Jest test files with minimum 80% coverage"

### 3. Provide Context
❌ "Use proper structure"  
✅ "Organize code into: routes/ (endpoints), controllers/ (logic), services/ (data), middleware/ (cross-cutting)"

### 4. Set Boundaries
✅ "Never use var, only const/let"  
✅ "Always validate user input before processing"  
✅ "Must implement error handling middleware"

### 5. Include Examples
```markdown
Example folder structure:
src/
  ├── routes/
  ├── controllers/
  ├── services/
  └── middleware/
```

---

## Resources

- [GitHub Copilot Custom Agents](https://docs.github.com/en/copilot/concepts/agents/coding-agent/about-custom-agents)

---

## 💡 Key Takeaways

> **🎯 Power of Customization:** Custom agents transform GitHub Copilot from a generic assistant into a domain-specific expert that follows YOUR team's standards and practices.

1. **Consistency:** Enforces coding standards automatically
2. **Expertise:** Embeds best practices into every suggestion
3. **Productivity:** Reduces time spent on refactoring and code reviews
4. **Scalability:** One agent benefits entire team
5. **Flexibility:** Create different agents for different scenarios

---
