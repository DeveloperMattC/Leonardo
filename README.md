# Leonardo

An example of a personality, plus AI behaviors, skills, rules, and workflows, orchestrated as one pack.

Leonardo is a polite assistant with a creative Italian manner. When you reference it, the agent loads this pack and uses it as standing context: how to speak, which rules to obey, which workflows to run, and how to treat optional local extras.

This repository is a public, reusable example. It does not include personal data, credentials, resume text, private repos, or third-party external skills.

Repository: [github.com/DeveloperMattC/Leonardo](https://github.com/DeveloperMattC/Leonardo)

## Quick start

1. Copy `LeonardoAI/` to the root of the project where you want Leonardo available.
2. Copy `.cursor/skills/` and `.cursor/rules/` into that project's `.cursor/` folder (merge if `.cursor` already exists).
3. In Cursor, mention **Leonardo** (or attach the `leonardo` skill).
4. Add your own files under `Behaviors/`, `Outputs/`, or extra personalities as needed.

The agent should glob `LeonardoAI/**/*` at the start of a Leonardo turn and keep that pack as context.

## Layout

```
LeonardoAI/
  Personalities/   Voice and how Leonardo should act
  Rules/           Hard constraints (code and language)
  Workflows/       Named procedures you can invoke
  Behaviors/       Standing conduct (add your own)
  Outputs/         Notes from prior work (add your own)

.cursor/
  skills/          Cursor skills that point at this pack
  rules/           Cursor rules that mirror LeonardoAI/Rules
```

`External Skills/` is supported as a local-only folder. It is not published here. Add it on your machine if you want third-party skills. Do not commit vendor skill copies you are not allowed to share.

## Personalities

### Leonardo

Polite, calm, and inventive. English is the working language, with brief Italian courtesy. Technical terms stay in English. Leonardo is not a caricature.

When referenced, Leonardo:

- Loads the whole `LeonardoAI/` pack first
- Follows `Rules/` even when they conflict with a stylish reply
- Runs a matching file in `Workflows/` when you name it
- Keeps facts intact on resume work (see below)

## Skills

These Cursor skills are set to run when you ask for them (`disable-model-invocation: true`).

| Skill | What it does |
| --- | --- |
| `leonardo` | Activates the personality and loads this pack |
| `get-pr-ci-green` | Uses `gh` to watch a named PR, keep it as a draft, and work CI until required checks are green |

## Workflows

### Improve Resume

Takes resume text and rewrites it into stronger software-development bullets.

- Same employers, titles, dates, tools, and outcomes
- No invented metrics, tech, leadership, or seniority
- Best foot forward, not fiction

Ask Leonardo to improve a resume, or use an **Improve Resume** control in an app that follows this workflow.

### Get PR CI green

Uses the GitHub CLI against the PR you specify.

1. Convert the PR to draft
2. Read live checks
3. Watch running tests
4. Fix blockers in the PR, push, and poll until the new run finishes
5. Repeat until required checks pass

Does not mark the PR ready, merge, enable auto-merge, or force-push.

## Rules

Shipped as markdown under `LeonardoAI/Rules/` and as Cursor rules under `.cursor/rules/`.

| Rule | Intent |
| --- | --- |
| Angular version | Stay on Angular 19 APIs and patterns |
| Control flow | Use `@if`, `@for`, `@switch` instead of `*ngIf`, `*ngFor`, `*ngSwitch` |
| Signals | Prefer signals over observables for UI state |
| Language | Do not use an en dash or em dash in replies, code, or output |

## What this repo does not include

- `External Skills/` (local only, not in git)
- Application source, local models, API keys, or `.env` files
- Personal resume content or private project names
- Third-party skill copies (for example vendor packs under another license)

If you fork this, keep secrets and personal files out of git.

## Adding your own pieces

- **Rules:** add a focused markdown file in `LeonardoAI/Rules/`. For Cursor, add a matching `.mdc` file in `.cursor/rules/`.
- **Workflows:** add a named procedure in `LeonardoAI/Workflows/` and optionally a skill in `.cursor/skills/`.
- **Behaviors:** add standing conduct in `LeonardoAI/Behaviors/`.
- **Outputs:** drop notes or artifacts you want Leonardo to remember.
- **External skills:** create `LeonardoAI/External Skills/` locally. Leave it out of version control.

## License

MIT. See [LICENSE](LICENSE).
