# TaskMatch

TaskMatch is a local, deterministic B2B decision-support prototype for mapping employer tasks to explicit training-module skill coverage. It deliberately reports coverage and remaining gaps; it does not rate, certify, or qualify learners.

## Run locally

Prerequisite: Node.js 20+.

```powershell
npm.cmd install
npm.cmd run dev
```

Open the local URL shown by Vite. Build a production bundle with `npm.cmd run build`.

## Publish a public competition link (GitHub Pages)

The included workflow publishes the `dist` build whenever `main` is pushed. Create a **public** GitHub repository (for example, `taskmatch`), then run the following in this folder after replacing the URL with your repository URL:

```powershell
git add .
git commit -m "Deploy TaskMatch prototype"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/taskmatch.git
git push -u origin main
```

In the repository, open **Settings → Pages** and select **GitHub Actions** as the source if it is not already selected. Once the workflow completes, the public URL will be:

`https://YOUR-USERNAME.github.io/taskmatch/`

## Architecture

- `src/main.tsx` contains typed domain fixtures, the pure `calculate(task, module)` matching function, app state, and reusable interface components.
- `src/styles.css` provides the responsive desktop-first design system.
- Browser `localStorage` persists the in-progress demo state under `taskmatch-state`; no backend is simulated.

## Data model

`Skill` contains ID, name, and importance. `Task` has employer context and required skill IDs. `Module` has metadata and explicitly provided skill IDs. `State` stores the selected proposal, skill-identification state, reviewer decision, and trace.

## Matching logic

Coverage is the intersection of `Task.skills` and `Module.skills`. The app computes `covered`, `missing`, and `percentage` dynamically on every proposal selection. 100% is **Full skill coverage**, 1–99% is **Partial skill coverage**, and 0% is **No skill coverage**. Changing a module can worsen the match, and gaps remain visible.

## Synthetic vs. real components

All tasks, learner groups, modules, risks, test results, and trace entries are clearly synthetic C06 demo data. The matching calculation and local demo persistence are real client-side behavior. There is no Supabase configuration or external data connection.

## Environment variables

None are required for this local prototype.

## Known limitations

The catalog is seeded in the client rather than a governed database. “Automated Tests” on the Evidence page are a transparent demo evidence register; a production version should include CI tests against persisted data and validated skills taxonomy.

## Next validation step

Validate task wording, skills taxonomy, evidence requirements, language needs, and module outcomes with real employers and training providers before connecting a governed persistence layer.
