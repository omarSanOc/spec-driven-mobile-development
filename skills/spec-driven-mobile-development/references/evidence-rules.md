# Evidence rules

What counts as proof when planning validation (Stage 3), marking tasks done
(Stage 5) and closing a feature (Stage 6).

## Result states

| State | Meaning |
| --- | --- |
| PASSED | The check was executed and the expected result was observed. |
| FAILED | The check was executed and the result differed. Record what was observed. |
| NOT RUN | The check was not executed yet. |
| BLOCKED | The check could not be executed in this environment. Record the cause and the concrete check still needed. |

Never write PASSED for something that was not executed and observed. A test
name, an unexecuted command, a green build on another target, or "should work"
is not evidence.

## Per target platform

- Record every result with the target platform it was observed on. With a
  single target platform, the column still names it.
- Unit tests of shared code that run on the development machine (Dart VM, JVM,
  Node, a KMP host test) are recorded with platform **Shared**: they prove the
  logic, not how it behaves on a device. Behavior that depends on the platform
  still needs a row per target platform.
- Shared code compiling or passing tests on one target is not evidence for another.
- A screenshot from one platform does not demonstrate another.
- Checks blocked by missing macOS, Xcode, simulator, emulator or physical device
  are reported as BLOCKED.

## Choosing the method

| Claim | Suitable evidence | Not sufficient |
| --- | --- | --- |
| Business rule, calculation, validation | Unit test on every target that runs it | Screenshot |
| Each error case of an API | Unit test with a fake/mock transport, one per case | Manual test of the happy path |
| State transitions (loading → error → retry) | Unit test of the state holder (ViewModel, Bloc, store, presenter) | Visual check alone |
| Layout, insets/safe areas, clipping, large text, dark mode | Screenshot or visual check **on each target platform** | Test on one platform |
| Gestures, back behavior, keyboard | Manual or UI test on each target platform, with steps | Unit test |
| Persistence / survives process death | Kill the process and reopen, per target platform; inspect storage | Screenshot before and after without killing |
| Secure storage / encryption | Inspection of the platform store, or platform test | Screenshot |
| Request cancelled / only one request sent | Network trace, server-side count, or test with a counting fake | Screenshot |
| Nothing sensitive in logs | Test that searches the log output for known secrets; manual log inspection per platform | Code review alone |
| Analytics event sent with the right properties | Test with a fake analytics sink, or the analytics debug view | Code review alone |
| Screen reader announcement | A person listening with TalkBack / VoiceOver, or an instrumented accessibility test | Semantics present in code |
| Performance target | Measured value on a named device and build type | Impression |

## Tests

- Prefer testing observable behavior over mirroring implementation.
- Every bug fix gets a test that fails without the fix, where feasible.
- Never modify, skip or delete a test or a check to hide a failure. Fix the cause
  or report the failure with evidence.
- Adding a test dependency is a decision for the user (see PLAN).

## Completion report

State: what changed, which documents were updated, per-platform results for
every AC, what remains NOT RUN or BLOCKED and why, and the concrete next check
needed to close each open item.
