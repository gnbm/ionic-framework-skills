# Issue Triaging Guidelines

Comprehensive guide for triaging GitHub issues for Ionic Framework, Ionic Docs, and Ionicons repositories.

## Contents

- [Overview](#overview)
- [Triaging Responsibilities](#triaging-responsibilities)
- [Triaging Workflow](#triaging-workflow)
- [Label System](#label-system)
- [Reproduction Requirements](#reproduction-requirements)
- [Issue Types](#issue-types)
- [Common Scenarios](#common-scenarios)
- [Automation](#automation)
- [FAQ](#faq)

---

## Overview

Issue triaging is a critical part of maintaining Ionic Framework. All team members participate in triaging GitHub issues to ensure timely responses and proper categorization.

**Triaging Goals:**
- Provide first contact within **3 business days**
- Categorize issues accurately
- Identify and close support questions
- Require reproduction cases for bugs
- Root-cause bugs before marking as `type: bug`
- Validate feature requests

**Key Principle:** Better to ask clarifying questions than to guess or make assumptions.

---

## Triaging Responsibilities

### All Team Members

- Monitor GitHub issues for new submissions
- Respond to issues within 3 business days
- Spend 5-10 minutes per issue (max 20 minutes)
- Apply appropriate labels
- Request reproduction cases
- Close support questions

### Triaging Time Limits

- **5-10 minutes** - Target time per issue
- **20 minutes** - Maximum time per issue
- **3 business days** - First contact deadline

If an issue requires more than 20 minutes, it likely needs more information from the reporter or should be escalated.

---

## Triaging Workflow

### Step 1: Read the Issue

- Read the issue description carefully
- Check if it's a bug, feature request, or support question
- Look for reproduction steps
- Check if similar issues exist

### Step 2: Determine Issue Type

**Is it a support question?**
- Issue asks "How do I...?"
- Issue is about using Ionic, not a bug or feature
- ➡️ Add `ionitron: support` label and close

**Is it a bug report?**
- Issue describes unexpected behavior
- ➡️ Request reproduction if not provided
- ➡️ Verify and root-cause before adding `type: bug`

**Is it a feature request?**
- Issue requests new functionality
- ➡️ Validate against acceptance criteria
- ➡️ Add `type: feature request` if valid

**Is it unclear?**
- ➡️ Add `needs: reply` label
- ➡️ Ask clarifying questions

### Step 3: Request Reproduction

If the issue is a bug without a reproduction:
1. Add `ionitron: needs reproduction` label
2. Comment explaining what's needed
3. Issue will auto-close after 14 days if no reproduction is provided

### Step 4: Root-Cause the Bug

Before marking as `type: bug`:
1. Review the reproduction
2. Identify the root cause
3. Determine if it's a framework bug or user error
4. If confirmed bug, add `type: bug` label
5. If user error, explain and close
6. **Document root cause on the tracking ticket**

**CRITICAL:** All bugs must have a root-cause documented on the tracking ticket before the issue can be refined and assigned.

Without a documented root cause:
- ❌ Team cannot evaluate risk associated with the issue
- ❌ Team cannot assign a point value
- ❌ Issue will NOT be refined during refinement meetings
- ❌ Developer cannot effectively start work on a fix

The root cause should explain **why** the bug is happening, not just describe the symptoms. It should be specific enough that an implementer knows where to start looking to fix the issue.

**Examples of root cause documentation:**

❌ **Unhelpful:** "The arrow on the popover renders under the backdrop"
- This only describes the symptom, not why it's happening

❌ **Too vague:** "The arrow on the popover renders under the backdrop because of the styles"
- Too broad, doesn't help the implementer know where to start

✅ **Helpful:** "The arrow on the popover is rendering under the backdrop because the arrow's z-index value is too low. The backdrop has z-index: 100, but the arrow only has z-index: 10."
- Clearly states the root cause and provides specific information about what needs to change

### Step 5: Apply Labels

Apply appropriate labels:
- **Type:** `type: bug`, `type: feature request`
- **Status:** `triage`, `needs: reply`, `ionitron: needs reproduction`, `ionitron: support`
- **Area:** `package: core`, `package: angular`, `package: react`, `package: vue`
- **Priority:** Apply if critical or blocking

### Step 6: Follow Up

- Monitor issues with `needs: reply` label
- Issues with no response after 14 days will auto-close
- Re-engage if the reporter provides additional information

---

## Label System

### Type Labels

| Label | Description | Action |
|-------|-------------|--------|
| `type: bug` | Confirmed bug in the framework | Auto-imported to Jira |
| `type: feature request` | Validated feature request | Auto-imported to Jira |
| `type: docs` | Documentation issue | Route to ionic-docs repo |
| `type: performance` | Performance issue | Investigate and root-cause |

### Status Labels

| Label | Description | Auto-Close |
|-------|-------------|------------|
| `triage` | Needs initial review | No |
| `needs: reply` | Awaiting user response | 14 days |
| `ionitron: needs reproduction` | Requires GitHub repo reproduction | 14 days |
| `ionitron: support` | Support question, not bug/feature | Immediate |
| `ionitron: stale` | No activity for 30 days | 30 days |

### Package Labels

| Label | Description |
|-------|-------------|
| `package: core` | Core web components |
| `package: angular` | Angular integration |
| `package: react` | React integration |
| `package: vue` | Vue integration |
| `package: angular-server` | Angular server integration |

### Component Labels

Apply component-specific labels when applicable:
- `ion-button`, `ion-input`, `ion-modal`, etc.

### Platform Labels

Apply platform labels if the issue is platform-specific:
- `ios`, `android`, `web`, `desktop`

### Priority Labels

| Label | Description |
|-------|-------------|
| `priority: high` | Critical bug or blocking issue |
| `priority: medium` | Important but not blocking |
| `priority: low` | Nice to have, not urgent |

---

## Reproduction Requirements

### What is a Code Reproduction?

A code reproduction is a **minimal, public GitHub repository** that demonstrates the issue.

**Requirements:**
- Must be a GitHub repository (not Plunker, JSFiddle, etc.)
- Must be a standalone Ionic application
- Must contain the minimum code needed to reproduce the issue
- Must include clear reproduction steps
- Must use the latest version of Ionic (or specify version if regression)

### Why Require Reproductions?

- Confirms the issue is reproducible
- Helps isolate the root cause
- Prevents time spent on user errors
- Ensures issue affects the framework, not user code

### How to Request a Reproduction

**Template Comment:**
```markdown
Thanks for the issue! This issue has been labeled as `ionitron: needs reproduction` which requires the submission of a complete project reproducing the issue. A reproduction is required to ensure that we can adequately investigate your issue.

Please take a look at the [Contributing Guide](https://github.com/ionic-team/ionic-framework/blob/main/docs/CONTRIBUTING.md#creating-a-good-code-reproduction) for guidelines on creating a good code reproduction.

Sometimes it can be hard to isolate the cause of an issue. If you are unable to create a reproduction and feel this is not expected behavior, please consider opening a [Discussion](https://github.com/ionic-team/ionic-framework/discussions) instead.

This issue will be closed in 14 days if no reproduction is provided.
```

### Validating Reproductions

When a reproduction is provided:
1. Clone the repository
2. Follow the reproduction steps
3. Verify the issue occurs
4. Identify the root cause
5. If confirmed bug, remove `ionitron: needs reproduction` and add `type: bug`
6. If user error, explain and close

---

## Issue Types

### Bug Reports

**Criteria for `type: bug` label:**
- Issue is reproducible in a minimal test case
- Issue is caused by framework code, not user code
- Issue has been root-caused
- Issue is not expected behavior

**Process:**
1. Request reproduction if not provided
2. Verify reproduction
3. Root-cause the issue
4. Add `type: bug` label
5. Issue is auto-imported to Jira

**Common Invalid Bugs:**
- User errors (incorrect usage)
- Expected behavior (not a bug)
- Third-party library issues
- Environment-specific issues (not framework bug)

### Feature Requests

**Criteria for `type: feature request` label:**
- Concrete use case provided
- Solves a general problem in Ionic
- Burden of not having feature > burden of building/maintaining it
- Feature aligns with Ionic's vision

**Process:**
1. Validate against acceptance criteria
2. Ask clarifying questions if needed
3. Add `type: feature request` if valid
4. Issue is auto-imported to Jira
5. Close if feature does not meet criteria

**Feature Acceptance Criteria:**
1. Does the request have a **concrete use case**?
2. Is the burden of not having this feature **greater than** the burden of building/maintaining it?
3. Does the feature solve a **general problem** in Ionic?

**Common Invalid Feature Requests:**
- Already exists in the framework
- Too specific to the reporter's use case
- Out of scope for Ionic Framework
- Better solved with userland code

### Support Questions

**Criteria for `ionitron: support` label:**
- Issue asks "How do I...?"
- Issue is about using Ionic, not a bug or feature
- Issue is a usage question

**Process:**
1. Add `ionitron: support` label
2. Redirect to appropriate support channel
3. Close the issue

**Template Comment:**
```markdown
Thanks for the issue! This issue appears to be a usage question, not a bug report or feature request. We use the issue tracker exclusively for bug reports and feature requests.

For usage questions, please visit one of the following:
- [Ionic Forum](https://forum.ionicframework.com/)
- [Ionic Discord](https://ionic.link/discord)
- [Ionic Docs](https://ionicframework.com/docs)

This issue will be closed. Thank you for using Ionic!
```

---

## Common Scenarios

### Scenario 1: Bug Report Without Reproduction

**Action:**
1. Add `ionitron: needs reproduction` label
2. Comment requesting reproduction
3. Issue auto-closes after 14 days if no reproduction

### Scenario 2: Bug Report With Reproduction

**Action:**
1. Clone and verify reproduction
2. Root-cause the issue
3. If confirmed bug: add `type: bug`, remove `triage`
4. If user error: explain and close

### Scenario 3: Feature Request

**Action:**
1. Validate against acceptance criteria
2. If valid: add `type: feature request`
3. If invalid: explain and close

### Scenario 4: Support Question

**Action:**
1. Add `ionitron: support` label
2. Redirect to forum/Discord
3. Close immediately

### Scenario 5: Duplicate Issue

**Action:**
1. Add `duplicate` label
2. Comment with link to original issue
3. Close as duplicate

### Scenario 6: Cannot Reproduce

**Action:**
1. Add `needs: reply` label
2. Comment explaining you cannot reproduce
3. Request additional information
4. Issue auto-closes after 14 days if no response

### Scenario 7: Issue Requires More Information

**Action:**
1. Add `needs: reply` label
2. Comment asking specific questions
3. Issue auto-closes after 14 days if no response

---

## Automation

### Ionitron Bot

The Ionitron bot automates certain triaging tasks:

**Auto-Close Behaviors:**
- Issues with `ionitron: needs reproduction` label → Closes after 14 days
- Issues with `needs: reply` label → Closes after 14 days
- Issues with `ionitron: stale` label → Closes after 30 days
- Issues with `ionitron: support` label → Closes immediately

**Auto-Import to Jira:**
- Issues with `type: bug` label → Auto-imported to Jira
- Issues with `type: feature request` label → Auto-imported to Jira

**Auto-Comment:**
- Issues with `ionitron: needs reproduction` → Posts reproduction request
- Issues with `needs: reply` → Posts reminder after 14 days
- Issues with `ionitron: support` → Posts support redirect

---

## FAQ

### How do I know if an issue is a bug or user error?

1. Review the reproduction
2. Check if the behavior matches the documentation
3. Look for similar closed issues
4. Test with the latest version of Ionic
5. Identify the root cause

If the behavior is caused by framework code and contradicts documentation or expected behavior, it's a bug. Otherwise, it's likely a user error.

### What if I can't reproduce the issue?

1. Add `needs: reply` label
2. Comment explaining you cannot reproduce
3. Ask for more information (version, steps, environment)
4. Wait for response

If no response after 14 days, the issue will auto-close.

### Should I close issues that are old?

Only if:
- Issue has `ionitron: stale` label and no recent activity
- Issue has `needs: reply` or `ionitron: needs reproduction` with no response after 14 days
- Issue has `ionitron: support` label

Do not close issues just because they are old. Some bugs may affect older versions still in use.

### What if the reproduction is too complex?

1. Add `needs: reply` label
2. Request a **minimal** reproduction
3. Explain why a minimal reproduction is needed
4. Provide link to Contributing Guide

### What if the issue is a third-party library problem?

1. Identify the third-party library
2. Comment explaining it's a third-party issue
3. Provide link to the third-party issue tracker
4. Close the issue

### What if the issue is platform-specific?

1. Add platform label (`ios`, `android`, `web`, `desktop`)
2. Test on the specific platform if possible
3. Add `type: bug` if confirmed
4. Note in comments which platforms are affected

### What if I'm not sure about the issue?

1. Add `triage` label if not already present
2. Ask clarifying questions
3. Tag team members for input
4. Better to ask than to guess

### How do I handle issues with multiple problems?

1. Ask the reporter to split into separate issues
2. Address the primary issue first
3. Request new issues for additional problems
4. This helps with tracking and resolution

---

## Key Resources

- **Contributing Guide:** [https://github.com/ionic-team/ionic-framework/blob/main/docs/CONTRIBUTING.md](https://github.com/ionic-team/ionic-framework/blob/main/docs/CONTRIBUTING.md)
- **Code of Conduct:** [https://ionicframework.com/docs/contributing/coc](https://ionicframework.com/docs/contributing/coc)
- **Ionic Forum:** [https://forum.ionicframework.com/](https://forum.ionicframework.com/)
- **Ionic Discord:** [https://ionic.link/discord](https://ionic.link/discord)
- **Issue Templates:** `.github/ISSUE_TEMPLATE/` directory
- **Ionitron Bot:** Automated triaging assistant
