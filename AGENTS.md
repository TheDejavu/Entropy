# Starforge Codebase Rules

## Goal
- Optimize for large player clusters, fast first-join bursts, and cannon workloads.
- Prioritize packet-volume reduction, allocation rate, and cache behavior over cosmetic code style.
- Preserve cannoning mechanics unless the user explicitly approves a behavior change.

## Performance Rules
- Treat `net.minecraft.server.*`, `net.minecraft.network.*`, `net.minecraft.world.entity.*`, and explosion code as hot paths.
- In hot paths, avoid:
  - streams
  - lambdas that allocate per call
  - boxing/unboxing
  - `Optional`
  - temporary collections
  - regex
  - string building in loops
  - repeated map lookups when one lookup will do
- Prefer:
  - primitive math over helper objects
  - primitive/fastutil collections over boxed collections
  - cached fields over recomputation when state is stable for the tick
  - early returns and branch elimination
  - fixed-size arrays or snapshots when iteration dominates
- Memory matters as much as CPU. Reject optimizations that trade a small CPU win for large persistent heap growth.
- Network pressure is usually more important than MSPT in dense player areas. First reduce packet fanout before adding more simulation throttles.

## Dense-Player Working Assumptions
- If players time out while TPS/MSPT look healthy, assume packet fanout or join burst pressure first.
- First-join spikes are likely chunk/bootstrap/tracker pressure, not entity simulation pressure.
- Viewer-side reductions are safer than gameplay changes.
- Prefer reducing unnecessary packets over adding more async mutation.

## Async Safety Rules
- Only move pure compute or snapshot-based work off-thread by default.
- Do not move tracker mutation, `seenBy` iteration, entity state mutation, Bukkit event firing, or packet send state off-thread unless the user explicitly wants the risk.
- If async work touches main-thread state, queue the side effect back to the main thread.
- Any async optimization must have a config gate unless it is obviously behavior-preserving and low risk.

## Cannoning Safety Rules
- Do not change TNT, falling-block, explosion, piston, redstone, chunk-loading, or movement semantics without explicit approval.
- Packet suppression for TNT/falling blocks is acceptable if it is client-side only and does not alter mechanics.
- When in doubt, keep the mechanic and optimize the representation or packet path around it.

## Profiling Rules
- Use spark for high-level confirmation, but assume spark raw exports may only expose metadata snapshots.
- Use `/netdebug` during live incidents; trust it more than a post-timeout spark snapshot for connection state.
- When diagnosing dense-player issues, inspect:
  - packet volume
  - join burst rates
  - tracking fanout
  - chunk send rates
  - compression threshold
- Do not assume MSPT is the problem if ping/timeouts happen with healthy TPS.

## Patch Workflow
- This is a patch-based downstream workspace.
- For generated server changes:
  1. edit `starforge-server`
  2. commit the nested `starforge-server` repo first
  3. run root `rebuildPatches`
  4. run root `applyPatches :starforge-server:compileJava`
  5. commit the outer repo
- Do not run root `applyPatches` before the nested repo commit if you want to keep local generated-source edits.

## Verification Rules
- Minimum verification for server changes:
  - `.\gradlew.bat --no-configure-on-demand rebuildPatches`
  - `.\gradlew.bat --no-configure-on-demand applyPatches :starforge-server:compileJava`
- If a task-combination validation error appears, rerun the tasks in separate Gradle invocations instead of fighting paperweight.

## Current Known Pressure Points
- Dense joins and stacked player areas are constrained by packet fanout more than server-thread CPU.
- Current upstream config defaults seen in sparks are still expensive for this use case:
  - `view-distance=10`
  - `simulation-distance=10`
  - `entity-broadcast-range-percentage=100`
  - player tracking range `48`
  - `player-max-chunk-send-rate=75`
  - `network-compression-threshold=256`
- Prefer tuning those before invasive tracker rewrites.

## Review Standard
- Every optimization should answer:
  - what allocation or packet path is reduced
  - what main-thread work is removed
  - what memory cost is added
  - what mechanic or plugin behavior could regress
