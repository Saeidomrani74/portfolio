[ava.md](https://github.com/user-attachments/files/34212027/ava.md)
# AVA: Case Study

**Role:** the only Flutter developer. I built the Flutter client and all of its features myself: discovery, forum, dashboard, chat, and more.
**Stack:** Flutter, Dio, SignalR, Riverpod, `flutter_secure_storage`, Hive.
**Team:** two backend developers, one UX designer, one UI designer, one team lead. Duration: three months.

## Problem

AVA is a digital oral and dental healthcare platform. The product goal was to make dental care easier to discover and manage in one application. Patients can find dentists and clinics, manage dental information, and communicate with professionals. Dentists, clinics, and dental-office teams get profiles, consultations, and communication in one client.

## Constraints

- **Architecture:** I built the Flutter client, which communicates with a REST backend through Dio and supports real-time communication through SignalR. The backend was built by two colleagues.
- **Language:** Persian was the primary language: `fa_IR`, RTL layouts, Persian fonts, Persian date handling, and direction-aware UI, implemented in `lib/core/app/app.dart` and the shared UI components.
- **Privacy:** the app handles personal and medical information. The authentication token is stored through `flutter_secure_storage` in `lib/core/storage/secure_storage.dart`, and authorization values are masked when failed requests are described in `lib/core/network/api_service.dart`.

## Decisions

1. **Feature-based architecture with Riverpod providers, repositories, models, and reusable widgets.** It separated API, state, and presentation concerns as the product grew. Trade-off: more structure and boilerplate for smaller screens.
2. **One central HTTP layer** in `lib/core/network/api_service.dart`: path normalization, multipart uploads, response parsing, error handling, and token-aware requests. Inconsistent request behavior was risky across many healthcare workflows. Trade-off: every endpoint implementation had to follow the shared API response contract.
3. **Persian and RTL configured at the application level** in `lib/core/app/app.dart`. Retrofitting directionality later would have caused widespread UI defects. Trade-off: some icons and custom components needed manual directional adjustments.

**A concrete example.** Before path normalization, some callers passed API paths without a leading slash and others passed paths with one. With a base URL ending in `/v1`, passing `account/family-member` could produce `/v1account/family-member` instead of `/v1/account/family-member`. That caused a 404 response, while `/auth/login` worked correctly. I fixed this in the private method `ApiService._normalizePath`, which adds a leading slash to relative paths and leaves absolute URLs unchanged.

## Result

- **What shipped:** AVA was released as a Flutter web app at `ava.smartwellness.ir`. I also built an Android APK and installed it on one device (Poco F5). The Android build was not distributed.
- **What was tested:** the web release in Chrome. The Android build was installed but not functionally tested. iOS was not tested.
- **What I can show:** screenshots with demo data and a 60-90 second video are in progress as of 30 October 2026. The code is in a private repository and is available on request. The repository was scanned with `gitleaks` (`git` and `dir` modes) on 30 October 2026: no findings.
- **Measured number:** 116 endpoint operations are documented in `API/api_catalog.md` (counted on 30 October 2026). This is a documentation count, not a claim that all 116 are used in the released app.
- **What I would do differently:** I would introduce automated integration tests and API contract checks earlier, so regressions are detected before manual end-to-end verification.
