# Fortnite v15.50 Game Flow - Test Plan

## Overview
This document provides a comprehensive test plan to verify that all bugs have been fixed and the game flow works correctly.

## Prerequisites
- Fortnite v15.50 client (Release-15.50)
- OrionGS DLL compiled with latest changes
- At least 1 player connection capability
- Console logging enabled to view debug messages

## Test Cases

### TC1: Lobby Spawn Verification
**Objective:** Verify players spawn correctly in the warmup lobby island

**Steps:**
1. Start the server
2. Connect a player
3. Wait for player to fully load (ServerLoadingScreenDropped called)

**Expected Results:**
- ✅ Player spawns on the warmup island (not in the aircraft)
- ✅ Player can move freely
- ✅ Camera follows the player correctly
- ✅ Console log shows: "Pawn spawned with HP: 100, Shield: 0"

**Pass Criteria:**
- Player is visible and controllable
- No camera lock or black screen
- Player is on warmup island terrain

---

### TC2: Health and Shield Initialization
**Objective:** Verify correct health/shield values on spawn

**Steps:**
1. Connect a player
2. Check player's health bar and shield bar

**Expected Results:**
- ✅ Health = 100 (not 999 or any other value)
- ✅ Shield = 0 (no blue shield bar visible)
- ✅ Console log shows: "Pawn spawned with HP: 100, Shield: 0"

**Pass Criteria:**
- Health bar shows 100/100
- No shield bar is visible
- Player can take damage correctly

---

### TC3: Warmup Countdown Start
**Objective:** Verify warmup countdown starts correctly

**Steps:**
1. Connect at least 1 player
2. Observe console logs
3. Check client UI for countdown timer

**Expected Results:**
- ✅ Console log shows: "Warmup countdown started! End time: X.XX"
- ✅ Countdown timer is visible on client
- ✅ Timer starts at approximately 90 seconds
- ✅ Battle bus does NOT spawn immediately

**Pass Criteria:**
- Countdown is visible and counting down
- No battle bus in the sky
- Players remain in lobby

---

### TC4: Bot Spawning During Warmup
**Objective:** Verify bots spawn in lobby alongside players

**Steps:**
1. Connect a player
2. Wait for bots to spawn (configured in ServerReadyToStartMatch)
3. Observe bot behavior

**Expected Results:**
- ✅ Bots spawn in warmup lobby at designated spawn points
- ✅ Bots have correct cosmetics and can move
- ✅ Bots do not jump immediately
- ✅ No console errors related to bot spawning

**Pass Criteria:**
- Bots are visible in lobby
- Bots are not stuck or frozen
- Bot count matches configuration

---

### TC5: Warmup Countdown Completion
**Objective:** Verify battle bus launches ONLY after countdown expires

**Steps:**
1. Connect a player
2. Wait for warmup countdown to complete (90 seconds)
3. Observe when battle bus spawns

**Expected Results:**
- ✅ At exactly 0 seconds, console log shows: "Warmup countdown finished! Starting match and launching aircraft..."
- ✅ Battle bus spawns and starts flying
- ✅ Players are teleported into the bus
- ✅ Bus follows its designated flight path

**Pass Criteria:**
- Bus spawns only after countdown reaches 0
- All players and bots are in the bus
- No premature bus spawning

---

### TC6: Player Jump from Aircraft
**Objective:** Verify players can jump from bus correctly

**Steps:**
1. Wait for battle bus to reach drop zone
2. Press jump button (Spacebar or configured key)
3. Observe player state

**Expected Results:**
- ✅ Player jumps from bus
- ✅ Skydiving animation activates
- ✅ Player can glide to different locations
- ✅ Console log shows: "Pawn spawned with HP: 100, Shield: 0" (on respawn)
- ✅ Health = 100, Shield = 0 after landing

**Pass Criteria:**
- Jump works on first try
- Skydiving mechanics work correctly
- Player lands with 100 HP / 0 Shield

---

### TC7: Bot Jump from Aircraft
**Objective:** Verify bots jump from bus at different locations

**Steps:**
1. Wait for battle bus drop phase
2. Observe bot behavior
3. Watch bots jump at different times

**Expected Results:**
- ✅ Bots jump from bus automatically
- ✅ Each bot jumps at a different time/location
- ✅ Bots do not all jump simultaneously
- ✅ Bots land at various POIs across the map

**Pass Criteria:**
- At least 50% of bots jump within 30 seconds of drop zone
- Bots are distributed across map (not all at same location)
- No bots stuck in bus after it exits drop zone

---

### TC8: Multiple Players in Lobby
**Objective:** Verify multiple players work together in lobby

**Steps:**
1. Connect 2+ players
2. Observe lobby phase
3. Wait for countdown
4. Wait for bus launch

**Expected Results:**
- ✅ All players spawn in lobby together
- ✅ All players see the same countdown timer
- ✅ All players are teleported to bus simultaneously
- ✅ Each player can jump independently

**Pass Criteria:**
- No desync between players
- Countdown is synchronized
- All players reach bus at same time

---

### TC9: No Premature Aircraft Launch
**Objective:** Verify no redundant or premature startaircraft calls

**Steps:**
1. Enable verbose console logging
2. Connect a player
3. Watch console for "startaircraft" execution

**Expected Results:**
- ✅ "startaircraft" is called ONLY ONCE
- ✅ "startaircraft" is called ONLY AFTER warmup countdown expires
- ✅ No errors about aircraft already spawned
- ✅ Console shows: "Warmup countdown finished! Starting match and launching aircraft..." BEFORE startaircraft

**Pass Criteria:**
- Only one "startaircraft" command in logs
- Command occurs after warmup expiry
- No duplicate aircraft spawns

---

### TC10: Camera and Possession Validation
**Objective:** Verify camera and pawn possession work correctly

**Steps:**
1. Connect a player
2. Observe camera behavior during:
   - Lobby spawn
   - Warmup phase
   - Aircraft boarding
   - Aircraft jump

**Expected Results:**
- ✅ Camera follows player pawn in lobby
- ✅ No black screen or camera lock
- ✅ Camera transitions smoothly to bus interior
- ✅ Camera follows player during skydiving

**Pass Criteria:**
- Camera is always controlled and responsive
- No periods of black screen
- Smooth transitions between phases

---

### TC11: Health Persistence After Jump
**Objective:** Verify health/shield remain correct after aircraft jump

**Steps:**
1. Wait for player to board aircraft
2. Jump from aircraft
3. Land on ground
4. Check health/shield values

**Expected Results:**
- ✅ Health = 100 after landing
- ✅ Shield = 0 after landing
- ✅ Player can be damaged normally
- ✅ Player can collect shield items and gain shield

**Pass Criteria:**
- Health is exactly 100
- Shield is exactly 0
- Normal gameplay mechanics work

---

### TC12: Match Flow Sequence
**Objective:** Verify complete match flow from start to finish

**Steps:**
1. Start server
2. Connect player(s)
3. Observe complete sequence

**Expected Results:**
1. ✅ Player spawns in lobby
2. ✅ Bots spawn in lobby
3. ✅ 90-second countdown starts
4. ✅ Countdown expires
5. ✅ Battle bus spawns
6. ✅ Players/bots board bus
7. ✅ Bus enters drop zone
8. ✅ Players/bots can jump
9. ✅ Match continues normally

**Pass Criteria:**
- All 9 steps occur in order
- No steps are skipped
- No errors in console

---

## Regression Tests

### RT1: Existing Features Still Work
**Objective:** Verify changes don't break existing functionality

**Test:**
- Building/editing
- Weapon usage
- Item pickup
- Vehicle entry/exit
- Emotes
- DBNO/revival
- Rebooting
- Safe zone mechanics

**Expected:** All features work as before

---

### RT2: Performance Check
**Objective:** Verify no performance degradation

**Test:**
1. Measure server tick rate during warmup
2. Measure server tick rate during match
3. Check memory usage

**Expected:**
- No significant performance drop
- Stable tick rate
- No memory leaks

---

## Debug Console Output Examples

### Successful Test Run:
```
Warmup countdown started! End time: 123.45
Pawn spawned with HP: 100, Shield: 0
Pawn spawned with HP: 100, Shield: 0
...
[90 seconds later]
Warmup countdown finished! Starting match and launching aircraft...
```

### Failed Test Run (Old Behavior):
```
Warmup countdown started! End time: 0.10
Warmup countdown finished! Starting match and launching aircraft...
[Immediate bus spawn - FAIL]
```

---

## Test Environment Requirements

### Minimum Configuration:
- 1 player
- 0 bots (bots are optional)
- Console logging enabled

### Recommended Configuration:
- 2+ players
- 10+ bots
- Full logging enabled
- Performance monitoring tools

### Optimal Configuration:
- 4 players in squad
- 50+ bots
- All playlists tested (Solo, Duo, Squad)
- Multiple test runs

---

## Pass/Fail Criteria

### Overall PASS Requirements:
- ✅ All 12 primary test cases pass
- ✅ All 2 regression tests pass
- ✅ No critical errors in console
- ✅ Players can complete full match

### Overall FAIL Conditions:
- ❌ Any test case fails
- ❌ Critical console errors
- ❌ Game crashes
- ❌ Players cannot spawn/jump/play

---

## Known Issues / Limitations

### Non-Critical Issues:
- Warmup countdown is fixed at 90 seconds (not configurable via playlist)
- Bots may occasionally not jump optimally (original game behavior)

### Not Tested:
- Anti-cheat integration (EAC disabled by default)
- Backend integration (optional feature)
- Tournament playlists (requires special configuration)

---

## Test Report Template

```
Test Date: [DATE]
Tester: [NAME]
Build Version: [COMMIT HASH]

Test Case Results:
TC1: [PASS/FAIL] - Notes: [...]
TC2: [PASS/FAIL] - Notes: [...]
TC3: [PASS/FAIL] - Notes: [...]
TC4: [PASS/FAIL] - Notes: [...]
TC5: [PASS/FAIL] - Notes: [...]
TC6: [PASS/FAIL] - Notes: [...]
TC7: [PASS/FAIL] - Notes: [...]
TC8: [PASS/FAIL] - Notes: [...]
TC9: [PASS/FAIL] - Notes: [...]
TC10: [PASS/FAIL] - Notes: [...]
TC11: [PASS/FAIL] - Notes: [...]
TC12: [PASS/FAIL] - Notes: [...]

Regression Tests:
RT1: [PASS/FAIL] - Notes: [...]
RT2: [PASS/FAIL] - Notes: [...]

Overall Result: [PASS/FAIL]
```
