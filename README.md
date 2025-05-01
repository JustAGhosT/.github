# VeritasVault – Bootstrap / Automation Scripts

> **Goal:** Stand-up the entire engineering workspace –  
> org-wide templates, five product repos, issue backlog, and an
> auto-populated GitHub Projects roadmap – in one automated run.

---

## 0. Prerequisites

| Tool / Access | Command to verify | Notes |
|---------------|-------------------|-------|
| GitHub CLI (`gh`) v2.46+ | `gh --version` | `winget upgrade GitHub.cli` if older. |
| Projects extension | `gh extension list` | Should list **`gh-projects`**. |
| Auth scopes | `gh auth status` | Needs **`repo, project`** → `gh auth refresh -s repo,project`. |
| PowerShell 7+ | `pwsh -v` | Windows PowerShell 5 works, but PS 7 faster. |
| Org admin rights | — | Must be able to create repos / edit project. |

---

## 1. Folder map

```text
scripts
│ Create-VVRepos.ps1 # ⇢ creates five empty repos 
│ Init-GithubMetaRepo.ps1 # ⇢ pushes CODEOWNERS, templates 
    │ Import-Issues.ps1 # ⇢ converts CSV rows → Issues per repo 
    │ Add-Issues-To-Project.ps1 # ⇢ links those Issues into the VV MVP project 
    │ Update-Project_Fields.ps1 # ⇢ fills Estimate / Sprint / Owner fields 
    │ │ roadmap-expanded.csv # single source of truth for backlog 
    └─ templates\ # content used by meta-repo 
        ├─ CODEOWNERS.md 
        ├─ SECURITY.md 
        ├─ PULL_REQUEST_TEMPLATE.md 
        ├─ ISSUE_TEMPLATE
        └─ workflows\
```

## 2. Execution order

```powershell
cd C:\Dev\PhoenixVC\scripts

# 1) Org-wide templates
.\Init-GithubMetaRepo.ps1

# 2) Product repos with branch-protection & topics
.\Create-VVRepos.ps1

# 3) Convert CSV backlog → real GitHub Issues
.\Import-Issues.ps1          # writes issue-log.json

# 4) Link Issues to VV MVP Project (project number 5 by default)
.\Add-Issues-To-Project.ps1

# 5) Populate Estimate, Sprint, Owner custom fields
.\Update-Project_Fields.ps1

> Tip: run each step once and verify in the UI before chaining in CI.

3. Re-run safety

Script	Idempotent? (safe to re-run)
Init-GithubMetaRepo	Yes – pushes same content, fast-forwards.
Create-VVRepos	Will error if repo exists. Pass -SkipExisting.
Import-Issues	Creates duplicate Issues. Delete/rename log before re-run.
Add-Issues-To-Project	Safe – duplicate URLs ignored by Project.
Update-Project_Fields	Safe – overwrites field values.
4. TODOs before first run
Edit roadmap-expanded.csv if you want to adjust labels / assignees.

Confirm the project number inside Add-Issues-To-Project.ps1 and Update-Project_Fields.ps1 ($Project = 5 → match your URL).

In Create-VVRepos.ps1 adjust:

Default team (--team frontend) or remove if not using Teams.

Branch-protection counts (currently 1 reviewer).

Verify label names in templates\.github\labeler.yml align with labels you reference in CSV.

Happy bootstrapping. ¯\_(ツ)_/¯