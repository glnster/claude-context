# Plan: Year-Based Folder Organization for thoughts/

## Overview
Organize documents generated in `thoughts/` folder by year to improve manual navigation and file browsing, while keeping existing filename conventions with day/month/year stamps.

**Current structure**: Documents flat in category folders
```
thoughts/shared/research/2026-01-25-description.md
thoughts/shared/plans/2026-01-25-ENG-123-description.md
thoughts/shared/handoffs/ENG-123/2026-01-25_14-30-45_ENG-123_description.md
thoughts/shared/prs/456_description.md
```

**Proposed structure**: Year-based organization with date-stamped filenames
```
thoughts/shared/research/2026/2026-01-25-description.md
thoughts/shared/plans/2026/2026-01-25-ENG-123-description.md
thoughts/shared/handoffs/2026/ENG-123/2026-01-25_14-30-45_ENG-123_description.md
thoughts/shared/prs/2026/456_description.md
```

**Benefits**:
- Much easier to manually browse documents by year
- Cleaner directory listing (year folders instead of many files)
- No search/automation impact (agents search recursively)
- Maintains historical record with dated filenames
- Scales well as document volume grows

---

## Files That Need Updates

### 1. `.claude/commands/research_codebase.md`
- Research docs: `thoughts/shared/research/` → `thoughts/shared/research/YYYY/`

**Affected sections**: Lines 88-120 (Metadata gathering and filename specification)

**Change details**:
- Update filename format to include year folder: `thoughts/shared/research/YYYY/YYYY-MM-DD-ENG-XXXX-description.md`
- Add year explanation: "YYYY is the current year (as a folder)"
- Update examples to show year-organized paths
- All other content remains the same

---

### 2. `.claude/commands/create_plan.md`

**Affected sections**: Lines 188-196

**Change details**:
- Update filename format: `thoughts/shared/plans/YYYY/YYYY-MM-DD-ENG-XXXX-description.md`
- Add year explanation: "YYYY is the current year (as a folder)"
- Update examples to show year-organized paths
- All other template and process content remains the same

---

### 3. `.claude/commands/research_codebase_generic.md`

**Change details**: Same as research_codebase.md
- Update filename format: `thoughts/shared/research/YYYY/YYYY-MM-DD-...md`
- Add year explanation in format description
- Update examples to show year-organized paths

---

### 4. `.claude/commands/research_codebase_nt.md`

**Change details**: Same as research_codebase.md (non-tool version)
- Update filename format: `thoughts/shared/research/YYYY/YYYY-MM-DD-...md`
- Add year explanation in format description
- Update examples to show year-organized paths

---

### 5. `.claude/commands/create_plan_generic.md`

**Affected sections**: Lines 171-178

**Change details**: Same as create_plan.md
- Update filename format: `thoughts/shared/plans/YYYY/YYYY-MM-DD-...md`
- Add year explanation in format description
- Update examples to show year-organized paths

---

### 6. `.claude/commands/create_plan_nt.md`

**Affected sections**: Lines 167-174

**Change details**: Same as create_plan.md (non-tool version)
- Update filename format: `thoughts/shared/plans/YYYY/YYYY-MM-DD-...md`
- Add year explanation in format description
- Update examples to show year-organized paths

---

### 7. `.claude/commands/create_handoff.md`

**Affected sections**: Lines 13-24

**Special case**: Handoff documents use nested folder structure
- Current: `thoughts/shared/handoffs/ENG-XXXX/YYYY-MM-DD_HH-MM-SS_ENG-ZZZZ_description.md`
- Proposed: `thoughts/shared/handoffs/YYYY/ENG-XXXX/YYYY-MM-DD_HH-MM-SS_ENG-ZZZZ_description.md`

**Change details**:
- Add year folder as parent: `thoughts/shared/handoffs/YYYY/ENG-XXXX/filename.md`
- Update path description: "YYYY is the current year (as a folder)"
- Update examples to show year-organized paths with ticket folder nested inside
- Adjust folder structure explanation in documentation

---

### 8. `.claude/commands/describe_pr.md`

**Affected sections**: Line 60 (file write instruction)

**Change details**:
- Update path from `thoughts/shared/prs/{number}_description.md`
- To: `thoughts/shared/prs/YYYY/{number}_description.md`
- Update file update command to use new path

---

### 9. `.claude/commands/describe_pr_nt.md`

**Note**: Writes to `/tmp/` not `thoughts/`, no changes needed

---

### 10. `.claude/commands/ci_describe_pr.md`

**Affected sections**: Line 59 (file write instruction)

**Change details**: Same as describe_pr.md
- Update path from `thoughts/shared/prs/{number}_description.md`
- To: `thoughts/shared/prs/YYYY/{number}_description.md`

---

### 11. `.claude/agents/thoughts-locator.md`

**Affected sections**: Lines 24-33 (Directory Structure diagram)

**Change details**:
- Update directory structure to show year-based organization
- Add year subfolders under `research/`, `plans/`, `prs/`, and `handoffs/`
- Show both 2026 and 2025 examples to illustrate the pattern
- Keep existing structure for tickets/ and notes/ (not organized by year)
- Update path correction logic (if any) to account for year folders
- **No impact on search**: Agents search recursively, so year folders are transparent

**Note**: All other sections of thoughts-locator (search strategy, output format) remain the same since recursive searching handles year-based organization automatically.

---

### 12. `.claude/agents/thoughts-analyzer.md`

**No changes needed**

The analyzer agent reads documents, which works regardless of their folder depth. No path patterns or examples need updating.

---

## Implementation Details

### Auto-Creation of Year Folders
- Commands should automatically extract current year from the date
- Create year folder if it doesn't exist
- No manual year selection needed from users or agents
- Example logic: `year = datetime.now().year` → creates folder `2026/`

### Agent Behavior
- **thoughts-locator**: Searches recursively, automatically finds documents in year folders
- **thoughts-analyzer**: Reads documents regardless of folder depth
- **resume_handoff**: Reads from handoff paths, which now include year folder
- **implement_plan**: Reads from plan paths, which now include year folder

### Backward Compatibility
- Existing documents don't need migration
- New documents will be generated in year-organized structure
- Agents search recursively, so old and new documents both accessible
- Users can manually organize old documents later if desired

### Manual Search & Browsing
- `thoughts/shared/research/` now contains year folders (2026/, 2025/, etc.) instead of individual files
- Much cleaner directory listing when browsing manually
- Easy to find documents from specific years
- Maintains dated filenames within each year folder for further organization

### Handoff Special Case
Current handoff structure:
```
thoughts/shared/handoffs/
  ├── ENG-1234/
  │   ├── file1.md
  │   └── file2.md
  └── ENG-5678/
      └── file3.md
```

Proposed handoff structure:
```
thoughts/shared/handoffs/
  ├── 2026/
  │   ├── ENG-1234/
  │   │   ├── file1.md
  │   │   └── file2.md
  │   └── ENG-5678/
  │       └── file3.md
  └── 2025/
      └── ...
```

This adds a year-level organization while keeping ticket folders nested inside.

---

## Affected Command Workflows

1. **research_codebase** (and variants: generic, nt)
   - Generates research docs → writes to `thoughts/shared/research/YYYY/YYYY-MM-DD-...md`

2. **create_plan** (and variants: generic, nt)
   - Generates plans → writes to `thoughts/shared/plans/YYYY/YYYY-MM-DD-...md`

3. **create_handoff**
   - Generates handoff docs → writes to `thoughts/shared/handoffs/YYYY/ENG-XXXX/YYYY-MM-DD_HH-MM-SS-...md`
   - **Note**: This has nested structure (year first, then ticket folder)

4. **describe_pr** (and variants: ci_describe_pr, describe_pr_nt)
   - Generates PR descriptions → writes to `thoughts/shared/prs/YYYY/PR-{number}-description.md`

5. **resume_handoff**
   - Reads handoff docs (no changes to file writing, but may need path updates for reading)

6. **implement_plan**
   - Reads plan docs (no changes needed, but agents may need to understand year-based organization)

---

## Quick Summary of Changes

| File | Type | Change |
|------|------|--------|
| research_codebase.md | Command | Lines ~88-120: Add year folder to research paths |
| research_codebase_generic.md | Command | Lines ~57-65: Add year folder to research paths |
| research_codebase_nt.md | Command | Lines ~82-90: Add year folder to research paths |
| create_plan.md | Command | Lines ~188-196: Add year folder to plan paths |
| create_plan_generic.md | Command | Lines ~171-178: Add year folder to plan paths |
| create_plan_nt.md | Command | Lines ~167-174: Add year folder to plan paths |
| create_handoff.md | Command | Lines ~13-24: Add year folder, nest ticket folders inside |
| describe_pr.md | Command | Line ~60: Add year folder to PR description paths |
| ci_describe_pr.md | Command | Line ~59: Add year folder to PR description paths |
| thoughts-locator.md | Agent | Lines ~24-33: Update directory structure diagram |
| thoughts-analyzer.md | Agent | No changes needed |

---

## Approval Status

✅ **Approved for implementation**

- ✅ Year-based organization works for the use case
- ✅ Handoff structure (year → ENG-XXXX) is acceptable
- ✅ Apply to all categories (research, plans, prs, handoffs)
- ⏳ Implementation pending manual plan commit
