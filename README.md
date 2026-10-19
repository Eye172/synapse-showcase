# Synapse
### Understand the movement while the set is still happening.

An exercise-analysis application built around a wearable sensor rig. Five IMUs on the body measure the movement, exercise rules turn those signals into feedback, and the phone camera shows the result on a 3D figure aligned with the user. The experience follows a real training session: choose a lift, learn what is being checked, put on the rig and review the set.

![The working exercise library shows the movement checks and rig measurements for each lift.](synapse-library.png)

*The working exercise library shows the movement checks and rig measurements for each lift.*

## One lift, a few understandable signals

“Your form is wrong” is not useful feedback during a set. Synapse breaks the problem into observations tied to the selected exercise. For a back squat, the application watches trunk angle, knee tracking, depth and left/right symmetry. Other lifts have different rule sets instead of inheriting the same generic posture score.

The library makes those differences visible before training starts. Its filters separate compound and accessory lifts, risk labels and the measurements each exercise takes from the rig. The labels organize the application’s rule coverage; they are not a medical risk assessment.

![The home screen provides a direct path into a lift and makes the empty training history explicit.](synapse-home.png)

*The home screen provides a direct path into a lift and makes the empty training history explicit.*

## Prepare before the set starts

An exercise page explains the lift and what the system watches. Reference footage gives the user a movement example. Training then needs the rig connected and the camera framed so the 3D view can follow the user; the documented framing lock is 1.5 seconds. A missing rig or unstable setup should not produce a fabricated completed set.

![The back-squat instruction view explains the monitored signals before the user starts a session.](synapse-exercise.png)

*The back-squat instruction view explains the monitored signals before the user starts a session.*

### During the set

The **rig of five IMUs** streams motion data to the Android app over UDP. Joint angles, symmetry, tempo and repetitions are calculated from these signals, and a movement-rule layer converts them into joint-level feedback, using teal, amber and red to distinguish states.

The important design constraint is continuity. Feedback should follow the measured movement rather than flicker between unrelated model opinions. Exercise rules, repetition state and the short coaching message have separate responsibilities. The offline RuleCoach can operate without a remote language model; optional language-model coaching is constrained to short utterances and summaries.

## Rig and camera have different jobs

The rig measures; the camera shows. All technique metrics come from the wearable sensors. The camera pipeline uses **MediaPipe’s 33 body landmarks**, processed at around 15 Hz, only to align a 3D mannequin with the user so that each error appears at the right joint.

~~~mermaid
flowchart LR
 A[Wearable rig: 5 IMUs] --> B[Movement signals over UDP]
 B --> C[Exercise-specific rules]
 C --> D[Joint feedback and short coaching]
 C --> E[Set metrics and report]
 F[Camera frames] --> G[Pose landmarks]
 G --> H[3D mannequin aligned with the user]
 D --> H
~~~

If the rig disconnects, measurement stops and the app shows the change instead of inventing values. If the camera is lost, the measurements continue; only the 3D view is affected. The web build does not provide the Android UDP connection, so it shows the interface without live rig data.

## Review without building a video archive

The session flow includes a temporary review and a report. Video is ephemeral and is removed on leaving the review or backgrounding the app; retained history is metric-oriented. This is an intentional product boundary: a training record can remain useful without retaining every camera recording.

**Stack:** Expo / React Native, TypeScript, an Android path for the 5-IMU wearable rig over UDP, deterministic movement rules, MediaPipe pose tracking and a 3D body visualization.

The screenshots here show the current web build’s home, library and instruction screens. They do not simulate a live session. Clinical effectiveness and broad real-world coaching reliability are not established by this showcase; the project remains an exercise-analysis prototype.

---

### Built by

**Synapse team**, including [Shakhnazar Akhmer](https://github.com/Eye172).

[More projects](https://github.com/Eye172) · [Contact](mailto:shakh090909@gmail.com)

This repository presents the product and its engineering. Implementation and internal data are maintained separately. Screenshots and documented experiments are identified in their captions.
