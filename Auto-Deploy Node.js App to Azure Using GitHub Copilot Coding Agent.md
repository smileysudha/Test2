# ☁️ Auto-Deploy Node.js App to Azure Using GitHub Copilot Coding Agent

> **Role:** Senior DevOps Engineer (Simulated via GitHub Copilot Agent)
> **Goal:** Use the GitHub Copilot Coding Agent to automatically generate a full CI/CD pipeline that deploys a Node.js app to Azure App Service when code is pushed to the `main` branch.

---

## 🎯 Objectives

By the end of this lab, you will be able to:

- Configure a GitHub repository secret for Azure authentication
- Create a GitHub Issue that acts as a task for the Copilot Coding Agent
- Let Copilot Agent generate all required files (app code, Bicep IaC, GitHub Actions workflow)
- Merge the Copilot-generated PR to trigger a live deployment on Azure
- Verify a running Node.js app on Azure App Service

---

## ✅ Prerequisites

| Requirement | Details |
|---|---|
| Azure Account | Active subscription with Contributor access |
| Azure Resource Group | `Sirin-RG` must already exist in **Central US** |
| GitHub Account | With a repository ready (public or private) |
| GitHub Copilot | License with **Coding Agent** (Workspace) enabled |
| Azure CLI | Installed locally — only needed to generate credentials |

---

## Step 1: Create a GitHub Repository

Create a GitHub repository where the Copilot Coding Agent will generate code and the deployment pipeline.


1. Go to [GitHub](https://github.com)
2. Click **+** (top-right corner) → **New repository**
3. Fill in the repository details:

   | Field | Value |
   |---|---|
   | **Repository name** | `sldc-repo` |
   | **Description** | `Copilot Agent Azure Deployment Lab` |
   | **Visibility** | 👉 **Public** (recommended for demo) |
   | **Add README.md** | ✅ Check this |
   | **Add .gitignore** | ❌ Leave unchecked (Copilot will generate) |
   | **Add license** | ❌ Leave unchecked (optional) |

4. Click **Create repository**
 
## Step 2: Create the GitHub Secret

The GitHub Actions workflow authenticates to Azure using a secret named `AZURE_CREDENTIALS`. You must create this once before running the lab.


### Add the Secret to GitHub

1. Go to your GitHub repository
2. Click **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Set:
   - **Name:** `AZURE_CREDENTIALS`
   - **Value:** Paste the full JSON 
5. Click **Add secret**

### ✅ Verification

| Check | Expected |
|---|---|
| Secret name | Exactly `AZURE_CREDENTIALS` (case-sensitive) |
| Secret value | Full JSON object (not just the clientId) |
| Resource Group | `Sirin-RG` exists in **Central US** |

---

## Step 3: Verify the Resource Group Exists

Before running the lab, confirm `Sirin-RG` exists in the correct region.

```bash
az group show --name Sirin-RG --query "{Name:name, Location:location, State:properties.provisioningState}"
```

### ✅ Expected Output

```json
{
  "Location": "centralus",
  "Name": "Sirin-RG",
  "State": "Succeeded"
}
```

> ❌ If the resource group does not exist, create it first:
> ```bash
> az group create --name Sirin-RG --location centralus
> ```

---


## Step 4: Create the GitHub Issue for the Copilot Coding Agent

This is the core step. You will create a GitHub Issue with an exact prompt that instructs the Copilot Coding Agent to generate all required files.

### 4.1 — Open Your Repository on GitHub

Navigate to your GitHub repository → click the **Issues** tab → click **New issue**.

### 4.2 — Copy the Exact Prompt Below

> ⚠️ Copy the issue body **exactly as written**. Do not modify the Coding Agent instructions.

---

### 📋 GitHub Issue — Copy This Exactly

**Issue Title:**

```
🚀 Task: Auto Deploy Node.js App to Azure 
```

**Issue Body:**

```

##  Task: Auto Deploy Node.js App to Azure 

###  Objective
Use GitHub Copilot Coding Agent to create a complete CI/CD pipeline that automatically deploys a Node.js app to Azure when code is pushed to main branch.

---

###  Application Requirements

1. Create a simple Node.js app using Express
2. App must:
   - Run on process.env.PORT
   - Return: "Hello from Sirin App Deployment"
3. Include:
   - index.js
   - package.json

---

###  Infrastructure Requirements (Bicep)

Deploy resources inside existing resource group:

- Resource Group: Sirin-RG (DO NOT CREATE NEW RG)
- Location: Central US

Resources to create:

1. App Service Plan:
   - Linux
   - SKU: B1

2. Azure Web App:
   - Name: sirin-app (use exactly this name)
   - Runtime: Node.js 18
   - Must be publicly accessible

---

###  CI/CD Requirements (GitHub Actions)

Create workflow file:

.github/workflows/deploy.yml

Workflow must:

1. Trigger:
   - On push to main branch

2. Steps:
   - Checkout code
   - Login to Azure using AZURE_CREDENTIALS
   - Deploy Bicep file to:
     Resource Group: Sirin-RG
     Location: Central US
   - Deploy Node.js app to Azure Web App (sirin-app)

3. Use:
   - azure/login action
   - az deployment group create
   - azure/webapps-deploy action

---

###  Available Secret

- AZURE_CREDENTIALS

---

###  Expected Output

- index.js
- package.json
- main.bicep
- .github/workflows/deploy.yml

---

### Important Constraints

- Do NOT create a new resource group
- Use exact app name: sirin-app
- Use region: Central US
- Ensure deployment works end-to-end

---

###  Final Expected Behavior

After PR merge:

1. Code pushed to main branch
2. GitHub Actions automatically runs
3. Azure login happens
4. Infrastructure deployed in Sirin-RG (Central US)
5. App deployed to sirin-app
6. App accessible via browser

---

###  Instruction to Copilot Agent

Act as a Senior DevOps Engineer.
Ensure all resources are correctly configured and deployment works without errors.

```

### ⚠️ VERY IMPORTANT (Don't Skip This)

👉 App name sirin-app MUST be globally unique

If it's already taken → deployment will FAIL ❌
Use this instead:
```
Use app name sirin-app-${uniqueString(resourceGroup().id)}
```

---

### Submit the Issue

1. Paste the title and body above
2. Click **Submit new issue**

---

## 🧠 Step 5: Assign the Issue to GitHub Copilot

After submitting the issue:

1. On the right-hand sidebar, find the **Assignees** section
2. Click the gear icon ⚙️
3. Search for and select **Copilot** (the GitHub Copilot Coding Agent)
4. Click away to save

> 💡 This tells the Copilot Coding Agent to pick up this issue as a work item.  
> Copilot will analyze the issue, generate a plan, and begin creating files automatically.

---

## 🔍 Step 6: Monitor Copilot Agent Progress

### Watch the Issue Activity

On the GitHub Issue page. You will see Copilot start posting comments with:

- A **plan** of the actions it will take
- Progress updates as it creates each file
- A link to the **Pull Request** it opens when done

### Review the Pull Request

When Copilot opens a PR, review the following files:

| File | Purpose |
|---|---|
| `index.js` | Express app returning "Hello from Sirin App Deployment" |
| `package.json` | Node.js dependencies and start script |
| `main.bicep` | Azure infrastructure definition (App Service Plan + Web App) |
| `.github/workflows/deploy.yml` | CI/CD pipeline triggered on push to `main` |

### What to Verify in Each File

**`index.js`** — confirm:
```js
const port = process.env.PORT || 3000;
// response includes "Hello from Sirin App Deployment"
```

**`main.bicep`** — confirm:
```bicep
// No new resource group definition
// App Service Plan: Linux, SKU B1
// Web App name: sirin-app (or sirin-app-${uniqueString(...)})
// location: 'centralus'
```

**`.github/workflows/deploy.yml`** — confirm:
```yaml
on:
  push:
    branches: [main]
# steps include: azure/login, az deployment group create, azure/webapps-deploy
```

---

##  Step 7: Merge the Pull Request

Once you are satisfied with the Copilot-generated files:

1. Click **Merge pull request** on the PR page
2. Click **Confirm merge**

> ⚡ This push to `main` immediately triggers the GitHub Actions workflow.

---

##  Step 8: Monitor the GitHub Actions Workflow

1. Go to your repository → **Actions** tab
2. Click the running workflow named based on your deploy file
3. Watch each step execute in real time:

| Step | Expected Result |
|---|---|
| Checkout | ✅ Code checked out |
| Azure Login | ✅ Logged in using `AZURE_CREDENTIALS` |
| Deploy Bicep | ✅ App Service Plan and Web App created in `Sirin-RG` |
| Deploy App | ✅ Node.js app deployed to `sirin-app` |

> 💡 If any step fails, click on it to expand the logs and read the error message.  
> Common issues: incorrect secret format, app name already taken, resource group not found.

---

## 🌐 Step 9: Verify the Deployed Application

Once the workflow completes successfully, open the Azure Portal, navigate to the Sirin-RG resource group, select the deployed web app, and browse it:

```
https://sirin-app.azurewebsites.net
```

### ✅ Expected Output

```
Hello from Sirin App Deployment
```

> If you used the dynamic name backup, find the actual URL by running:
> ```bash
> az webapp show --resource-group Sirin-RG --name <actual-app-name> --query defaultHostName
> ```

---

## 🔧 Troubleshooting Guide

| Problem | Likely Cause | Fix |
|---|---|---|
| `az deployment group create` fails | App name already taken globally | Use `sirin-app-${uniqueString(resourceGroup().id)}` in Bicep |
| `azure/login` step fails | Wrong secret format or name | Re-check `AZURE_CREDENTIALS` — must be full JSON |
| Resource Group not found | RG doesn't exist or wrong name | Run `az group show --name Sirin-RG` |
| App not reachable after deploy | Startup command missing | Ensure `package.json` has `"start": "node index.js"` |
| Bicep creates a new RG | Issue prompt not followed exactly | Review `main.bicep` — ensure no `resourceGroup` resource definition |

---

## 🧹 Cleanup

When you are done with the lab, remove all Azure resources to avoid charges:

```bash
# Delete the Web App
az webapp delete --resource-group Sirin-RG --name sirin-app

# Delete the App Service Plan (name may vary — check portal)
az appservice plan delete --resource-group Sirin-RG --name <plan-name> --yes
```

> ⚠️ Do **not** delete `Sirin-RG` itself unless you are sure no other resources are using it.

---

## 💡 Key Concepts Learned

| Concept | Description |
|---|---|
| **Copilot Coding Agent** | Automates code and infrastructure generation from a GitHub Issue |
| **Bicep** | Azure-native IaC language — simpler alternative to ARM templates |
| **GitHub Actions** | CI/CD platform built into GitHub — triggers on events like `push` |
| **Azure App Service** | Managed PaaS platform for running web apps without managing VMs |
| **Global Uniqueness** | Azure resources like Web Apps need unique names across all tenants |
| **Service Principal** | Azure identity used by automation tools to authenticate securely |

---

> **Lab complete!** You've used GitHub Copilot as a fully autonomous DevOps engineer — from infrastructure code to CI/CD pipeline — all triggered by a single GitHub Issue. 🎉
