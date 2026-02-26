# Lab 2 Revision: Detailed Execution Guide for Implementation

**Document Date:** February 26, 2026  
**Purpose:** Step-by-step guidance for implementing the IMPLEMENTATION_PLAN.md  
**Audience:** Workshop facilitators and content developers working on Lab 2 enhancement

---

## Phase 1: Preparation & Setup (Before Starting Content Changes)

### Step 1.1: Environment Preparation
Before making any changes to lab.md, prepare your local environment:

```bash
# Create a test workspace for taking screenshots
mkdir -p ~/workshops-pbig-lab2-test
cd ~/workshops-pbig-lab2-test

# Ensure Python 3.9+ is installed
python --version

# Create virtual environment for testing
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install fabric-cicd to test code examples
pip install fabric-cicd

# Verify installation
python -c "import fabric_cicd; print(fabric_cicd.__version__)"
```

### Step 1.2: Gather Technical Specifications
Collect the following information before implementation:

**For deploy.py examples:**
- [ ] Confirm Python compatibility versions (docs say 3.9-3.12)
- [ ] Test InteractiveBrowserCredential flow locally
- [ ] Verify AzureCliCredential requirements
- [ ] Document FabricWorkspace class parameters
- [ ] Test publish_all_items() function behavior

**For GitHub Actions:**
- [ ] Verify latest actions/checkout@v4 (or current version)
- [ ] Verify latest actions/setup-python@v4 (or current version)
- [ ] Document workflow trigger options
- [ ] Test workflow_dispatch manual trigger capability

**For Fabric portal:**
- [ ] Screenshots of workspace ID location (current UI)
- [ ] Service principal addition procedure (current UI)
- [ ] Workspace settings access path (current UI)

### Step 1.3: Create Implementation Checklist in Repository

Create a tracking file at `.labs/lab2/REVISION_CHECKLIST.md`:

```markdown
# Lab 2 Revision Tracking Checklist

Implementation Date: [TBD]
Estimated Completion: [TBD]

## Content Changes
- [ ] Section 0: Add fabric-cicd prerequisites
- [ ] Section 1: NEW - fabric-cicd introduction (created)
- [ ] Section 2: Revise GitHub repository setup
- [ ] Section 3: NEW - Local manual deployment (created)
- [ ] Section 4: GitHub Actions automation setup
- [ ] Section 5: First automated deployment
- [ ] Section 6: Branch protection & PR workflow

## Screenshots Captured
- [ ] deploy.py in VS Code
- [ ] Fabric workspace ID location
- [ ] Successful local deployment terminal
- [ ] GitHub Actions deploy.yml workflow
- [ ] Workflow run in progress
- [ ] Workflow run completed

## Testing & Validation
- [ ] Full walkthrough completed
- [ ] All code examples tested
- [ ] All links verified
- [ ] Peer review completed
- [ ] Grammar/style check completed

## Status: [ ] NOT STARTED [ ] IN PROGRESS [ ] COMPLETE
```

---

## Phase 2: Detailed Content Creation Guide

### Section 2.1: Creating New Section 1 (fabric-cicd Introduction)

**File Location:** `.labs/lab2/lab.md` - Insert after Section 0 (Setup)

**Content Structure:**
```markdown
## 1. Understanding fabric-cicd: The Deployment Tool

### 1.1 What is fabric-cicd?
- Brief explanation (3-4 paragraphs)
- Emphasize it's Microsoft's recommended approach
- Highlight key advantages (see Appendix B of plan for talking points)

### 1.2 Why fabric-cicd over other tools?
- Comparison table or bullet list
- Link to fabric-cicd documentation

### 1.3 Installation
- Step 1: Verify Python version (3.9-3.12)
  - Command: python --version
  - Show expected output
- Step 2: Install via pip
  - Command: pip install fabric-cicd
  - Show installation output
- Step 3: Verify installation
  - Command: python -c "from fabric_cicd import FabricWorkspace; print('✅ fabric-cicd installed')"
  - Show expected output

### 1.4 How fabric-cicd fits into your deployment pipeline
- Diagram: Local Machine → deploy.py → Fabric
- Mention: Same tool runs locally AND via GitHub Actions
- Foreshadow: Next section, we'll create deploy.py
```

**Estimated Length:** 800-1000 words  
**Screenshots:** None (conceptual only)  
**Time to Create:** 1-2 hours

**Key Points to Research:**
- Verify fabric-cicd installation instructions from official docs
- Check for any environment variable requirements
- Document any authentication prerequisites

---

### Section 2.2: Creating New Section 3 (Local Manual Deployment)

This is the **most important new section**. It requires careful technical validation.

**File Location:** `.labs/lab2/lab.md` - Insert before current Section 2 (GitHub automated deployment)

#### **Subsection 3.1: Create deploy.py Script**

**Content:**
```markdown
## 3.1 Create Your Deployment Script

### Step 1: Create deploy.py in your repository root

Create a new file named `deploy.py` in the root of your PBIP repository with the following content:

[SHOW FULL CODE BLOCK]

### Understanding the Code:

**Imports:**
- `argparse`: Command-line argument parsing
- `InteractiveBrowserCredential` / `AzureCliCredential`: Azure authentication
- `FabricWorkspace`, `publish_all_items`: fabric-cicd core functions

**argparse Configuration:**
- `--workspace_id`: (Required) Fabric workspace UUID
- `--environment`: (Optional, default: 'dev') Environment name for parameterization

**Credentials:**
- Tries `AzureCliCredential()` first (for CI/CD environments)
- Falls back to `InteractiveBrowserCredential()` (for local development)
- This approach works both locally AND in GitHub Actions

**workspace_params Dictionary:**
- `workspace_id`: Target workspace UUID
- `environment`: Environment name (affects parameter.yml replacements)
- `repository_directory`: Path to PBIP project (. = current directory)
- `item_type_in_scope`: Which items to deploy (SemanticModel, Report, etc.)
- `token_credential`: Authenticated credential object

### Where to Find Your Workspace ID

[INSERT SCREENSHOT:fabric-workspace-id-location.png]

1. Go to **Fabric Portal**
2. Select your target workspace
3. In the URL bar, note the workspace ID OR
4. Go to **Workspace Settings > General**
5. Copy the **Workspace ID** (shown as a UUID)

⚠️ **Important:** Use the workspace UUID (long alphanumeric string), NOT the workspace name!

### Common Customizations:

If you only want to deploy the semantic model without reports:
\`\`\`python
"item_type_in_scope": ["SemanticModel"],
\`\`\`

For more customization options, see [fabric-cicd documentation](https://microsoft.github.io/fabric-cicd/latest/)
```

**Technical Validation Needed:**
- [ ] Test the exact code with actual Fabric workspace
- [ ] Verify InteractiveBrowserCredential opens browser correctly
- [ ] Confirm error messages when workspace_id is invalid
- [ ] Test with both absolute and relative paths for repository_directory
- [ ] Document expected output when successful

**Screenshots Needed:**
- Screenshot 1: deploy.py file in VS Code (syntax highlighted)
- Screenshot 2: Workspace ID location in Fabric portal

**Time to Create:** 2-3 hours (including testing)

#### **Subsection 3.2: Deploy from Local Machine**

**Content Structure:**
```markdown
## 3.2 Run Your First Local Deployment

### Step 1: Open a Terminal
- In VS Code: Press `Ctrl + '` (backtick)
- Navigate to repository root: `cd C:\path\to\Sales` (or your PBIP folder)

### Step 2: Run the Deployment Script
[EXACT COMMAND]
\`\`\`bash
python deploy.py --workspace_id "11111111-1111-1111-1111-111111111111"
\`\`\`

⚠️ Replace the UUID with your actual workspace ID from the previous step.

### Step 3: Authenticate (First Time Only)
[EXPLAIN BROWSER FLOW]
- A browser window will open automatically
- Sign in with your Azure/Microsoft account
- Grant permissions when asked
- Return to terminal (browser can close)

### Step 4: Monitor Deployment Progress
[SHOW EXPECTED OUTPUT]
\`\`\`
[info] Publishing SemanticModel 'Sales'
       Operation in progress. Checking again in 1 second (Attempt 1)...
       Operation in progress. Checking again in 1 second (Attempt 2)...
[info] Publishing Report 'Sales'
       Published
\`\`\`

Deployment typically takes 20-30 seconds.

### Step 5: Verify in Fabric

[INSERT SCREENSHOT: Fabric workspace with newly deployed items]

1. Go to Fabric Portal
2. Open your workspace
3. Verify semantic model appears
4. Verify report appears

### 🔄 First-Time Data Connectivity Issue (Expected)

[EXPLAIN KNOWN ISSUE]

The report will not show data yet. This is expected! Follow these steps:

1. Click on the semantic model
2. Go to **Settings > Data source credentials**
3. Configure credentials for your data sources
4. Save

Then return to the report and refresh.

[INSERT SCREENSHOT: Data source credentials configuration]
[INSERT SCREENSHOT: Report with data successfully loaded]

### ✅ Success Checklist
- [ ] Deployment script ran without errors
- [ ] Semantic model appears in Fabric workspace
- [ ] Report appears in Fabric workspace
- [ ] Report shows data after credentials configured
```

**Technical Validation Needed:**
- [ ] Test exact deployment output
- [ ] Document all possible error messages
- [ ] Verify data source credential configuration procedure
- [ ] Test with different data source types
- [ ] Confirm refresh behavior post-deployment

**Screenshots Needed:**
- Screenshot 1: Terminal showing successful deployment (crop output)
- Screenshot 2: Fabric workspace with newly deployed items
- Screenshot 3: Data source credentials configuration screen
- Screenshot 4: Refreshed report with data visible

**Time to Create:** 2-3 hours (including testing)

#### **Subsection 3.3: Understanding Parameterization (Optional/Advanced)**

**Content Structure:**
```markdown
## 3.3 Environment-Specific Parameterization (Optional)

### Overview

[EXPLAIN PARAMETER.YML CONCEPT]

For this workshop, basic deployment works great. However, in production, you likely need to deploy to multiple environments (dev, staging, production) with different workspace IDs, lakehouse IDs, or connection strings.

The `fabric-cicd` tool supports this via `parameter.yml` files.

### Example: Multi-Environment Workspace IDs

Create a `parameter.yml` file in your repository root:

\`\`\`yaml
find_replace:
  - find_value: "11111111-1111-1111-1111-111111111111"  # Placeholder
    replace_value:
      dev: "11111111-1111-1111-1111-111111111111"       # Dev workspace
      staging: "22222222-2222-2222-2222-222222222222"   # Staging workspace
      prod: "33333333-3333-3333-3333-333333333333"       # Prod workspace
\`\`\`

Then update your PBIP files to use the placeholder UUID. When deploying with:
- `--environment dev` → Uses dev workspace ID
- `--environment staging` → Uses staging workspace ID
- `--environment prod` → Uses prod workspace ID

### Why This Matters
- Deploy the same PBIP code to multiple environments
- No manual editing of workspace IDs required
- Supports different data sources per environment
- Critical for proper DevOps practices

### For This Workshop
We'll skip parameterization for simplicity. To learn more, see the [fabric-cicd documentation on parameterization](https://microsoft.github.io/fabric-cicd/latest/).

In Lab activities, we'll keep it simple with single-environment deployments.
```

**Note:** This subsection is optional/advanced. Include it for completeness but mark it as "advanced topic" so participants don't get overwhelmed.

**Screenshots:** None required

**Time to Create:** 1 hour

---

### Section 2.3: Revising Current Content Sections

#### **Revision Guide for Section 0 (Setup)**

**Changes to make:**
- [ ] After "Create deployment workspace" section, add bullet:
  - "Note: Save your workspace UUID - you'll need it when creating deploy.py"
- [ ] In prerequisites, add link to fabric-cicd requirements (Python 3.9-3.12)

**Reason:** Transition into the fabric-cicd tool introduction

---

#### **Revision Guide for Current Section 2 → New Section 4 (GitHub Actions)**

**Key Change:** Introduce the canonical GitHub Actions workflow that calls deploy.py

**Current State:** Lab seems to assume pre-built workflow exists

**Changes to make:**

1. **Add Subsection 4.0: GitHub Actions Workflow Setup**

Create `.github/workflows/deploy.yml` with:
```yaml
name: Deploy PBIP to Fabric

on:
  push:
    branches: [dev, main]
  workflow_dispatch:

env:
  PYTHON_VERSION: '3.12'

jobs:
  deploy:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: ${{ env.PYTHON_VERSION }}
      
      - name: Install fabric-cicd
        run: pip install fabric-cicd
      
      - name: Determine workspace ID and environment
        id: env-config
        shell: pwsh
        run: |
          $branch = "${{ github.ref_name }}"
          
          if ($branch -eq "main") {
              $workspace_id = "${{ secrets.FABRIC_WORKSPACE_ID_PROD }}"
              $environment = "prod"
          } elseif ($branch -eq "dev") {
              $workspace_id = "${{ secrets.FABRIC_WORKSPACE_ID_DEV }}"
              $environment = "dev"
          } else {
              Write-Error "Unknown branch: $branch"
              exit 1
          }
          
          echo "workspace_id=$workspace_id" >> $env:GITHUB_OUTPUT
          echo "environment=$environment" >> $env:GITHUB_OUTPUT
      
      - name: Deploy PBIP to Fabric
        shell: pwsh
        env:
          FABRIC_CLIENT_ID: ${{ secrets.FABRIC_CLIENT_ID }}
          FABRIC_CLIENT_SECRET: ${{ secrets.FABRIC_CLIENT_SECRET }}
          FABRIC_TENANT_ID: ${{ secrets.FABRIC_TENANT_ID }}
        run: |
          python deploy.py --workspace_id "${{ steps.env-config.outputs.workspace_id }}" --environment "${{ steps.env-config.outputs.environment }}"
```

2. **Explain Each Workflow Component:**
   - `on:` triggers (push to main/dev branches, manual dispatch)
   - `runs-on: windows-latest` (Windows runner needed for PBIP tools)
   - Setup Python step
   - Install fabric-cicd
   - Environment determination logic
   - Deploy using same deploy.py script

3. **Key Concept to Emphasize:**
   > "The exact same `deploy.py` script you ran locally is now running in GitHub Actions. You understand both the local AND automated deployment now!"

---

#### **Revision Guide for Current Section 3 → New Section 6 (Branch Protection & PR)**

**Minimal changes needed** - mostly reorder and update references

Changes:
- [ ] Update section numbers in cross-references
- [ ] Update workflow names/references to use "Deploy PBIP to Fabric" workflow name
- [ ] Add subsection: "Understanding Workflow Runs During PRs"
  - [ ] Explain that PRs trigger workflow_dispatch (or separate validate workflow)
  - [ ] BPA checks happen separately
  - [ ] Main deployment happens on merge to main

---

## Phase 3: Technical Content Validation

### Pre-Publication Validation Checklist

**Code Examples:**
- [ ] Test deploy.py script exactly as written with real Fabric workspace
- [ ] Verify Python version compatibility (3.9-3.12)
- [ ] Test InteractiveBrowserCredential flow
- [ ] Test with service principal credentials
- [ ] Document exact error messages for common errors
- [ ] Test parameter.yml with actual substitution
- [ ] Verify workflow YAML syntax (can use GitHub YAML linter)
- [ ] Test GitHub Actions workflow end-to-end

**Screenshots:**
- [ ] All screenshots show current UI (verify against live Fabric/GitHub)
- [ ] Crop to relevant area only (no sensitive data, clean framing)
- [ ] Add subtle annotations/arrows where needed
- [ ] Check resolution (aimed for ~1200x800 px minimum)
- [ ] Verify file formats (PNG preferred, <500KB)
- [ ] Check for typos in any text visible in screenshots

**Documentation:**
- [ ] All command examples copy/paste exactly
- [ ] All links are live and accurate
- [ ] No broken references
- [ ] Terminology consistent throughout
- [ ] Code indentation correct (verify in rendering)
- [ ] File paths use correct separators (/ for paths, `\` only in Windows-specific commands)

**Accuracy:**
- [ ] Workspace UUID vs. workspace name - clarified throughout
- [ ] Service principal requirements - still accurate?
- [ ] Fabric API requirements - still current?
- [ ] GitHub Actions API - still current?
- [ ] Python package versions - still compatible?

---

## Phase 4: Testing the Full Lab Workflow

### Full Walkthrough Script

**Duration:** One complete run-through of the entire lab (should take ~2 hours)

**Test Environment Requirements:**
- [ ] Fresh Fabric test workspace (not your main workspace)
- [ ] Fresh GitHub test repository
- [ ] Test service principal account
- [ ] Windows/Linux machine with all prerequisites

**Walkthrough Steps:**

1. **Start with blank slate**
   ```bash
   # Use the Sales.pbix from Lab 1
   # Save as PBIP in test location
   cd ~/test-lab
   ```

2. **Follow Section 0 exactly**
   - [ ] Create service principal in Entra ID
   - [ ] Verify credentials saved securely
   - [ ] Add to Fabric workspace with correct permissions
   - [ ] Note: Keep workspace UUID saved

3. **Follow Section 1 exactly**
   - [ ] Install fabric-cicd per instructions
   - [ ] Verify installation works
   - [ ] Run: `python -c "from fabric_cicd import FabricWorkspace; print('OK')"`

4. **Follow Section 2 exactly**
   - [ ] Create GitHub repository
   - [ ] Add secrets/variables
   - [ ] **Document:** Did current instructions match GitHub UI?
   - [ ] Add git remote
   - [ ] Attempt to publish branch

5. **Follow Section 3 exactly** ← Most critical
   - [ ] Create deploy.py in repository root
   - [ ] Update workspace_id with real UUID
   - [ ] Run: `python deploy.py --workspace_id "..."`
   - [ ] **Document:** Did script work first try?
   - [ ] **Document:** Did browser authentication work?
   - [ ] Verify items deployed to Fabric
   - [ ] Configure data source credentials
   - [ ] Refresh report
   - [ ] **Document:** How long did first deployment take?

6. **Follow Section 4 exactly**
   - [ ] Create .github/workflows/deploy.yml
   - [ ] Update with real workspace IDs
   - [ ] Commit and push to main
   - [ ] **Document:** Did workflow trigger automatically?
   - [ ] Monitor workflow in GitHub Actions UI
   - [ ] Verify successful deployment

7. **Follow Section 5 & 6 exactly**
   - [ ] Create feature branch
   - [ ] Make small change to report
   - [ ] Commit and push
   - [ ] Create pull request
   - [ ] Verify workflow runs (BPA checks)
   - [ ] Merge PR
   - [ ] Verify deployment workflow triggered
   - [ ] Verify changes deployed

**Issue Tracking Document to Create:**

During this walkthrough, create a file `.labs/lab2/TESTING_NOTES.md`:
```markdown
# Lab 2 Full Walkthrough Testing Notes

Date: [DATE]
Tester: [NAME]
Environment: [FABRIC WORKSPACE], [GITHUB REPO]

## Issues Encountered

### Issue 1: [Title]
- **Section:** X.X
- **Symptom:** [What went wrong]
- **Root Cause:** [Why it happened]
- **Resolution:** [How to fix]
- **Updated Instructions:** [If docs need changes]

### Issue 2: [etc.]

## Timing Notes

- Section 0: [X min] (expected: 20 min)
- Section 1: [X min] (expected: 5 min)
- Section 2: [X min] (expected: 25 min)
- Section 3: [X min] (expected: 25 min)
- Section 4: [X min] (expected: 20 min)
- Section 5: [X min] (expected: 15 min)
- Section 6: [X min] (expected: 20 min)

**Total Time:** X minutes (estimated 110-120 min)

## Missing Screenshots

- [ ] Section X.X needs screenshot of [component]

## UI Changes Noticed

- GitHub [component] UI changed from [old] to [new] - Update screenshot needed
- Fabric [component] UI changed - Verify links still work

## Success Criteria Met

- [ ] All code examples work as written
- [ ] All links are live
- [ ] No conceptual errors in explanations
- [ ] Timing is appropriate for workshop
- [ ] Participant learning objectives achievable
```

---

## Phase 5: Documentation & Delivery Preparation

### Create Instructor Guide

**File:** `.labs/lab2/INSTRUCTOR_GUIDE.md`

**Content:**
```markdown
# Lab 2 Instructor Guide: Power BI & CI/CD

## Quick Reference

**Duration:** 110-120 minutes (expanded from 90 min v1.0)  
**Prerequisites:** Lab 1 completed, Fabric tenant with admin access  
**Key New Content:** Sections 1, 3, 4 (fabric-cicd introduction and local deployment)  
**Most Challenging Section:** Section 3 (local deployment with Python)  

## One-Minute Lab Summary

Participants will learn to deploy Power BI projects to Fabric using the `fabric-cicd` Python tool, both locally and through GitHub Actions CI/CD automation. They'll understand how parameterization enables multi-environment deployments and practice collaborative workflows with branch protection.

## Key Learning Outcomes

By the end of Lab 2, participants should be able to:

1. **Explain** why fabric-cicd is Microsoft's recommended deployment tool
2. **Create** a deploy.py script for PBIP deployments
3. **Execute** a local deployment using fabric-cicd
4. **Configure** GitHub Actions to automate deployments
5. **Practice** collaborative workflows with pull requests and branch protection
6. **Troubleshoot** common deployment failures

## Section-by-Section Teaching Notes

### Section 0: Setup (20 min)
**Teaching Tips:**
- Most participants will complete this before lab
- Have backup service principal prepared in case of issues
- Watch for workspace ID confusion (UUID vs. name)

**Common Issues:**
- Service principal not enabled at tenant level (setup should catch this)
- Wrong workspace ID format (emphasize it's a UUID)

**Checkpoint:** Verify everyone has service principal credentials saved securely

---

### Section 1: fabric-cicd Introduction (5 min)
**Teaching Tips:**
- Keep this conceptual, not too detailed
- Emphasize Microsoft recommends this tool
- Preview that they'll use it hands-on in next section

**Key Points:**
- fabric-cicd is Python library from Microsoft
- Deployment logic lives in deploy.py
- Same script runs locally AND in GitHub Actions (big insight!)

**Checkpoint:** Ask participants to explain what fabric-cicd does in their own words

---

### Section 3: Local Manual Deployment (CRITICAL - 25 min)
**Teaching Tips:**
- This is the NEW critical section - spend extra time here
- Watch participants closely when they create deploy.py
- Many will struggle getting workspace_id formatted correctly
- "This is where the actual deployment happens" - emphasize importance

**Common Issues & Solutions:**
| Issue | Solution |
|-------|----------|
| Python module not found error | Verify pip install ran successfully |
| `ModuleNotFoundError: No module named 'azure'` | May need to install: `pip install azure-identity` |
| Workspace not found error | Verify workspace UUID format (long alphanumeric) |
| Browser authentication fails | Check if running in non-interactive environment |
| Deployment hangs | Dependencies might not be installed; check internet |

**Checkpoint:** Have participants show you successful deployment output from their terminal

---

### Section 4: GitHub Actions (20 min)
**Teaching Tips:**
- Show how this automates the same deploy.py script
- Many participants get lost in YAML syntax
- Emphasize: "It's the same Python script you just learned"
- Help them understand branch-based routing logic

**Common Issues:**
- Workflow file in wrong location (.github/workflows/ directory crucial)
- Workspace ID not updated in workflow
- Secrets not created before pushing workflow

**Checkpoint:** Demonstrate workflow running in GitHub Actions UI

---

### Section 6: Branch Protection & PR (20 min)
**Teaching Tips:**
- This brings together entire lab
- Real-world workflow demo
- Many participants find this conceptually clearest section
- Emphasize protection prevents accidents in production

**Checkpoint:** Successful merge and automatic deployment verification

## Setting Up Your Demo Environment

Before the workshop, create a demo setup:

1. Create a clean PBIP project (use Sales from Lab 1)
2. Create test GitHub repository
3. Create test Fabric workspace
4. Walk through entire lab yourself at least twice
5. Document timing for each section
6. Identify where your environment differs from instructions
7. Prepare customized instructions if needed

## Troubleshooting Guide

### "Module not found: fabric_cicd"

**Diagnosis:** pip install didn't work

**Solutions:**
1. Verify Python version: `python --version` (need 3.9+)
2. Verify pip is using correct Python: `pip --version`
3. Try: `python -m pip install fabric-cicd`
4. Try in admin/elevated terminal

### "Workspace with ID ... not found"

**Diagnosis:** Wrong workspace ID or permissions

**Solutions:**
1. Verify workspace UUID format (21-char alphanumeric)
2. Verify service principal is member of workspace with Contributor role
3. Check workspace exists: navigate to it in Fabric portal
4. Verify workspace is in Premium capacity (shared laptops sometimes don't show)

### "Authentication failed / browser didn't open"

**Diagnosis:** InteractiveBrowserCredential issue

**Causes:**
- Running in restricted network environment
- Multiple browser tabs/windows interfering
- Terminal blocking browser popup

**Solutions:**
1. Check if browser opened (might be behind other windows)
2. Run with: `set PYTHONUNBUFFERED=1` (Win) / `export PYTHONUNBUFFERED=1` (Mac/Linux)
3. Check terminal output for authentication URL to copy manually
4. Last resort: have participant use their Azure CLI credentials if available

### "Deployment timed out / took too long"

**Diagnosis:** Large semantic model or network issues

**Solutions:**
1. This is normal for large models (20-30 sec is expected)
2. Check internet connection
3. Verify Fabric capacity isn't under heavy load
4. Try again after few minutes

### GitHub Actions workflow not triggering

**Diagnosis:** File not committed or branch not matched

**Solutions:**
1. Verify .github/workflows/deploy.yml is committed (not just local)
2. Verify pushing to `main` or `dev` branch (check workflow trigger branches)
3. Check GitHub Actions tab for any error messages
4. Manually trigger with workflow_dispatch button

### Data not showing in report after deployment

**Diagnosis:** Expected behavior - credentials not set up yet

**Solution:**
This is not a problem! Document states data source credentials must be configured manually after first deployment. Walk through:
1. Open semantic model
2. Settings > Data source credentials
3. Configure for "Web" source (Anonymous for demo)
4. Refresh report

## Q&A: Expected Participant Questions

**Q: "Why do I need deploy.py if GitHub Actions will call it?"**  
A: You need to understand the tool locally first. Local understanding helps you troubleshoot CI/CD issues. Plus, many organizations use local deployment for manual control.

**Q: "Can I deploy without a service principal?"**  
A: For this lab, yes - you use your own Microsoft account via browser. In production, service principals prevent personal account dependencies.

**Q: "What's the difference between dev and main branches?"**  
A: Dev is typically lower environment (slower, less stable). Main is production (stable, protected). In real projects, you'd have separate workspaces for each.

**Q: "Do I need to configure data source credentials every time I deploy?"**  
A: No - only the first time. fabric-cicd caches credentials in Fabric after initial setup.

**Q: "Can I deploy just the report without the semantic model?"**  
A: Yes! In deploy.py, change `"item_type_in_scope": ["SemanticModel", "Report"]` to just `["Report"]`

**Q: "What's parameter.yml for?"**  
A: Advanced feature for multi-environment deployments. For this workshop, we keep it simple.

## Post-Lab Activities

**For Participants to Do After Workshop:**

1. Set up parameter.yml for multi-environment deployment
2. Create separate dev/staging/prod workspaces
3. Set up approval gates in GitHub for main branch
4. Explore fabric-cicd advanced features
5. Create custom validation checks beyond BPA

**For Instructors to Do After Workshop:**

1. Collect feedback specifically on Sections 1, 3, 4
2. Note any timing adjustments needed
3. Document any environment-specific issues
4. Update TESTING_NOTES.md with actual participant experience
5. Share learnings with other facilitators

## Resources for Supplementary Learning

**Microsoft Official:**
- [fabric-cicd Documentation](https://microsoft.github.io/fabric-cicd/latest/)
- [Deploy PBIP using fabric-cicd](https://learn.microsoft.com/en-us/power-bi/developer/projects/projects-deploy-fabric-cicd)
- [Service Principals in Fabric](https://learn.microsoft.com/en-us/fabric/admin/service-admin-portal-developer)

**Supplementary Reading:**
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Azure Identity SDK - Python](https://learn.microsoft.com/en-us/python/api/overview/azure/identity-readme)
- [Power BI Governance & Deployment](https://learn.microsoft.com/en-us/power-bi/guidance/powerbi-implementation-planning-content-lifecycle-management-overview)

## Appendix: Key Commands Cheat Sheet

**For participants to reference after lab:**

```bash
# Install fabric-cicd
pip install fabric-cicd

# Deploy locally
python deploy.py --workspace_id "YOUR_WORKSPACE_UUID"

# Deploy to specific environment
python deploy.py --workspace_id "YOUR_WORKSPACE_UUID" --environment prod

# Check Git status
git status

# Create and switch to branch
git checkout -b feature/my-change

# Commit changes
git commit -am "Description of change"

# Push to remote
git push origin feature/my-change
```

## Customization for Your Organization

**Items to customize before delivering to your participants:**

- [ ] Service principal tenant ID (if not provided)
- [ ] Workspace IDs for dev/staging/prod environments
- [ ] GitHub organization name
- [ ] Internal naming conventions
- [ ] Approval process requirements (who reviews PRs)
- [ ] Deployment frequency expectations
- [ ] Data source credential management policy

---

**Last Updated:** [DATE]  
**Version:** 1.0
```

### Create Quick Reference / Cheat Sheet

**File:** `.labs/lab2/PARTICIPANT_CHEATSHEET.md`

```markdown
# Lab 2: Power BI & CI/CD - Quick Reference

## Key Concepts

| Concept | What It Does | When Used |
|---------|-------------|-----------|
| **fabric-cicd** | Python tool for deploying PBIP to Fabric | Both local and GitHub Actions |
| **deploy.py** | Deployment script you create | Local testing + GitHub Actions automation |
| **Service Principal** | Azure identity for automated deployments | Enables unattended deployments |
| **GitHub Actions** | Automation platform | Runs deploy.py when you push code |
| **Branch Protection** | Requires PR review before merging | Prevents direct changes to main |

## Commands You'll Use

```bash
# Install fabric-cicd
pip install fabric-cicd

# Deploy locally to Fabric
python deploy.py --workspace_id "<YOUR_WORKSPACE_ID>"

# Git commands
git remote -v                                    # View remotes
git checkout -b feature/name                     # Create branch
git commit -am "Your message"                    # Commit changes
git push origin feature/name                     # Push to GitHub
```

## Files You'll Create

| File | Purpose | Location |
|------|---------|----------|
| `deploy.py` | Deployment logic | Repository root |
| `.github/workflows/deploy.yml` | GitHub Actions automation | `.github/workflows/` |
| `parameter.yml` | Environment configs (optional) | Repository root |

## Workspace ID: Where to Find It

1. **Fabric Portal** → Your workspace → **Settings** → **General**
2. Look for "Workspace ID" (long alphanumeric string)
3. Copy and save - you'll use it multiple times

## Pre-Deployment Checklist

Before running `python deploy.py ...`:

- [ ] Workspace ID copied and ready
- [ ] Service principal has Contributor role in workspace
- [ ] fabric-cicd installed: `pip install fabric-cicd`
- [ ] You're in repository root directory
- [ ] PBIP project structure intact

## After Deployment: Data Setup

First deployment deploys metadata only - no data:

1. Open semantic model in Fabric
2. **Settings** → **Data source credentials**
3. Configure credentials for your data source
4. Return to report and **Refresh**

## GitHub Actions Workflow: What It Does

```
You push to main branch
    ↓
GitHub Actions triggers
    ↓
Actions runs: pip install fabric-cicd
    ↓
Actions runs: python deploy.py --workspace_id "..."
    ↓
Your PBIP deploys to Fabric automatically
    ↓
New/updated items appear in workspace
```

## Troubleshooting: 3 Most Common Issues

### Issue 1: "Module not found: fabric_cicd"
```bash
# Fix: Make sure it's installed
pip install fabric-cicd
python -c "import fabric_cicd; print('OK')"
```

### Issue 2: "Workspace not found"
- Double-check workspace ID format (should be UUID)
- Verify service principal has access to workspace
- Confirm workspace exists in Fabric portal

### Issue 3: "Data doesn't appear after deployment"
- This is expected!
- Follow "After Deployment: Data Setup" section above
- Add your data source credentials

## One-Line Summary

> **Take your PBIP code from GitHub and automatically deploy it to Fabric using Python and GitHub Actions**

## Key Files Reference

**deploy.py** - Your deployment script:
```python
# Just the important parts:
from fabric_cicd import FabricWorkspace, publish_all_items

workspace = FabricWorkspace(
    workspace_id="YOUR_WORKSPACE_ID",
    repository_directory="."
)
publish_all_items(workspace)
```

**.github/workflows/deploy.yml** - GitHub Actions automation:
```yaml
name: Deploy PBIP to Fabric
on:
  push:
    branches: [main]
  
jobs:
  deploy:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
      - run: pip install fabric-cicd
      - run: python deploy.py --workspace_id "YOUR_WORKSPACE_ID"
```

## After the Workshop

**Next Steps You Can Take:**

1. Add `parameter.yml` for dev/staging/prod environments
2. Create separate workspaces for each environment
3. Set up GitHub branch protection rules
4. Create approval requirements for PRs
5. Explore fabric-cicd advanced options
6. Automate data refresh after deployments

## Useful Links

- [fabric-cicd Documentation](https://microsoft.github.io/fabric-cicd/latest/)
- [GitHub Actions Docs](https://docs.github.com/en/actions)
- [Fabric REST APIs](https://learn.microsoft.com/en-us/rest/api/fabric/)
- [Power BI Best Practices](https://learn.microsoft.com/en-us/power-bi/guidance/)

---

**💡 Remember:** fabric-cicd is the deployment tool - it's the same script whether you run it locally or GitHub runs it automatically!
```

---

## Phase 6: Final Review Checklist

Before publishing revised lab.md:

### Content Review
- [ ] All sections follow logical progression
- [ ] Learning objectives clear at start of lab
- [ ] Estimated times provided for each section
- [ ] Code examples tested and verified
- [ ] All screenshots captured and properly referenced
- [ ] No broken links or references

### Technical Review
- [ ] All commands work as written
- [ ] Python code follows best practices
- [ ] YAML syntax is valid (use linter)
- [ ] File paths use correct separators
- [ ] Workspace ID/name distinction clear throughout

### Pedagogical Review
- [ ] Lab builds on Lab 1 concepts
- [ ] Progressive complexity (basic → intermediate → advanced)
- [ ] Hands-on checkpoints at key steps
- [ ] Real-world scenarios presented
- [ ] Troubleshooting guidance provided

### Accessibility Review
- [ ] Screenshots have alt-text descriptions
- [ ] Code blocks have syntax highlighting
- [ ] Important notes in callout boxes (⚠️, 💡, ℹ️)
- [ ] Terminology explained on first use
- [ ] Different learning styles accommodated

### Final QA
- [ ] Peer review completed
- [ ] Grammar/spell-check passed
- [ ] Formatting consistent throughout
- [ ] No placeholder text remaining
- [ ] Ready for workshop delivery

---

## Summary: What Gets Delivered

**After completing this execution guide, the following deliverables exist:**

1. ✅ **IMPLEMENTATION_PLAN.md** (7-page strategic plan) - Already created
2. **Revised lab.md** (10-12 pages, 7 sections instead of 5)
   - Section 0: Setup (revised)
   - Section 1: fabric-cicd Introduction (NEW)
   - Section 2: GitHub Repository Setup (revised)
   - Section 3: Local Manual Deployment (NEW - critical)
   - Section 4: GitHub Actions Automation (NEW)
   - Section 5: First Automated Deployment (revised)
   - Section 6: Branch Protection & PR (revised)
3. **New Screenshots** (8-10 new images)
4. **INSTRUCTOR_GUIDE.md** (comprehensive teaching guide)
5. **PARTICIPANT_CHEATSHEET.md** (quick reference)
6. **TESTING_NOTES.md** (documentation from full walkthrough)
7. **REVISION_CHECKLIST.md** (tracking progress)

**Total Implementation Time:** 40-50 hours (across one week)

---

**Document Version:** 1.0  
**Next Review:** After first full lab delivery  
**Questions:** Contact workshop lead
