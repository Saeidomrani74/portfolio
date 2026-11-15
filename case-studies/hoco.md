# HOCO: Case Study

## Context
The first version of the HOCO app was written by an earlier developer. I continued developing the app, fixed bugs, and added `hocommax`.

## Problem
HOCO controls a security system by SMS. Within the app, `hocommax` is an optional voice assistant that guides device setup with voice prompts and takes voice commands in the setup and contacts screens.

## Constraints
The app targets Android only and was tested on a Poco F5; I did not test iOS. One limitation of `hocommax` is that `speech_to_text` relies on the device's speech service, which I checked on my Poco F5.

## Decisions
The app already used `Provider` for state management. For `hocommax`, `audioplayers` plays the voice prompts and `speech_to_text` takes voice commands in the setup and contacts screens. `hocommax` is an optional assistant that the user can disable because the setup and contacts screens still work without voice. If speech is not understood, the assistant listens once more; after a second failure, it plays a “no answer” prompt and disables the assistant for the current screen only.

## Result
The Git repository contains 142 commits between 18 January 2023 and 18 March 2025 (git, counted 11 Nov 2026), with the first `hocommax` commit dated 11 June 2023. I manually tested four flows on a Poco F5: the voice prompt, voice command, retry flow, and disabled state. I also tested the other features of the app on a Poco F5 and fixed bugs in them. `hocommax` has no automated tests.