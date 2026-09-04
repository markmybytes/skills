---
name: style-review
description: Audits code against workspace STYLE.md and outputs a structured violation log.
---

# Style Review Playbook

> **CRITICAL:** Do not ask the user for confirmation, automatically execute the matching action below immediately upon invocation.

## 1. Execution Matrix & Input Routing

Parse input arguments against this matrix before beginning analysis:

| Input                              | Mode          | Action                                                             |
| :--------------------------------- | :------------ | :----------------------------------------------------------------- |
| `/style-review`                    | **Diff**      | Run `git diff HEAD`. Audit only modified lines against `STYLE.md`. |
| `/style-review all` \| `workspace` | **Workspace** | Scan `app/` and `resources/js/` globally against `STYLE.md`.       |
| `/style-review [path]`             | **Targeted**  | Isolate focus strictly to the specified file or directory path.    |

_Fallback: If the environment lacks git tools, prompt the user for a path or the `--all` flag._

---

## 2. Workflow

1. **Fetch Rules:** Read root `STYLE.md` dynamically to ensure the latest conventions are used.
2. **Resolve Scope:** Establish the target footprint based on the Matrix above.
3. **Audit:** Evaluate target code directly against the rules extracted from `STYLE.md`.

---

## 3. Output Format

Group findings by file. Do not output markdown code blocks or code snippets.

### 📄 `[Path/To/File]`

- **Line [Number]**
    - **Rule:** `[Rule Name/Category]`
    - **Violation:** [Brief description of the non-compliant code found]
    - **Expected:** [Clear declaration of the architecture required by STYLE.md]

---

## 4. Post-Review Verification

Conclude by reminding the user to run the exact validation commands required for the modified files. The canonical list lives in the **Quality & Testing** section of `README.md` and the **Verification Order** section of `AGENTS.md` (e.g., `./vendor/bin/pint`, `npm run lint`, `npm run type-check`).