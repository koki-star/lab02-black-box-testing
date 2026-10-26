# Lab 02: Black-box test design

**Website:** Stopwatch.now  
**URL:** https://stopwatch.now/countdown  
**Features:** Countdown duration input, countdown controls, sound, and auto-repeat.

Codex inspected the rendered page only. The observations below came from short browser tests on October 25, 2026. An expected result is a test goal, not a claim that the test passed.

## 1. Equivalence partitions: duration input

The page has Hours, Minutes, and Seconds fields. These partitions use the total duration. No maximum duration was established, so this draft does not claim one.

| Partition | Representative input (H:M:S) | Expected result | Observed |
| --- | --- | --- | --- |
| Valid, positive time under one minute | 0:0:3 | Displays 00:03 and counts down when Start is clicked | Displayed 00:03 and reached 00:00 |
| Positive duration using 60 seconds | 0:0:60 | Displays an equivalent one-minute duration | Displayed 01:00 while Seconds still showed 60; countdown not run |
| Invalid, zero total | 0:0:0 | Start should not begin a countdown | Start left the display at 00:00 |
| Invalid, negative value | 0:0:-1 | Reject the value and show a valid duration | Field accepted -1 and display showed -1:-1; expected result not met |
| Invalid, letters | 0:0:abc | Reject nonnumeric input | Browser input rejected `abc`; countdown not run |

## 2. Decision table: sound and repeat at finish

For this table, **sound on** means Beep is selected; **sound off** means None. Each case uses 0:0:2. Sound playback needs a person listening. The repeat action is an expectation based on the Auto-repeat label, not a verified pass.

| Case | Sound on? | Repeat on? | Expected action at 00:00 | Test status |
| --- | --- | --- | --- | --- |
| D1 | No | No | Stay finished; no sound | Untested |
| D2 | Yes | No | Stay finished; play Beep | Untested |
| D3 | No | Yes | Start another two-second cycle; no sound | None and Auto-repeat selected; loop and audio not confirmed |
| D4 | Yes | Yes | Start another two-second cycle; play Beep | Beep and Auto-repeat selected; loop and audio not confirmed |

**Matching test cases:** For D1-D4, set the listed sound and repeat choices, enter 0:0:2, click Start, and check the action at 00:00. Audio results remain untested. In the two repeat runs, the display reached 00:00 and Start appeared; this was not enough to confirm a continuous loop.

## 3. State transitions: countdown controls

**States:** Ready shows the set duration and Start; Running counts down and shows Pause; Paused holds the remaining time and shows Resume; Finished shows 00:00 and Start. **Events:** Start, Pause, Resume, Reset, and reaching zero. A missing control makes that event invalid in the listed state.

Auto-repeat is off for these state-transition cases.

| Case | From | Event | Expected next state | Valid? | Test status |
| --- | --- | --- | --- | --- | --- |
| S1 | Ready | Start with 0:0:3 | Running | Yes | Observed countdown and Pause |
| S2 | Running | Pause | Paused | Yes | Observed Resume and held time |
| S3 | Paused | Resume | Running | Yes | Expected, untested |
| S4 | Paused | Reset | Ready | Yes | Observed set duration and Start |
| S5 | Running | Reach 00:00 | Finished | Yes | Observed 00:00 and Start |
| S6 | Finished | Reset | Ready | Yes | Expected, untested |
| S7 | Ready | Pause | Remain Ready | No | No Pause control shown in Ready |
| S8 | Finished | Resume | Remain Finished | No | No Resume control shown at 00:00 |

**Matching test cases:** S1-S6 use the listed starting state, trigger the event, and compare the visible display and button with the expected next state. S7 and S8 check that the unavailable control cannot be used. Results are limited to the observations in the table.

