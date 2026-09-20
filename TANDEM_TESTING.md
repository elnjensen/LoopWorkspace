# Tandem testing branch

This branch is LoopKit/LoopWorkspace `dev` plus one commit that adds
[TandemKit](https://github.com/jwoglom/TandemKit) (branch `dev`) as a pump
manager. Nothing else is changed: Loop and every other submodule stay at the
revisions `dev` pins. Built this way, Loop offers **Tandem Mobi** when you add
a pump.

TandemKit is for EXPERIMENTAL USE ONLY.

## Before you start

TandemKit is a **private repository**. Ask its maintainer (jwoglom) for access
first — without it, the steps below fail when they reach the TandemKit
submodule. If you plan to use the GitHub browser build, the `GH_PAT` secret in
your fork also has to be able to read TandemKit, not just your own repos.

## Getting the branch into your fork

Both routes end with a `tandem` branch on your fork. Pick whichever you prefer.

### In the browser

1. On your LoopWorkspace fork, create a branch named `tandem`. GitHub always
   branches from an existing branch, so start it from your `dev`.
2. Open this link, replacing `YOUR-USERNAME` (and the repo name, if your fork
   isn't called `LoopWorkspace`):

   ```
   https://github.com/YOUR-USERNAME/LoopWorkspace/compare/tandem...elnjensen:LoopWorkspace:tandem?expand=1
   ```

   That opens a pull request **into your own fork's `tandem` branch** — not
   into LoopKit.
3. Create the pull request, then merge it.

If the pull request shows conflicts, your `tandem` branch started from
something other than an up-to-date `dev`. The local route below avoids that.

### Locally

If you don't have a clone yet:

```bash
git clone --recurse-submodules https://github.com/YOUR-USERNAME/LoopWorkspace.git
cd LoopWorkspace
```

Then copy this branch into your fork:

```bash
git remote add elnjensen https://github.com/elnjensen/LoopWorkspace.git
git fetch elnjensen tandem
git push origin elnjensen/tandem:refs/heads/tandem
```

Your fork now has a `tandem` branch identical to this one, with no merge
commit. This only works if you don't already have a `tandem` branch carrying
commits of your own.

To build it on your Mac:

```bash
git checkout tandem
git submodule update --init --recursive
```

Open `LoopWorkspace.xcworkspace`, select the **LoopWorkspace** scheme, and
build as usual.

### Signing

If you put your Apple Developer Team ID two directory levels above the
workspace folder, Xcode picks it up and signs automatically. Create
`../../LoopConfigOverride.xcconfig` — so if the workspace is at
`~/Developer/LoopWorkspace`, the file goes at `~/LoopConfigOverride.xcconfig` —
containing:

```
LOOP_DEVELOPMENT_TEAM = ABCDE12345
```

The workspace's own `LoopConfigOverride.xcconfig` includes that path if it
exists. Keeping it outside the repo means it survives fresh clones and branch
switches, and can never be committed by accident. Editing the copy inside the
workspace works too, but leaves a modified file in your clone.
