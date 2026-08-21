# Forking

Fork of [anomalyco/opentui](https://github.com/anomalyco/opentui), carrying fixes
bliss needs before they land upstream.

- `main` mirrors upstream.
- `bliss` is a release tag, this file, then our fixes. Rebased, never merged.

`bliss` is a patchset, not a PR branch: one squashed commit per fix, each with an
`Upstream:` trailer naming the PR it is filed as. Drop a commit once its PR is in
the new release — `git rebase` drops only patches upstream took verbatim, so a
squash-merged or review-edited fix will conflict or duplicate instead.

The bottom commit also deletes `packages/core`'s `optionalDependencies` and its
`@opentui/keymap` devDependency, from the manifest and from `bun.lock`, and every
release rewrites those, so rebasing conflicts there — delete them again; it also
patches `packages/native/build.zig` and `packages/core/scripts/build.ts` so a host
build emits every variant matching the host, which linux needs and upstream's
release matrix does not.

Bump:

```sh
git fetch upstream --tags
git rebase --onto vNEW vOLD bliss
git push --force-with-lease origin bliss
```

Then point bliss's `opentui` submodule at the new head.
