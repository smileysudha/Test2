# MCP Practical Lab - Build Your First AI-Powered Tool
## Extend GitHub Copilot with Custom Capabilities

---

## 📋 Lab Overview
**Duration**: 60-75 minutes  
**Difficulty**: Beginner to Intermediate  
**Prerequisites**: 
- ✅ VS Code installed
- ✅ GitHub Copilot subscription
- ✅ Node.js 18+ installed
- ✅ Basic JavaScript/TypeScript knowledge

---

## 🎯 What You'll Build
A **Weather MCP Server** that lets GitHub Copilot fetch real-time weather data through simple conversation!

**Before MCP:**
```
You: "What's the weather in Paris?"
Copilot: "I can't check live weather, but here's code to call an API..."
```

**After MCP:**
```
You: "What's the weather in Paris?"
Copilot: *Actually fetches weather* "It's 18°C and Sunny in Paris!"
```

---

## 🚀 Quick Start: Verify Prerequisites

Open PowerShell and run:

```powershell
# Verify Node.js (need 18+)
node --version

# Verify npm
npm --version

# Check Copilot is installed


```

**✓ You should see version numbers**. If not, install Node.js from https://nodejs.org

---

## Part 1: Setup Your MCP Server 

### Step 1: Create Project Structure

```powershell
# Navigate to your workspace

# Create and enter project directory
mkdir mcp-weather-server
cd mcp-weather-server

# Initialize Node.js project
npm init -y

# Install dependencies
npm install @modelcontextprotocol/sdk
npm install -D typescript @types/node tsx
```

### Step 2: Configure TypeScript

Create `tsconfig.json`:
```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "Node16",
    "moduleResolution": "Node16",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  },
  "include": ["src/**/*"]
}
```

### Step 3: Update package.json

Open `package.json` and add:
```json
{
  "type": "module",
  "scripts": {
    "build": "tsc",
    "start": "node dist/index.js"
  },
  "devDependencies": {
    "@types/node": "^25.5.0",
    "typescript": "^6.0.2"
  },
  "dependencies": {
    "@modelcontextprotocol/sdk": "^1.28.0"
  }
}

```

---

## Part 2: Build Your Weather Server

### Step 4: Create the MCP Server

Create `src/index.ts` with this complete code:

```typescript
#!/usr/bin/env node
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { CallToolRequestSchema, ListToolsRequestSchema } from "@modelcontextprotocol/sdk/types.js";

const server = new Server(
  { name: "weather-server", version: "1.0.0" },
  { capabilities: { tools: {} } }
);

// Define available tools
server.setRequestHandler(ListToolsRequestSchema, async () => ({
  tools: [
    {
      name: "get_weather",
      description: "Get current weather for any city worldwide",
      inputSchema: {
        type: "object",
        properties: {
          city: { type: "string", description: "City name (e.g., London, Tokyo, Paris)" }
        },
        required: ["city"]
      }
    },
    {
      name: "get_forecast",
      description: "Get 5-day weather forecast for a city",
      inputSchema: {
        type: "object",
        properties: {
          city: { type: "string", description: "City name" }
        },
        required: ["city"]
      }
    }
  ]
}));

// Handle tool calls
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  const { name, arguments: args } = request.params;
  const city = (args?.city as string) || "Unknown";

  if (name === "get_weather") {
    const weather = {
      city,
      temperature: Math.floor(Math.random() * 20) + 10,
      condition: ["Sunny", "Cloudy", "Rainy", "Partly Cloudy"][Math.floor(Math.random() * 4)],
      humidity: Math.floor(Math.random() * 40) + 40,
      windSpeed: Math.floor(Math.random() * 20) + 5,
      feelsLike: Math.floor(Math.random() * 20) + 8
    };
    return {
      content: [{ type: "text", text: JSON.stringify(weather, null, 2) }]
    };
  }

  if (name === "get_forecast") {
    const forecast = Array.from({ length: 5 }, (_, i) => ({
      day: new Date(Date.now() + i * 86400000).toLocaleDateString('en-US', { weekday: 'long' }),
      high: Math.floor(Math.random() * 15) + 15,
      low: Math.floor(Math.random() * 10) + 5,
      condition: ["Sunny", "Cloudy", "Rainy", "Partly Cloudy"][Math.floor(Math.random() * 4)]
    }));
    return {
      content: [{ type: "text", text: JSON.stringify({ city, forecast }, null, 2) }]
    };
  }

  throw new Error(`Unknown tool: ${name}`);
});

// Start server
async function main() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
  console.error("Weather MCP Server running!");
}

main().catch((error) => {
  console.error("Error:", error);
  process.exit(1);
});
```

### Step 5: Build the Server

Make sure you're in the project directory

Compile TypeScript to JavaScript
```powershell
npm run build
```

**✓ Success!** You should see a `dist` folder with `index.js`

---

## Part 3: Connect to VS Code

### Step 6: Configure VS Code

Press `Ctrl+Shift+P` and type: **"Preferences: Open User Settings (JSON)"**

Add this configuration (update the path if your **MCP Server** is different):

```json
{
  "github.copilot.chat.mcp.servers": {
    "weather": {
      "command": "node",
      "args": [
        "C:\\Users\\SirinAli\\OneDrive - CloudThat\\Desktop\\test4\\mcp-weather-server\\dist\\index.js"
      ],
      "enabled": true
    }
  }
}
```

**💡 Pro Tip**: Make sure the path uses double backslashes `\\` on Windows!

### Step 7: Reload VS Code

Press `Ctrl+Shift+P` and run: **"Developer: Reload Window"**

---

## Part 4: Test Your MCP Server! 

### Step 8: Verify It's Working

Open Copilot Chat (`Ctrl+Shift+I`) and ask:

```
List all MCP servers currently available to you
```

**Expected**: You should see "weather" in the list!

### Step 9: Try These Commands

**Test 1 - Simple Weather Query:**
```
What's the weather in London?
```

**Test 2 - Forecast:**
```
Give me a 5-day forecast for Tokyo
```

**Test 3 - Comparison:**
```
Compare the weather in New York and Los Angeles
```

**Test 4 - Planning:**
```
Should I bring an umbrella to Paris this week?
```

**Test 5 - Multiple Cities:**
```
What's the weather like in Berlin, Madrid, and Rome?
```

### 🎉 What's Happening?

- Copilot **automatically detects** when to use your weather tool
- It **calls your MCP server** in the background
- Your server returns data
- Copilot presents it conversationally!

---

## Part 5: Understanding the Code (10 minutes)

### Key Components Explained

**1. Server Definition**
```typescript
const server = new Server(
  { name: "weather-server", version: "1.0.0" },
  { capabilities: { tools: {} } }
);
```
Creates your MCP server with a name and declares it has "tools" capability.

**2. Tool Registration**
```typescript
server.setRequestHandler(ListToolsRequestSchema, async () => ({
  tools: [/* your tools */]
}));
```
Tells Copilot what tools are available. The `description` field is crucial - it helps Copilot know WHEN to use each tool!

**3. Tool Execution**
```typescript
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  // Handle the actual tool logic
});
```
Executes when Copilot calls a tool. Your code runs, returns data to Copilot.

**4. Communication**
```typescript
const transport = new StdioServerTransport();
```
Uses stdin/stdout for communication (like a command-line program).

---

## 🎯 Challenge Exercises

### Challenge 1: Add a New Tool (Easy)
Add a `get_temperature` tool that only returns temperature (no other weather data).

<details>
<summary>💡 Hint</summary>

Add a new tool definition in `ListToolsRequestSchema` and handle it in `CallToolRequestSchema`.
</details>

### Challenge 2: Add City Validation (Medium)
Make your server reject invalid city names (e.g., cities with numbers or special characters).

<details>
<summary>💡 Hint</summary>

```typescript
if (!/^[a-zA-Z\s]+$/.test(city)) {
  throw new Error("Invalid city name");
}
```
</details>

### Challenge 3: Add Real Weather API (Advanced)
Replace mock data with a real weather API like OpenWeatherMap.

<details>
<summary>💡 Hint</summary>

1. Sign up at https://openweathermap.org/api
2. Install axios: `npm install axios`
3. Make HTTP requests in your tool handlers
</details>

---

## 🐛 Troubleshooting

| Problem | Solution |
|---------|----------|
| **"Server not found"** | Check the path in settings.json matches your actual folder |
| **"Build failed"** | Make sure TypeScript is installed: `npm install -D typescript` |
| **"Tools not called"** | Reload VS Code window (Ctrl+Shift+P → Reload Window) |
| **"Permission denied"** | Run VS Code as administrator or check file permissions |
| **Copilot doesn't use tools** | Make tool descriptions more specific and clear |

### View Server Logs

1. Press `Ctrl+Shift+U` (Output panel)
2. Select **"GitHub Copilot Chat: MCP Servers"** from dropdown
3. Look for errors or connection messages

---

### Ideas for Your Own MCP Servers:

**For Developers:**
- 🗄️ Database query server (PostgreSQL, MongoDB)
- 🐙 Git operations server (commit history, branch info)
- 📦 Package manager server (npm, pip search)
- 🧪 Test runner server (run Jest tests, get coverage)

**For Data/Analytics:**
- 📊 CSV/Excel analyzer
- 🔢 Statistical calculations
- 📈 Data visualization generator
- 🤖 ML model predictor

**For DevOps:**
- 🐳 Docker container manager
- ☸️ Kubernetes cluster info
- 📊 Log analyzer
- 🔔 Alert monitor

**For Business:**
- 📧 Email sender (via SMTP)
- 📅 Calendar integration
- 💬 Slack/Teams notifier
- 📝 Document generator

### Resources:

- **Official MCP Docs**: https://modelcontextprotocol.io
- **MCP SDK GitHub**: https://github.com/modelcontextprotocol/typescript-sdk
- **Example Servers**: https://github.com/modelcontextprotocol/servers
- **Community**: https://github.com/modelcontextprotocol/mcp/discussions

---

## 🎓 Quick Quiz

Test your understanding:

1. **What does MCP stand for?**
   <details><summary>Answer</summary>Model Context Protocol</details>

2. **What are the two main request handlers in an MCP server?**
   <details><summary>Answer</summary>ListToolsRequestSchema and CallToolRequestSchema</details>

3. **Why is the tool `description` field important?**
   <details><summary>Answer</summary>It helps Copilot understand WHEN to use the tool</details>

4. **What communication method does MCP use?**
   <details><summary>Answer</summary>stdio (standard input/output)</details>

5. **Where do you configure MCP servers in VS Code?**
   <details><summary>Answer</summary>settings.json under "github.copilot.chat.mcp.servers"</details>



---

## 🎉 Congratulations!
