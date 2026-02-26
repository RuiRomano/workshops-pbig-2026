# Lab 2 Implementation Plan: CI/CD Alignment & Enhancement

**Document Date:** February 26, 2026  
**Status:** Planning Phase  
**Target Alignment:** Microsoft Learn - [Deploy Power BI projects (PBIP) using fabric-cicd](https://learn.microsoft.com/en-us/power-bi/developer/projects/projects-deploy-fabric-cicd)

---

## Executive Summary

This document outlines a detailed analysis of the current Lab 2 instructions against the official Microsoft documentation for fabric-cicd deployment. The current lab provides a solid foundation for CI/CD concepts but **significantly diverges from the official Microsoft approach**, which emphasizes the `fabric-cicd` Python tool. This plan details the changes needed to align the lab with Microsoft's recommended practices while maintaining a pedagogical flow suitable for a workshop environment.

### Key Finding: Two Different Approaches

- **Current Lab:** Uses GitHub Actions workflow (pre-built, appears to use a Power BI Analyzer action for validation), GitHub secrets, and direct deployment orchestration
- **Microsoft Official:** Emphasizes the `fabric-cicd` Python library as the core deployment tool, with environment parameterization via `parameter.yml` and local manual deployment before automation

---

## 1. Detailed Analysis: Current Lab vs. Official Documentation

### 1.1 Architecture Comparison

| Aspect | Current Lab | Microsoft Official | Gap |
|--------|-------------|-------------------|-----|
| **Core Deployment Tool** | GitHub Actions (pre-built workflow) | `fabric-cicd` Python library | **Critical: Lab doesn't mention fabric-cicd** |
| **Deployment Logic** | In GitHub Actions workflow | Python script (`deploy.py`) | **Major: No deploy.py creation in lab** |
| **Environment Config** | GitHub variables (workspace name) | `parameter.yml` for find/replace logic | **Major: No parameterization layer** |
| **Authentication** | 3 separate secrets (CLIENT_ID, SECRET, TENANT_ID) | Single JSON AZURE_CREDENTIALS secret | **Moderate: Different structure** |
| **First Deployment** | Goes directly to GitHub Actions automation | Starts with local manual deployment | **Major: No local testing path** |
| **Parameterization** | Workspace ID only | Workspace IDs, lakehouse IDs, connection strings, etc. | **Major: Severely limited** |
| **Deployment Scope** | Fixed items (all semantic models and reports) | Configurable by item type | **Moderate: Less flexibility** |
| **Data Source Handling** | Mentions manual credential setup post-deploy | Documented as part of deployment flow | **Minor: Timing and sequencing** |

### 1.2 Missing Components from Official Documentation

1. **fabric-cicd Python library introduction and installation**
   - Not mentioned in current lab
   - Essential for understanding the deployment mechanism
   - Should be prerequisite software in main README

2. **Local manual deployment workflow**
   - Official docs start with `python deploy.py` on local machine
   - Current lab jumps directly to GitHub Actions automation
   - Missing: opportunity for participants to understand the tool before automation

3. **deploy.py script creation**
   - Official docs show complete code example with argparse
   - Current lab assumes pre-built workflow exists
   - No guidance on customizing deployment logic

4. **parameter.yml for environment-specific values**
   - Official docs detail find/replace parameterization
   - Enables multi-environment deployments (dev, staging, prod)
   - Current lab only handles workspace name as variable

5. **AZURE_CREDENTIALS secret structure**
   - Official docs use JSON format with clientId, clientSecret, subscriptionId, tenantId
   - Current lab uses 3 separate secrets
   - Different authentication approach in workflows

6. **Azure DevOps as alternative CI platform**
   - Official docs provide azure-pipelines.yml example
   - Not mentioned in current lab
   - May be relevant for enterprise participants

### 1.3 Current Lab Strengths (to Preserve)

✅ **Service Principal Setup:** Excellent step-by-step guidance with Entra ID screenshots  
✅ **GitHub Setup Clarity:** Very clear repository creation and secret/variable configuration  
✅ **Branch Protection Rules:** Good introduction to protected branches concept  
✅ **Pull Request Workflow:** Excellent hands-on demonstration of collaborative workflow  
✅ **Data Refresh Requirement:** Explicitly addresses post-deployment data source credential configuration  
✅ **BPA Quality Gate:** Good introduction to automated quality checks as part of PR reviews  
✅ **Practical Scenario:** Real-world workflow patterns (feature branch → PR → merge → deploy)

---

## 2. Recommended Changes & Enhancement Strategy

### 2.1 Overall Structure Reorganization

**Current Flow (5 sections):**
1. Setup / Deployment Credentials
2. Create & configure GitHub repository
3. First automated CI/CD deployment
4. Set up branch protection rules
5. Start work in a new branch and create a PR

**Recommended Flow (7 sections):**
1. Setup / Deployment Credentials *(retain)*
2. Introduction to fabric-cicd tool *(new)*
3. Create and configure GitHub repository *(enhance)*
4. **Local Manual Deployment with fabric-cicd** *(new - critical)*
5. GitHub Actions Automation Setup *(revise)*
6. First automated deployment *(revise)*
7. Branch Protection & Pull Request Workflow *(retain & enhance)*

**Rationale:** This progression moves from understanding the tool → using it locally → automating it with GitHub Actions. Aligns with Microsoft's pedagogical approach and builds confidence before dealing with CI/CD complexity.

### 2.2 Detailed Changes Required

#### **Section 0: Setup (Retain with Minor Updates)**

**Changes:**
- ✏️ Add note that service principal credentials will be used both for local `deploy.py` AND GitHub Actions
- ✏️ Add link to fabric-cicd Python requirements (3.9-3.12) and reference Python installation check
- ✅ Keep all Entra ID screenshots (excellent)
- ✅ Keep Fabric workspace setup (excellent)
- ✏️ Update authentication note: mention AZURE_CREDENTIALS will be used instead of individual secrets (foreshadowing)

---

#### **Section 1: Introduction to fabric-cicd (NEW)**

**Content to Add:**
- Brief explanation of why fabric-cicd is the recommended approach:
  - Native Fabric REST APIs
  - Python-native for modern DevOps workflows
  - Environment parameterization support
  - Flexible deployment control
  - Reliable authentication
  
- Link to [fabric-cicd documentation](https://microsoft.github.io/fabric-cicd/latest/)

- Installation command:
  ```bash
  pip install fabric-cicd
  ```

- Overview of how fabric-cicd fits into the deployment pipeline:
  - Local deployment script → CI/CD automation

**No screenshots needed** - mostly conceptual introductory content

---

#### **Section 2: Create & Configure GitHub Repository (REVISE)**

**Changes:**
- ✏️ Update secrets configuration section:
  - Current: 3 separate secrets (FABRIC_CLIENT_ID, FABRIC_CLIENT_SECRET, FABRIC_TENANT_ID)
  - Recommended: **EITHER** use AZURE_CREDENTIALS JSON (official approach) **OR** keep current for simplicity with local deploy.py
  - **Decision needed:** Workshop participants may not have az cli installed. Current approach (3 secrets) actually works better for local deploy.py with InteractiveBrowserCredential fallback
  - Recommendation: **Keep current 3-secret approach for local deploy.py**, note that GitHub Actions can use AZURE_CREDENTIALS
  
- ✏️ Add note about workspace variable vs. workspace ID:
  - Current lab uses workspace NAME as variable
  - Official docs use workspace UUID
  - Recommendation: Keep NAME-based approach for simplicity, but document that production should use IDs with parameter.yml

- ✏️ Update Git remote section:
  - Add note that this remote will be used for pushing all commits (both local and automated)

**Screenshots:** Reuse all current screenshots (excellent quality)

---

#### **Section 3: Local Manual Deployment with fabric-cicd (NEW - CRITICAL)**

**This is the biggest addition. New subsection breakdown:**

##### **3.1 Create deploy.py Script**
- Create a Python script in the repository root
- Show simplified version first:
  ```python
  import argparse
  from azure.identity import InteractiveBrowserCredential
  from fabric_cicd import FabricWorkspace, publish_all_items
  
  parser = argparse.ArgumentParser(description="Deploy PBIP to Fabric")
  parser.add_argument("--workspace_id", type=str, required=True)
  parser.add_argument("--environment", type=str, default="dev")
  args = parser.parse_args()
  
  credential = InteractiveBrowserCredential()
  
  workspace_params = {
      "workspace_id": args.workspace_id,
      "environment": args.environment,
      "repository_directory": ".",
      "item_type_in_scope": ["SemanticModel", "Report"],
      "token_credential": credential,
  }
  
  target_workspace = FabricWorkspace(**workspace_params)
  publish_all_items(target_workspace)
  ```

- Explanation of parameters
- Guidance on getting workspace ID from Fabric portal

**Screenshots needed:**
- VS Code with deploy.py file created
- Fabric portal showing workspace ID location (reuse/adapt from official docs)

##### **3.2 Deploy from Local Machine**
- Step-by-step instructions to run local deployment
- Command: `python deploy.py --workspace_id "<WORKSPACE_ID>"`
- Show expected output/success messages
- Address data refresh requirement (same as section 2 currently)

**Screenshots needed:**
- Terminal showing successful deployment output
- Fabric workspace with deployed items

##### **3.3 Understanding Parameter Parameterization (parameter.yml)**
- Introduce `parameter.yml` for environment-specific values
- Show simple example:
  ```yaml
  find_replace:
    - find_value: "11111111-1111-1111-1111-111111111111"
      replace_value:
        dev: "11111111-1111-1111-1111-111111111111"
        prod: "22222222-2222-2222-2222-222222222222"
  ```
- Explain how this enables multi-environment deployments
- Note: For this workshop, keeping it simple. Production guidance available in fabric-cicd docs

**Screenshots:** Conceptual - show file structure in VS Code

---

#### **Section 4: GitHub Actions Automation Setup (REVISED)**

**Changes:**
- ✏️ Create `.github/workflows/deploy.yml` based on Microsoft's official template
- ✏️ Adapt to use 3 individual secrets since that's what's configured for local deploy.py
- ✏️ Include both `dev` and `main` branches (though lab only uses main initially)
- ✏️ Embed the deploy.py execution in the workflow

**Workflow structure:**
```yaml
name: Deploy PBIP to Fabric

on:
  push:
    branches: [dev, main]
  workflow_dispatch:

jobs:
  deploy:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: '3.12'
      - name: Install fabric-cicd
        run: pip install fabric-cicd
      - name: Deploy PBIP to Fabric
        shell: pwsh
        run: |
          export FABRIC_CLIENT_ID="${{ secrets.FABRIC_CLIENT_ID }}"
          export FABRIC_CLIENT_SECRET="${{ secrets.FABRIC_CLIENT_SECRET }}"
          export FABRIC_TENANT_ID="${{ secrets.FABRIC_TENANT_ID }}"
          
          $workspace_id = "<WORKSPACE_ID>"  # To be parameterized
          python -u deploy.py --workspace_id "$workspace_id" --environment prod
```

**Screenshots needed:**
- GitHub Actions workflow file in VS Code
- GitHub Actions run in progress
- GitHub Actions run completed successfully

---

#### **Section 5: First Automated Deployment (REVISE)**

**Changes:**
- ✏️ Update to reference the GitHub Actions workflow that executes deploy.py
- ✏️ Clarify the difference between local and automated deployment (same tool, different trigger)
- ✏️ Keep all existing screenshots and explanations
- ✏️ Add reference to understanding the workflow execution in GitHub Actions UI

**Screenshots:** Reuse current ones

---

#### **Section 6: Branch Protection & Pull Request Workflow (REVISE)**

**Changes:**
- ✏️ Update references to "deploy #1" and workflow names to match new deploy.yml
- ✏️ Update BPA check references - clarify whether BPA is in a separate action or part of main deploy
- ✏️ Enhance explanation of why branch protection is critical with automated deployments
- ✏️ Add section on "Checking Workflow Runs" to help troubleshoot failures

**Current flow:**
1. Create new branch
2. Make changes, commit, push
3. Create PR
4. BPA tests run
5. Fix errors and push fixes
6. Merge PR
7. Deployment triggered

**Enhancements:**
- Add note on workflow separation: "validate" workflow (BPA) vs. "deploy" workflow
- Add troubleshooting section for common deployment failures

**Screenshots:** Reuse current ones, may need new ones for GitHub Actions workflow interface

---

### 2.3 Content Organization & Formatting Improvements

**Recommendations:**

1. **Add Prerequisites Subsection**
   - Explicit list of software needed specifically for Lab 2
   - Reference back to main README
   - Add: `fabric-cicd` Python package
   - Add: Azure PowerShell modules (if using AZURE_CREDENTIALS approach)

2. **Add Conceptual Diagram**
   - Visual showing: Local Machine → deploy.py → Fabric Workspace
   - And parallel: Local Machine → Push to GitHub → GitHub Actions → deploy.py → Fabric Workspace
   - Mermaid diagram would be ideal

3. **Add "Common Issues & Troubleshooting" Section**
   - Authentication failures with InteractiveBrowserCredential
   - Workspace not found (workspace ID vs. name confusion)
   - Data source credential issues post-deploy
   - Secret rotation and expiration

4. **Add "Next Steps" Section**
   - Multi-environment deployments with parameter.yml
   - Custom item type filtering
   - Advanced GitHub Actions features (environments, approvals)
   - References to fabric-cicd advanced documentation

5. **Terminology Consistency**
   - Use "fabric-cicd" or "fabric_cicd" consistently (appears as both in Microsoft docs)
   - Use "semantic model" (not "dataset") consistently
   - Use "Power BI Project" or "PBIP" clearly distinguished

---

### 2.4 Screenshots Inventory

**Screenshots to Reuse from Current Lab (All Excellent):**
- ✅ Entra ID registration screenshots (0.1-0.2)
- ✅ GitHub repository creation (1.1)
- ✅ GitHub secrets/variables setup (1.2)
- ✅ VS Code git remote configuration (1.3)
- ✅ GitHub Actions runs (2.1)
- ✅ Fabric workspace deployment views (2.2) 
- ✅ Branch protection setup (3.1)
- ✅ Pull Request workflow (4.1-4.3)
- ✅ BPA error resolution (4.2)
- ✅ All data refresh screenshots

**New Screenshots Needed:**

| Section | Need | Description | Source |
|---------|------|-------------|--------|
| 3.1 | Screenshot: deploy.py in VS Code | Show created script file | Create locally |
| 3.1 | Screenshot: Workspace ID in Fabric | Show where to find workspace ID | Create locally or reuse from MS docs |
| 3.2 | Screenshot: Terminal output | Show successful local deploy run | Create locally |
| 3.2 | Screenshot: Items in Fabric post-deploy | Confirmation items deployed | Create locally |
| 4.1 | Screenshot: deploy.yml in GitHub | Show workflow file in Actions tab | Create locally |
| 4.1 | Screenshot: New workflow run details | Show workflow parameters and logs | Create locally |
| 5.1 | Screenshot: GitHub Actions workflow interface | Show workflow dropdown in UI | Create locally |
| 5.1 | Screenshot: Successful workflow run | Show deploy job completed | Create locally |

**Optional (can reference Microsoft Learn docs with attribution):**
- fabric-cicd architecture diagram
- Deployment flow diagrams
- Multi-environment parameterization example

---

## 3. Implementation Checklist

Use this checklist to track completion of changes. Update status as work progresses.

### Phase 1: Analysis & Planning ✅
- [x] Review current lab.md content
- [x] Review official Microsoft documentation
- [x] Compare approaches and identify gaps
- [x] Create this implementation plan document

### Phase 2: Content Restructuring
- [ ] **2.1** Reorganize lab.md into 7 sections (currently 5)
- [ ] **2.2** Update Section 0 with fabric-cicd prerequisites note
- [ ] **2.3** Create new Section 1 (fabric-cicd introduction)
  - [ ] Write conceptual explanation
  - [ ] Add link to fabric-cicd documentation
  - [ ] Explain why it's recommended by Microsoft
- [ ] **2.4** Revise Section 2 (GitHub repository setup)
  - [ ] Keep existing content mostly intact
  - [ ] Add workspace ID explanation
  - [ ] Add note about secret vs. parameter-based authentication
- [ ] **2.5** Create new Section 3 (Local manual deployment) - CRITICAL
  - [ ] **3.1** Create deploy.py subsection
    - [ ] Write full code example with explanations
    - [ ] Explain argparse parameters
    - [ ] Explain FabricWorkspace and publish_all_items
    - [ ] Test script locally for accuracy
  - [ ] **3.2** Create "Deploy from Local" subsection
    - [ ] Step-by-step execution instructions
    - [ ] Command examples
    - [ ] Expected output/success criteria
    - [ ] Data refresh workflow
  - [ ] **3.3** Create "Understanding Parameterization" subsection
    - [ ] parameter.yml examples
    - [ ] Explain find/replace logic
    - [ ] Show multi-environment setup
    - [ ] Note about complexity (optional for workshop)
- [ ] **2.6** Revise current Section 2 → new Section 4 (GitHub Actions setup)
  - [ ] Create .github/workflows/deploy.yml example
  - [ ] Adapt Microsoft's example to workshop context
  - [ ] Explain workflow triggers (push, workflow_dispatch)
  - [ ] Explain each workflow step
  - [ ] Document how to update workspace_id variable
- [ ] **2.7** Revise current Section 3 → new Section 5 (First automated deployment)
  - [ ] Update references to new workflow structure
  - [ ] Clarify GitHub Actions UI navigation
  - [ ] Enhance troubleshooting notes
  - [ ] Reference local deployment for comparison
- [ ] **2.8** Revise current Section 4-5 → new Section 6 (Branch protection & PR workflow)
  - [ ] Keep existing structure
  - [ ] Update workflow reference names
  - [ ] Enhance explanation of protective measures
  - [ ] Add workflow run inspection guidance

### Phase 3: Visual Assets
- [ ] Take screenshot: deploy.py in VS Code
- [ ] Take screenshot: Fabric workspace ID location
- [ ] Take screenshot: Terminal successful local deployment
- [ ] Take screenshot: Items in Fabric after deployment
- [ ] Take screenshot: .github/workflows/deploy.yml in VS Code
- [ ] Take screenshot: GitHub Actions workflow run
- [ ] Take screenshot: GitHub Actions job details
- [ ] Take screenshot: Successful workflow completion
- [ ] Create or adapt: Deployment architecture diagram (Mermaid)
- [ ] Create or adapt: Local vs. Automated deployment flow diagram

### Phase 4: Enhancement & Quality
- [ ] **3.1** Add "Prerequisites for Lab 2" section with fabric-cicd and dependencies
- [ ] **3.2** Add conceptual diagrams (Mermaid format)
  - [ ] Local deployment flow
  - [ ] Automated GitHub Actions flow
- [ ] **3.3** Add "Common Issues & Troubleshooting" section
  - [ ] Authentication issues
  - [ ] Workspace identification problems
  - [ ] Credential/secret problems
  - [ ] Workflow timing/quotas
- [ ] **3.4** Add "Next Steps & Advanced Topics" section
  - [ ] Multi-environment deployments
  - [ ] Custom parameterization
  - [ ] Performance optimization
  - [ ] Links to fabric-cicd advanced docs
- [ ] **3.5** Review terminology consistency
  - [ ] fabric-cicd naming convention
  - [ ] Semantic model vs. dataset
  - [ ] Consistent abbreviation use
- [ ] **3.6** Technical accuracy review
  - [ ] Test all code examples locally
  - [ ] Verify all links are current
  - [ ] Cross-reference Microsoft docs
  - [ ] Test all command examples
- [ ] **3.7** Workshop delivery notes
  - [ ] Add timing estimates for each section
  - [ ] Document hand-on activity checkpoints
  - [ ] Create instructor notes (separate file)

### Phase 5: Testing & Validation
- [ ] **4.1** Full walkthrough: Follow lab.md start to finish
- [ ] **4.2** Validate all code snippets
  - [ ] Test deploy.py locally
  - [ ] Test GitHub Actions workflow
  - [ ] Verify all command examples
- [ ] **4.3** Review all screenshots
  - [ ] Ensure they match current UI (Fabric, GitHub, VS Code)
  - [ ] Verify screenshots are clear and labeled
  - [ ] Check for sensitive data exposure
- [ ] **4.4** Peer review with workshop co-facilitators
- [ ] **4.5** Test with fresh participant accounts (if possible)

### Phase 6: Documentation & Delivery
- [ ] Create instructor guide (separate file: `INSTRUCTOR_GUIDE.md`)
  - [ ] Timing breakdown per section
  - [ ] Troubleshooting guide
  - [ ] Common participant questions
  - [ ] Assessment criteria
- [ ] Create participant cheat sheet (optional: `.labs/lab2/CHEATSHEET.md`)
  - [ ] Key commands
  - [ ] Common troubleshooting
  - [ ] Useful links
  - [ ] Post-workshop resources
- [ ] Update main README.md if needed
  - [ ] Add fabric-cicd to prerequisites
  - [ ] Add Python version requirement
  - [ ] Link to implementation plan (optional)
- [ ] Create quick reference guide for CI/CD concepts
- [ ] Prepare slide deck/presentation materials (if not already existing)

### Phase 7: Post-Workshop
- [ ] **5.1** Gather participant feedback on lab
- [ ] **5.2** Document lessons learned
- [ ] **5.3** Update plan based on workshop results
- [ ] **5.4** Create FAQ based on participant questions
- [ ] **5.5** Review Microsoft docs for updates (quarterly)

---

## 4. Pedagogical Considerations for Workshop Delivery

### 4.1 Timing and Pacing

**Current Lab Duration:** 90 minutes  
**Recommended Addition:** +20-30 minutes for new content  
**Suggested Total:** 110-120 minutes (1.75-2 hours)

**Proposed Breakdown:**
- Section 0 (Setup): 20 min (prerequisite, partially done before lab)
- Section 1 (fabric-cicd intro): 5 min (conceptual overview)
- Section 2 (GitHub setup): 15 min (mostly configuration)
- **Section 3 (Local deployment): 25 min** ← NEW, hands-on critical
- **Section 4 (GitHub Actions setup): 20 min** ← EXPANDED from current
- Section 5 (First automated deployment): 15 min
- Section 6 (Branch protection & PR): 20 min (current sections combined)

### 4.2 Learning Outcomes

**Add to lab objectives:**
- ✨ Understand the fabric-cicd Python library and its role in PBIP deployments
- ✨ Create and execute a local deployment script using fabric-cicd
- ✨ Configure environment-specific parameterization with parameter.yml
- ✨ Automate PBIP deployments using GitHub Actions and fabric-cicd

### 4.3 Hands-On Activities

**Hands-on checkpoints where participants must demonstrate:**

1. **After Section 0:** Service principal created and credentials saved securely
2. **After Section 2:** GitHub repository created with secrets/variables configured
3. **After Section 3 (NEW):** Local deployment script executes successfully
4. **After Section 4:** GitHub Actions workflow file created (.github/workflows/deploy.yml)
5. **After Section 5:** GitHub Actions deployment completes successfully
6. **After Section 6:** Pull request created, tested, and merged with protection rules active

### 4.4 Difficulty Progression

- **Beginner:** GitHub repository setup, basic secret management
- **Intermediate:** Understanding Python deploy.py, local deployment execution
- **Advanced:** Parameterization with parameter.yml, workflow customization, multi-environment setup

### 4.5 Common Stumbling Blocks & Mitigation

**Likely Issues & Solutions:**

| Issue | Solution | Prevention |
|-------|----------|-----------|
| Workspace ID vs. Workspace Name confusion | Provide clear screenshot showing where to find ID in Fabric portal | Document both, show examples |
| InteractiveBrowserCredential browser window doesn't appear | Check if running in restricted environment; show alternative GitHub authentication | Mention early, have backup auth method |
| deploy.py import errors | Ensure fabric-cicd installed; check Python version 3.9-3.12 | Verify in prereqs section; test command provided |
| GitHub Actions authentication failures | Check secrets are exactly correct; validate AZURE_CREDENTIALS JSON format | Provide validation checklist |
| Data not appearing after deployment | Missing manual refresh of semantic model (documented transition issue) | Add callout box, emphasize in instructions |
| Workflow not triggering on push | Check .github/workflows/ path; verify branch matches trigger | Show correct file structure screenshot |

---

## 5. Alignment Assessment: Current vs. Official Approach

### 5.1 Current Lab Coverage

| Official Docs Topic | Current Lab Coverage | Status |
|---------------------|---------------------|--------|
| Why fabric-cicd (advantages) | ❌ Not covered | **CRITICAL GAP** |
| Prerequisites (Python, etc.) | ⚠️ Partial (Python in main README) | **Minor Gap** |
| fabric-cicd installation | ❌ Not covered | **CRITICAL GAP** |
| PBIP project preparation | ✅ Full (from Lab 1) | ✅ Covered |
| Local manual deploy.py | ❌ Not covered | **CRITICAL GAP** |
| Running deploy script locally | ❌ Not covered | **CRITICAL GAP** |
| parameter.yml configuration | ❌ Not covered | **GAP** |
| Environment-specific parameterization | ❌ Not covered | **GAP** |
| GitHub Actions workflow setup | ⚠️ Partial (seems pre-built) | ⚠️ Unclear |
| Service principal creation | ✅ Full | ✅ Covered |
| Adding SP to workspace | ✅ Full | ✅ Covered |
| Workspace setup | ✅ Full | ✅ Covered |
| Azure DevOps alternative | ❌ Not covered | ✅ OK (GitHub focus) |
| PR workflow | ✅ Full | ✅ Covered |
| Branch protection | ✅ Full | ✅ Covered |
| Data refresh & credentials | ✅ Full | ✅ Covered |

**Alignment Score: 60%** (needs significant enhancement to reach 90%+)

### 5.2 This Plan's Expected Impact

After implementing this plan:
- **New Alignment Score: 90%+**
- **Critical Gaps Addressed: 100%**
- **Lab completeness:** From "basic CI/CD setup" to "comprehensive fabric-cicd deployment"

---

## 6. Implementation Resources & References

### 6.1 Official Microsoft Documentation
- [Deploy Power BI projects (PBIP) using fabric-cicd](https://learn.microsoft.com/en-us/power-bi/developer/projects/projects-deploy-fabric-cicd) - PRIMARY REFERENCE
- [fabric-cicd Documentation](https://microsoft.github.io/fabric-cicd/latest/) - TECHNICAL REFERENCE
- [fabric-cicd GitHub Repository](https://github.com/microsoft/fabric-cicd) - SOURCE
- [Power BI Desktop Projects (PBIP) Overview](https://learn.microsoft.com/en-us/power-bi/developer/projects/projects-overview)
- [Service principals can call Fabric public APIs](https://learn.microsoft.com/en-us/fabric/admin/service-admin-portal-developer#service-principals-can-call-fabric-public-apis)

### 6.2 Workshop Delivery Resources
- Current Lab 1 materials (for PBIP project understanding)
- Main README.md (for context and prerequisites)
- Resources folder with existing screenshots

### 6.3 External Tools & Services
- GitHub Actions documentation
- Azure DevOps (for alternative path, if desired)
- Python documentation (3.9-3.12)
- Visual Studio Code

---

## 7. Notes for Workshop Facilitators

### 7.1 Before the Workshop
- [ ] Test all code examples in deploy.py locally
- [ ] Ensure all required screenshots are accurate for current UI versions
- [ ] Verify Fabric tenant has sufficient capacity for deployments
- [ ] Check service principal doesn't have restrictions
- [ ] Test GitHub Actions runners are available (may have quotas)
- [ ] Prepare backup workspace in case of issues
- [ ] Create a demo deployment before workshop

### 7.2 During the Workshop
- **Section 3 (Local Deployment) is CRITICAL** - spend extra time here since it's new
- Watch for participants struggling with Python/fabric-cicd installation
- Have detailed troubleshooting guide ready
- Be prepared to help with workspace ID lookup in Fabric portal
- Mention that the pre-configured GitHub Actions workflow is what runs after push
- Emphasize the concept: same `deploy.py` script runs both locally and in GitHub Actions

### 7.3 After the Workshop
- Collect feedback specifically on new content (Sections 1, 3, 4)
- Document any issues participants encountered
- Update this plan based on real-world workshop results
- Share learnings with Microsoft documentation team if appropriate

---

## 8. Success Criteria

Lab 2 will be considered **successfully updated** when:

- ✅ All critical gaps from official documentation are addressed
- ✅ Participants understand fabric-cicd as the core deployment tool
- ✅ Participants can execute local deployments using deploy.py
- ✅ Participants understand how GitHub Actions automates the same deploy.py
- ✅ Participants can troubleshoot basic deployment issues
- ✅ All code examples are tested and verified working
- ✅ All screenshots are current and relevant
- ✅ Lab duration is reasonable for workshop schedule (110-120 min)
- ✅ Participant feedback scores increase (post-feedback comparison)
- ✅ No workshop participants get blocked due to unclear instructions

---

## 9. Version History

| Date | Version | Status | Notes |
|------|---------|--------|-------|
| 2026-02-26 | 1.0 | DRAFT | Initial analysis and planning |
| TBD | 1.1 | IN PROGRESS | Awaiting implementation feedback |
| TBD | 2.0 | FINAL | Post-implementation and testing |

---

## Appendix A: Quick Reference - Old vs. New Structure

**Old Structure (5 sections):**
```
0. Setup
1. Create & Configure GitHub Repo
2. First Automated CI/CD Deployment
3. Set up Branch Protection Rules
4. Start Work in New Branch & Create PR
```

**New Structure (7 sections):**
```
0. Setup (revised)
1. Introduction to fabric-cicd (NEW)
2. Create & Configure GitHub Repository (revised)
3. Local Manual Deployment with fabric-cicd (NEW)
4. GitHub Actions Automation Setup (expanded)
5. First Automated Deployment (revised)
6. Branch Protection & Pull Request Workflow (expanded)
```

---

## Appendix B: Key Insights from Official Documentation

**Why fabric-cicd matters:**
- It's the Microsoft-recommended approach for PBIP deployments
- Provides built-in parameterization for multi-environment support
- Integrates with both GitHub Actions and Azure DevOps
- Offers flexible deployment control (item type filtering, orphan cleanup, etc.)
- Uses official Fabric REST APIs (future-proof)

**Key feature: Parameterization**
- Enables dev/staging/prod deployments with different workspace IDs
- Supports find/replace for connection strings, lakehouse IDs, etc.
- Example: Change workspace reference from dev to prod without code changes

**Learning progression (Microsoft's recommended path):**
1. Understand PBIP format (Lab 1) ✅
2. Learn fabric-cicd tool locally (NEW) ← We're adding this
3. Automate with GitHub Actions (REVISE) ← We're enhancing this
4. Optimize for multiple environments (ADVANCED) ← Could be Lab 3 follow-up

---

**Document Prepared By:** Implementation Assistant  
**For:** Power BI for Developers Workshop (PBIP & CI/CD)  
**Feedback:** This plan should be reviewed and approved before implementation begins.
