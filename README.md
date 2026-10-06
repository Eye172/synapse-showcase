# Synapse
### Understand the movement while the set is still happening.

An exercise-analysis application combining camera pose tracking, a body-aligned 3D view, movement rules and optional wearable IMUs. The experience follows a real training session: choose a lift, learn what is being checked, establish camera framing and review the set.

![The working exercise library exposes the movement checks and optional rig requirements for each lift.](synapse-library.png)

*The working exercise library exposes the movement checks and optional rig requirements for each lift.*

## One lift, a few understandable signals

“Your form is wrong” is not useful feedback during a set. Synapse breaks the problem into observations tied to the selected exercise. For a back squat, the application watches trunk angle, knee tracking, depth and left/right symmetry. Other lifts have different rule sets instead of inheriting the same generic posture score.

The library makes those differences visible before training starts. Its filters separate compound and accessory lifts, risk labels and exercises that can use rig-specific measurements. The labels organize the application’s rule coverage; they are not a medical risk assessment.

![The home screen provides a direct path into a lift and makes the empty training history explicit.](synapse-home.png)

*The home screen provides a direct path into a lift and makes the empty training history explicit.*

## Prepare before the camera starts judging

An exercise page explains the lift and what the system watches. Reference footage gives the user a movement example. Training then requires camera access and stable framing; the documented lock period is 1.5 seconds. A missing camera or unstable setup should not produce a fabricated completed set.

![The back-squat instruction view explains the monitored signals before the user starts a session.](synapse-exercise.png)

*The back-squat instruction view explains the monitored signals before the user starts a session.*

### During the set

The camera pipeline uses **MediaPipe’s 33 body landmarks**, with processing targeted around 15 Hz. Measured proportions and the solved camera feed a 3D mannequin aligned with the user. A movement-rule layer converts observations into joint-level feedback, using teal, amber and red to distinguish states.

The important design constraint is continuity. Feedback should follow the observed movement rather than flicker between unrelated model opinions. Exercise rules, repetition state and the short coaching message have separate responsibilities. The offline RuleCoach can operate without a remote language model; optional language-model coaching is constrained to short utterances and summaries.

## Camera and rig have different jobs

The optional Android rig path accepts measurements from five IMUs over UDP. It can supply signals that are difficult to infer robustly from a single camera, such as selected spine-orientation measurements. The web build does not provide the Android UDP connection.

~~~mermaid
flowchart LR
 A[Camera frames] --> B[Pose landmarks]
 B --> C[Body alignment and movement state]
 D[Optional Android IMU rig] --> C
 C --> E[Exercise-specific rules]
 E --> F[Joint feedback and short coaching]
 E --> G[Set metrics and report]
~~~

If the rig disconnects, the camera path can continue with a visible status change; measurements that depend on missing hardware should not be implied to remain available. A lost camera is a different condition because it removes the central observation source.

## Review without building a video archive

The session flow includes a temporary review and a report. Video is ephemeral and is removed on leaving the review or backgrounding the app; retained history is metric-oriented. This is an intentional product boundary: a training record can remain useful without retaining every camera recording.

**Stack:** Expo / React Native, TypeScript, MediaPipe pose tracking, a 3D body visualization, deterministic movement rules and an optional Android sensor path.

The screenshots here show the current web build’s home, library and instruction screens. They do not simulate a live pose session. Physical-rig accuracy, clinical effectiveness and broad real-world coaching reliability are not established by this showcase; the project remains an exercise-analysis prototype.

---

### Built by

**Synapse team**, including [Shakhnazar Akhmer](https://github.com/Eye172).

[More projects](https://github.com/Eye172) · [Contact](mailto:shakh090909@gmail.com)

This repository presents the product and its engineering. Implementation and internal data are maintained separately. Screenshots and documented experiments are identified in their captions.
