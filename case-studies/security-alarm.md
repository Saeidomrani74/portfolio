# Alarm App: Controlling an SMS Alarm System from Android

## Problem
Users need a mobile interface to control an alarm system through alarm commands. When a user taps an action button in the app, it sends the corresponding command to the alarm system. The alarm system responds via SMS, and the app reads that incoming reply to track the system's status.

## Constraints
The application targets Android only and was tested on a Poco F5, with no iOS version. Because the app talks to the alarm system only by SMS, it requires permissions to send, read, and receive SMS messages. On Google Play, access to these SMS permissions is restricted to default SMS handlers and approved exceptions (verified on 14 November 2026). I do not know whether the app was published on Google Play.

## Decisions
Application state is managed with `Provider`, while incoming messages are captured using `flutter_sms_listener`. A single function handles waiting for the alarm system's reply to a command: it takes the expected state, enforces a 120-second timeout, and updates the stored state only if the incoming reply matches that expected state. A `handled` flag is set before the first `await` because a second incoming SMS can trigger the listener callback again while the first callback is still running and updating the state. In my view, this design carries two main trade-offs: the app relies on the alarm system's exact reply wording, and checking only the sender number is a weak verification check.

## Result
I developed the app between March 2025 and February 2026 across 44 Git commits. I manually tested the arm, disarm, half arm, and timeout scenarios on a Poco F5. The codebase does not include automated tests.