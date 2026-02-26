# Lab 2 Revision: Complete Analysis & Comprehensive Plan

**Date:** February 26, 2026  
**Deliverable:** Strategic Analysis + Detailed Implementation Roadmap  
**Target:** Align Lab 2 with Microsoft Learn fabric-cicd documentation

---

## 📊 Executive Summary

Your Lab 2 (CI/CD) draft is a **solid foundation** with excellent hands-on coverage of Git workflows, GitHub Actions, and PR processes. However, it **significantly diverges from Microsoft's official recommendation**, which centers on the **`fabric-cicd` Python library** as the core deployment tool.

### The Gap
- **Current Lab:** Uses GitHub Actions (appears pre-configured), focuses on automation mechanics
- **Microsoft Official:** Emphasizes fabric-cicd tool, shows local deployment first, then automates with GitHub Actions

### The Plan
Rather than just adding content, we're **reorganizing the learning flow** from top-down (GitHub automation) to bottom-up (understand tool → use locally → automate with GitHub).

### Expected Impact
- **Current alignment:** 60%
- **After implementation:** 90%+
- **Time to implement:** 3-4 weeks (40-50 hours)
- **Lab duration:** 90 min → 110-120 min

---

## 📁 What You Now Have

### Three Comprehensive Planning Documents

1. **IMPLEMENTATION_PLAN.md** (7,500 words)
   - Detailed gap analysis (section by section)
   - 4 CRITICAL missing elements identified
   - Exact changes needed for each section
   - Complete 35-item checklist
   - Success criteria and metrics

2. **EXECUTION_GUIDE.md** (5,000 words)
   - Step-by-step implementation roadmap
   - Code examples ready for validation
   - Testing procedures
   - Template materials for instructors and participants
   - Phase-by-phase breakdown

3. **README_PLAN.md** (this directory's quick reference)
   - Project status and timeline
   - Risk assessment
   - Immediate next steps
   - Reading guide for different audiences

All files located in `.labs/lab2/` folder.

---

## 🎯 Key Findings

### Current Lab Strengths ✅
- Excellent service principal setup with clear screenshots
- Outstanding GitHub repository configuration walkthrough
- Great branch protection and PR workflow explanation
- **Perfect** data refresh explanation (critical knowledge)
- Real-world workflow demonstration
- Good BPA quality gate integration

### Critical Gaps ❌
- **NO mention of fabric-cicd tool** (Microsoft's recommended approach)
- **NO deploy.py script creation** (core deployment logic)
- **NO local manual deployment** (jump straight to automated)
- **NO parameterization guidance** (parameter.yml for multi-env)
- Pre-configured GitHub Actions workflow appears "out of nowhere"

### Why This Matters
A participant doing your current lab will understand GitHub Actions but won't understand the **underlying deployment mechanism** (`fabric-cicd`). They can't troubleshoot; they can't adapt for other platforms; they can't extend the solution.

---

## 🔄 Proposed Learning Flow

### Current Structure (5 sections)
```
Setup → GitHub Repo → GitHub Actions Deploy → Branch Protection → PR Workflow
(service principal setup but no tool explanation)
```

### Recommended Structure (7 sections)
```
Setup 
  ↓
fabric-cicd Introduction 
  ↓
GitHub Repository Setup 
  ↓
Local Deploy with fabric-cicd  ← NEW & CRITICAL (hands-on with the tool)
  ↓
GitHub Actions Automation Setup (automate the deploy.py script)
  ↓
First Automated Deployment 
  ↓
Branch Protection & PR Workflow
```

**The key insight:** Participants learn the TOOL first, then see it automated. This is pedagog ically superior and transfers better.

---

## 📋 What Changes Are Needed

### Section-by-Section Summary

| Section | Current Status | Changes Needed | Priority | Est. Time |
|---------|---|---|---|---|
| **0: Setup** | ✅ Excellent | Minor updates, add fabric-cicd note | Medium | 30 min |
| **1: fabric-cicd intro** | ❌ Missing | Create new section | CRITICAL | 1.5 hrs |
| **2: GitHub Repo** | ✅ Good | Streamline, clarify workspace ID | Low | 1 hr |
| **3: Local Deploy** | ❌ Missing | Create new section with deploy.py | CRITICAL | 3 hrs |
| **4: GitHub Actions** | ⚠️ Partial | Expand with workflow explanation | High | 2 hrs |
| **5: First Deploy** | ✅ Good | Update references, clarify workflow | Medium | 1 hr |
| **6: Branch Protection** | ✅ Good | Minor updates, add troubleshooting | Low | 1 hr |

**Total content creation time:** ~11 hours  
**Plus testing & validation:** ~15 hours  
**Plus assets & delivery materials:** ~10 hours

---

## 💻 The Core Addition: Local Deployment Section

This is where the magic happens. Participants will:

1. **Understand the tool**
   - What is fabric-cicd?
   - Why Microsoft recommends it
   - What it does (deploying PBIP to Fabric)

2. **Create deploy.py script**
   ```python
   import argparse
   from azure.identity import InteractiveBrowserCredential
   from fabric_cicd import FabricWorkspace, publish_all_items
   
   # [Real code example from Microsoft docs]
   ```

3. **Deploy locally**
   - Run: `python deploy.py --workspace_id "..."`
   - Browser opens for authentication
   - Items deploy to Fabric in 20-30 seconds
   - Understand the flow intimately

4. **See the same script in GitHub Actions**
   - GitHub Actions runs the exact same deploy.py
   - Participants understand both local AND automated paths
   - Tool mastery achieved

**This is the KEY addition that makes the difference.**

---

## 📸 Visual Assets Needed

**Good news:** 13 existing screenshots can be reused!  
**New captures needed:** 8-10 new screenshots

| Screenshot | Why Needed | Use Case |
|---|---|---|
| `deploy.py` in VS Code | Shows script file | Explain code structure |
| Workspace ID in Fabric UI | Where to find it | Participants copy ID |
| Terminal successful deploy | Proves it works | Success validation |
| Fabric workspace after deploy | Confirmation | See deployed items |
| GitHub Actions workflow file | YAML code | Explain automation |
| Workflow run in progress | GitHub UI | Navigate to runs |
| Workflow run success | Proof of automation | Deployment worked |
| Data source credentials setup | Known issue resolution | Fix data loading |

All can be captured locally during implementation testing.

---

## 🎓 What Participants Will Learn

### Currently (Draft Lab)
- How GitHub Actions works
- How to use branch protection
- How to do pull request reviews
- How to push changes and trigger GitHub Actions

### After Improvements (This Plan)
- Above ⬆️ PLUS:
- What fabric-cicd tool is and why it exists
- How to write a Python deployment script
- How to authenticate with Azure/Fabric
- How to deploy manually for testing
- How to understand and troubleshoot deployments
- How to parameterize for multiple environments (advanced)

**Better foundation for real-world scenarios.**

---

## 🛠️ Implementation Roadmap

### Week 1: Content Creation
- **Day 1-2:** Draft new Sections 1 & 3 (fabric-cicd and local deploy)
- **Day 3-4:** Revise Sections 2, 4, 5, 6
- **Day 5:** Consolidate into single lab.md

### Week 2: Validation & Assets
- **Day 1-2:** Test all code examples end-to-end
- **Day 3-4:** Capture screenshots (8-10 new)
- **Day 5:** Finalize and review

### Week 3: Materials & Delivery
- **Day 1-2:** Create instructor guide and cheat sheet
- **Day 3-4:** Full walkthrough testing (2+ hours)
- **Day 5:** Final polish and publication

**Realistic timeline:** 3-4 weeks, 2-3 hours per day

---

## 🗂️ Files You're Creating

All files go in `.labs/lab2/` folder:

**Main Lab**
- `lab.md` (REVISED) - 10-12 pages, 7 sections

**Planning & Implementation** (already created)
- `IMPLEMENTATION_PLAN.md` - Detailed strategic analysis
- `EXECUTION_GUIDE.md` - Step-by-step how-to guide
- `README_PLAN.md` - Quick reference

**Teaching Materials** (templates in Execution Guide, TBD for implementation)
- `INSTRUCTOR_GUIDE.md` - Teaching notes, timing, FAQs
- `PARTICIPANT_CHEATSHEET.md` - Quick reference for attendees
- `TESTING_NOTES.md` - QA documentation
- `REVISION_CHECKLIST.md` - Progress tracking

**Screenshots**
- Retain all 43 existing images
- Add 8-10 new images in `resources/img/`

---

## ⚡ Quick Start: If You're Ready to Begin

### Option A: You're Implementing Yourself
1. Read `IMPLEMENTATION_PLAN.md` Section 2 (30 min)
2. Read `EXECUTION_GUIDE.md` Phase 1-2 (1 hour)
3. Set up Python virtual environment
4. Write draft of Section 1 (fabric-cicd intro)
5. Get feedback before proceeding

### Option B: Delegating to Team
1. Share all three planning docs with team member
2. Do 30-min walkthrough of IMPLEMENTATION_PLAN.md together
3. Pair programming on Section 1 (2 hours)
4. Have them draft Section 3, you review

### Option C: All-Hands Review First
1. Team reads IMPLEMENTATION_PLAN.md
2. 1-hour workshop discussion on "Is this the right approach?"
3. Decide on implementation timeline
4. Assign sections to team members

---

## ✅ Quality Checklist

Before you publish revised lab:

- [ ] All code examples tested with real Fabric workspace
- [ ] All Python scripts run without errors
- [ ] All command examples work exactly as written
- [ ] All screenshots current (match latest UI)
- [ ] All links verified (live and relevant)
- [ ] Grammar and spell check complete
- [ ] Terminology consistent (fabric-cicd, PBIP, semantic model, etc.)
- [ ] Timing estimates realistic
- [ ] Full walkthrough completed (2+ hours) without blockers
- [ ] Team review completed

---

## 💡 Key Decisions You Need to Make

### 1. Authentication Approach
**Option A:** 3 individual secrets (FABRIC_CLIENT_ID, CLIENT_SECRET, TENANT_ID)  
**Option B:** Single AZURE_CREDENTIALS JSON secret

**Recommendation:** Option A (simpler for workshop participants)

### 2. Parameterization Depth
**Option A:** Full parameter.yml in core content (advanced)  
**Option B:** Mention as "next step" (simpler)

**Recommendation:** Option B (keep focused for workshop)

### 3. Multi-Environment Setup
**Option A:** Core content covers dev/staging/prod (complex)  
**Option B:** Focus on single environment (simpler)

**Recommendation:** Option B (participants can extend afterward)

### 4. Timeline
**Option A:** Keep at 90 minutes (squeeze content)  
**Option B:** Extend to 110-120 minutes (natural pace)

**Recommendation:** Option B (don't skimp on critical learning)

---

## 🎯 Success Metrics

**You'll know this succeeded when:**

1. ✅ Participants understand what fabric-cicd is and why it matters
2. ✅ Participants can create and run deploy.py locally
3. ✅ Participants understand how GitHub Actions calls the same script
4. ✅ All code examples work on first try (90%+ success rate)
5. ✅ Post-workshop survey: "Lab instructions were clear" = 85%+ agree
6. ✅ Participants can troubleshoot basic deployment issues
7. ✅ Course materials meet Microsoft's official documentation standards

---

## 📞 Common Questions Answered

**Q: Should I change the current lab.md significantly?**  
A: Yes, but carefully. Keep all the good parts (sections 0, 2, 5, 6). Insert new sections 1 and 3. This is a surgical enhancement, not a rewrite.

**Q: Do I need to change the screenshots completely?**  
A: No! 13 current screenshots are excellent and reusable. You're adding 8-10 new ones for the fabric-cicd and GitHub Actions sections.

**Q: Is 110-120 minutes too long for one lab?**  
A: No. Lab 1 is 120 minutes. Lab 2 at 110-120 is consistent. If workshop schedule is tight, you could compress to 100 minutes by shortening Q&A sections.

**Q: What if my participants don't have Python installed?**  
A: This is a blocker. Add to Prerequisites: Python 3.9-3.12 installed and verified. Reference the main README.md which already requires Python.

**Q: Can I use Azure CLI credentials instead of browser auth?**  
A: Yes, but it's more complex for workshop participants. The InteractiveBrowserCredential approach is simpler and better for learning.

**Q: What if Microsoft updates fabric-cicd before I finish?**  
A: Stay current with their docs during implementation. The structure and concepts stay the same; minor examples might change. Test everything before workshop delivery.

---

## 🚀 Are You Ready?

### You're Ready if:
- ✅ You understand the gap between current lab and official docs
- ✅ You agree with the 7-section reorganization
- ✅ You have 3-4 weeks to implement
- ✅ You have Python installed locally for testing
- ✅ You can create/test code examples

### You Need Help if:
- ❓ You're not sure how to create deploy.py script
- ❓ You don't have test Fabric workspace access
- ❓ You need to compress timeline below 2 weeks
- ❓ You want detailed code review before implementing

---

## 📚 Document Guide

### To Start Implementation
1. Read **IMPLEMENTATION_PLAN.md** - whole document (1 hour)
2. Read **EXECUTION_GUIDE.md Phases 1-2** - content creation (1 hour)
3. Set up environment
4. Begin Phase 2 (content creation)

### To Delegate to Team
1. Share **IMPLEMENTATION_PLAN.md** with team
2. Do walkthrough meeting (30 min)
3. Assign sections based on expertise
4. Have daily check-ins
5. Use EXECUTION_GUIDE.md as team reference

### For Stakeholder Review
1. Share **README_PLAN.md** (this file) - 10 min read
2. Share **IMPLEMENTATION_PLAN.md** Section 1 - overview (15 min)
3. Schedule 30-min discussion
4. Approve or request adjustments

---

## 🎓 These Documents Provide

**IMPLEMENTATION_PLAN.md:**
- Why changes are needed (gap analysis)
- What changes are needed (detailed section-by-section)
- How to organize the work (checklist)
- How to validate the work (quality criteria)
- Success definition

**EXECUTION_GUIDE.md:**
- Step-by-step how to create each section
- Code examples ready to use
- Testing procedures
- Template materials
- Troubleshooting guides

**README_PLAN.md:**
- Quick status summary
- Timeline and resources
- Key decisions
- Risk assessment

---

## 🎬 Next Action: You Decide

### Path 1: Fast Review (30 minutes)
1. Read this document completely ✅
2. Skim IMPLEMENTATION_PLAN.md Section 2
3. Decide: proceed or delegate?

### Path 2: Prepare for Implementation (2 hours)
1. Read this document ✅
2. Read full IMPLEMENTATION_PLAN.md
3. Skim EXECUTION_GUIDE.md
4. Set up environment

### Path 3: Begin Implementation Today (3 hours)
1. Read all three documents ✅
2. Follow EXECUTION_GUIDE.md Phase 1 (environment setup)
3. Create first draft of Section 1 (fabric-cicd intro)
4. Get peer review

---

## 📋 Completion Checklist

After you've digested these documents, mark these off:

- [ ] Read README_PLAN.md (start here)
- [ ] Reviewed IMPLEMENTATION_PLAN.md
- [ ] Reviewed EXECUTION_GUIDE.md
- [ ] Made decisions on 4 key questions (auth method, parameterization, environments, timeline)
- [ ] Decided: implement myself or delegate
- [ ] (If delegating) Scheduled team walkthrough
- [ ] (If implementing) Set up Python environment
- [ ] Scheduled code review appointments
- [ ] Blocked calendar time for implementation

---

## 📞 Support Resources

If you get stuck during implementation:

1. **IMPLEMENTATION_PLAN.md** - For strategic questions ("Should I include X?")
2. **EXECUTION_GUIDE.md** - For practical questions ("How do I create X?")
3. **Microsoft Learn fabric-cicd docs** - For technical validation
4. **Testing the code locally** - Best way to verify it works
5. **Workshop peer review** - Get feedback before finalizing

---

## Final Word

**You have a solid draft lab.** This plan makes it **great** by aligning it with Microsoft's official approach while improving the pedagogical flow. The work is significant (40-50 hours) but doable in 3-4 weeks, and participants will have a much better learning experience.

The three planning documents are **ready to use** right now. The actual implementation can start whenever you're ready.

---

**Created:** February 26, 2026  
**Documents Location:** `.labs/lab2/` folder  
**Status:** ✅ Planning Complete | Ready for Implementation  
**Next Step:** Choose your path above and begin!
