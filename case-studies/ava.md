# AVA: Case Study

**Role:** Flutter developer. I extended and redesigned the Flutter client, which started from an earlier version written by a colleague.
**My work:** redesigned the home page, login screen and chat page; added a professional dashboard, forum pages, a stories provider, banners, OTP input and resend widgets, chat routes and a chat-rules checkbox.
**Stack:** Flutter, Dio, SignalR, Riverpod, `flutter_secure_storage`, Hive.
**Team:** two backend developers, one UX designer, one UI designer, one team lead. Duration: three months.

## Problem

AVA is a digital oral and dental healthcare platform. The product goal was to make dental care easier to discover and manage in one application. Patients can find dentists and clinics, manage dental information, and communicate with professionals. Dentists, clinics, and dental-office teams get profiles, consultations, and communication in one client.

## Constraints

- **Architecture:** I extended and redesigned the Flutter client, which communicates with a REST backend through Dio and supports real-time communication through SignalR. The backend was built by two colleagues.
- **Language:** Persian was the primary language: `fa_IR`, RTL layouts, Persian fonts, Persian date handling, and direction-aware UI, implemented in `lib/core/app/app.dart` and the shared UI components.
- **Privacy:** the app handles personal and medical information. The authentication token is stored through `flutter_secure_storage` in `lib/core/storage/secure_storage.dart`, and authorization values are masked when failed requests are described in `lib/core/network/api_service.dart`.

## Decisions

The central HTTP layer, the app setup and the secure storage code were first written by a colleague (initial commit, 20 August 2024). The decisions below describe the codebase; my own change to the HTTP layer is the path normalization in the example.

1. **Feature-based architecture with Riverpod providers, repositories, models, and reusable widgets.** It separated API, state, and presentation concerns as the product grew. Trade-off: more structure and boilerplate for smaller screens.
2. **One central HTTP layer** in `lib/core/network/api_service.dart`: path normalization, multipart uploads, response parsing, error handling, and token-aware requests. Inconsistent request behavior was risky across many healthcare workflows. Trade-off: every endpoint implementation had to follow the shared API response contract.
3. **Persian and RTL configured at the application level** in `lib/core/app/app.dart`. Retrofitting directionality later would have caused widespread UI defects. Trade-off: some icons and custom components needed manual directional adjustments.

**A concrete example (precaution).** Some request paths have no leading slash, for example `account/family-member`. On 3 September 2026 I added the private method `ApiService._normalizePath`, which root-anchors relative paths and leaves absolute `http` and `https` URLs unchanged. The `get`, `post`, `put` and `delete` methods call it. I added it as prevention; I did not see a failed request before it.

## Result

- **What shipped:** AVA was released as a Flutter web app at `ava.smartwellness.ir`. I also built an Android APK and installed it on one device (Poco F5). The Android build was not distributed.
- **What was tested:** the web release in Chrome. The Android build was installed but not functionally tested. iOS was not tested.
- **What I can show:** screenshots with demo data and a 60-90 second video are in progress as of 30 October 2026. The code is in a private repository and is available on request. The repository was scanned with `gitleaks` (`git` and `dir` modes) on 30 October 2026: no findings.
- **Measured number:** 116 endpoint operations are documented in `API/api_catalog.md` (counted on 30 October 2026). This is a documentation count, not a claim that all 116 are used in the released app.
- **What I would do differently:** I would introduce automated integration tests and API contract checks earlier, so regressions are detected before manual end-to-end verification.
