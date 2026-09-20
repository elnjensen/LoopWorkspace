# Tandem closed beta branch

This branch is LoopKit/LoopWorkspace `dev` plus one commit that adds
[TandemKit](https://github.com/jwoglom/TandemKit) (branch `main`) as a pump
manager. This should be identical to the `dev` branch of Loop, but with the 
option to add **Tandem Mobi** as a pump. TandemKit is a **private repository** 
while we are in closed beta testing, so you will be unable to build unless
you follow the directions below. 

TandemKit is for EXPERIMENTAL USE ONLY.

## Before you start

This branch is for the use of those participating in a closed beta test of using the 
Tandem Mobi with Loop. If you are not a member of the Closed Beta group, you cannot build this branch.

In order to build using this branch, you must send your GitHub user name to Eric Jensen via Direct Message 
on ZulipChat with your request to be included. This is only for expert testers who are experienced with Loop. 
Please let Eric know how long you have used Loop, if you ever tested a new pump manager and if you both have 
a Tandem Mobi and know how to use it. If you don't provide that information, you will receive a rejection. 
If you are accepted to the closed beta group, you will get a reply to your DM with additional information.


## Getting the branch into your fork

Both of these routes end with a `feat/tandem-closed-beta` branch on your fork. Pick
whichever you prefer.

### In the browser

1. On your LoopWorkspace fork, create a branch named
   `feat/tandem-closed-beta`. GitHub always branches from an existing branch,
   so start it from your `dev`.
2. Open this link, replacing `YOUR-USERNAME` (and the repo name, if your fork
   isn't called `LoopWorkspace`):

   ```
   https://github.com/YOUR-USERNAME/LoopWorkspace/compare/feat/tandem-closed-beta...LoopKit:LoopWorkspace:feat/tandem-closed-beta?expand=1
   ```

   That opens a pull request **into your own fork's branch** — not into
   LoopKit/LoopWorkspace itself.
3. Create the pull request, then merge it.

If the pull request shows conflicts, your branch started from something other
than an up-to-date `dev`. The local route below avoids that.

### Locally

If you don't have a clone yet:

```bash
git clone --recurse-submodules https://github.com/YOUR-USERNAME/LoopWorkspace.git
cd LoopWorkspace
```

Then copy this branch into your fork. Skip the first line if you already have
an `upstream` remote pointing at LoopKit:

```bash
git remote add upstream https://github.com/LoopKit/LoopWorkspace.git
git fetch upstream feat/tandem-closed-beta
git push origin upstream/feat/tandem-closed-beta:refs/heads/feat/tandem-closed-beta
```

Your fork now has a `feat/tandem-closed-beta` branch identical to this one,
with no merge commit. This only works if you don't already have a branch by
that name carrying commits of your own.

To build it on your Mac:

```bash
git checkout feat/tandem-closed-beta
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
