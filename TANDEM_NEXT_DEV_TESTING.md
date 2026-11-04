# Testing Tandem on Loop next-dev

> **This branch has a known dosing bug, on purpose, so it can be tested. Never use it for therapy or on a pump
> attached to anyone.**
>
> In testing against an emulator, Loop sent doses to the pump but never recorded them, so that insulin wasn't
> counted and Loop kept dosing. This branch exists to find out whether the same thing happens on a real pump.

## What's on this branch
- LoopWorkspace `next-dev` at `97b2cfc`. This isn't the latest `next-dev`: later commits change the pump manager
  API that this TandemKit branch was written against.
- TandemKit `trio-next-dev` at `2bfa207a`, added as the `TandemKit` submodule.

Nothing else is changed, and there is no fix for the bug.

## Suspected cause
TandemKit holds back everything it reports to the host until the host has set both `pumpManagerDelegate` and
`delegateQueue` (`canNotifyDelegate` in `TandemPumpManager.swift`). Loop `next-dev` no longer sets `delegateQueue`
for the pump, so those reports may never be sent, while commands to the pump still go out.

## Setup
Follow "Before you start" in `TandemKit/docs/hardware-test-plan.md`:
- Use a pump that isn't attached to anyone, with saline in the cartridge and the tubing end over a container.
- Use a phone and Loop install that nobody relies on for therapy. This build may replace an existing Loop install
  signed with the same team, so don't use a phone with a Loop you need.
- **Leave closed loop off.** If the bug is real, automatic dosing would keep adding insulin that Loop doesn't count.

You need access to `jwoglom/TandemKit`, which is private. Then:

```bash
git clone --branch tandem-next-dev-do-not-use --recurse-submodules https://github.com/elnjensen/LoopWorkspace.git
```

Open `LoopWorkspace.xcworkspace`, select the **LoopWorkspace** scheme, and build. Do a clean build if you've built
this folder before, and check that Tandem Mobi is listed when you add a pump: an incremental build can leave the
plugin out of the app.

## The test
1. Pair the pump with Loop.
2. Give a 0.5 U bolus from Loop.
3. Wait for it to finish, and note whether the pump delivered it.
4. Set a temp basal, if Loop lets you with closed loop off, and repeat the check.
5. On Loop's Insulin Delivery Log and the Active Insulin chart, note whether each dose appears.
6. Leave the pump idle for 15 minutes, then note whether Loop's pump status has updated. Take the phone out of range
   and back, and note whether it reconnects by itself.

## What to report
- **Bug confirmed:** the pump delivers, but the dose never appears in Loop. Please say so even if everything else
  looks normal.
- **Bug not seen:** doses appear in Loop, with the right amounts and times. That would mean the emulator result
  doesn't carry over, which is just as useful to know.
- **Logs:** use "Share Tandem pump logs" in TandemKit's settings, and look for
  `No pump delegate; holding N pump event(s) until one attaches`.
- **Also include:** pump model, firmware and API versions. Logs and state dumps contain the pump's serial number and
  therapy settings, so remove those before posting anywhere public.
