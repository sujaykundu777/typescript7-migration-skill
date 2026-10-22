---
name: typescript7-migration
description: Migrate a TypeScript application from any older release to stable TypeScript 7, including compiler and tool compatibility, code fixes, and end-to-end verification. Use for an actual repository upgrade, not a version lookup.
---

# TypeScript 7 migration

Treat this as a repository migration, not a dependency-only bump. Start from a new branch or isolated worktree when requested; preserve unrelated work. Read repository instructions and use its package manager and lockfile.

1. Establish the baseline: installed and declared TypeScript versions, package scripts, `tsconfig` inheritance, bundler/framework versions, code that imports `typescript`, editor or language-service plugins, and current typecheck, unit, build and browser-test results. Record the actual commands and failures.
2. Check the current stable TypeScript 7 release and migration guidance from the [TypeScript team](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/) and [TypeScript 6 release notes](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-6-0.html). For a 5.x or older project, use 6.x as a diagnostic bridge if the direct jump produces many configuration or inference errors; `stableTypeOrdering` in 6.x can help isolate 7.x differences. Do not leave that diagnostic setting on without a reason.
3. Upgrade with the repository's package manager and update its lockfile. Confirm that the installed compiler and the CLI used by the typecheck script are actually 7.x. TypeScript 7.0 does not expose the old compiler API. Inspect tools that import `typescript`; if they still need that API, use the TypeScript team's 6/7 side-by-side compatibility pattern only for those tools, or defer the incompatible integration with a documented reason. Avoid unreviewed nightly or beta releases.
4. Fix the causes of diagnostics. Explicitly review changed defaults for `strict`, `module`, `target`, side-effect imports, `rootDir`, and `types`, plus options deprecated in 6.x that 7.x rejects. Set compiler options to preserve the app's intended behavior rather than disabling checks broadly. Update affected source and tests with meaningful types; avoid blanket `any`, `skipLibCheck`, or suppression directives as migration shortcuts.
5. Re-run typecheck, unit tests, production build, and the repository's end-to-end suite. For browser applications, verify supported browser engines and real user flows; distinguish mocked services from live integrations. Run the existing CI workflow or verify its first run after pushing. Fix regressions found by these checks and repeat only the affected checks plus the required final gates.
6. Hand off the branch, version installed, changed files, command results, and any tooling that remains on a compatibility compiler. A passing `tsc` alone does not establish runtime or browser compatibility.

If the project uses Vue, MDX, Astro, Svelte, Angular, or compiler-API-dependent plugins, check their current TypeScript 7 compatibility before changing their compiler integration. TypeScript 7.0's native compiler may be usable for CLI checks while embedded tooling still needs TypeScript 6.
