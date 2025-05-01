# VeritasVault – Org-wide Community Files

This repository is **auto-inherited** by every repo in the `veritasvault.ai`
organisation.  
If you want to change a PR template, CODEOWNERS rule, or a reusable CI
workflow, **PR the change here** and it will flow to every project on the next
push.

| Folder / file | What it does |
| ------------- | ------------ |
| `.github/ISSUE_TEMPLATE/` | YAML issue forms – bug & feature. |
| `.github/workflows/reusable-ci.yml` | Lint → Test → Build pipeline all repos call with:<br>`uses: veritasvault/.github/.github/workflows/reusable-ci.yml@main` |
| `.github/labeler.yml` | Path-based auto-labels (<br/>`frontend`, `backend`, `docs`). |
| `CODEOWNERS` | Auto-assign reviewers per directory. |
| `PULL_REQUEST_TEMPLATE.md` | Single PR checklist for every repo. |
| `SECURITY.md` | Vulnerability disclosure policy. |

> **How to test a workflow change locally**
> ```bash
> gh workflow run reusable-ci.yml -R veritasvault/vv-landing
> gh run watch
> ```

---

## Updating

1. Fork & branch on this repo.  
2. Edit files (or add new `.github/workflows/*.yml`).  
3. PR → get ✅ from the reusable-ci itself.  
4. Merge – GitHub will automatically propagate to all child repos.

---

## FAQ

* **Do repo-level templates override these?**  
  Yes. If a child repo defines its own file with the same name, GitHub uses the repo version.

* **Why use YAML instead of Markdown for issue templates?**  
  YAML gives required fields & dropdowns, reducing support back-and-forth.

* **Can I add another reusable workflow?**  
  Absolutely – keep it in `.github/workflows/` and document the `uses:` line here.
