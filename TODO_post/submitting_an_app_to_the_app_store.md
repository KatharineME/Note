+++ 
date = "2021-10-05"
title = "Creating a MacOS App"
slug = "creating_a_macos_app"
tags = []
categories = []
+++

## Submitting an App to the App Store

![app store](/images/app_store.png)

Before getting into the step by step process, you need to know the following about submitting an app to the app store:

- The first step is applying to the Apple Developer Program and it takes months to be approved
- Apps needs to be submitted through Xcode
- Two potentially helpful video tutorials if questions come up:
  - https://www.youtube.com/watch?v=fXeDe9tafG8
  - https://www.youtube.com/watch?v=fXeDe9tafG8&t=84s

#### 1. Enroll in Apple Developer Program

- https://developer.apple.com/programs/

#### 2. Setup Apple Developer Account

- Sign all contracts in App Store Connect under "Agreements, Tax and Banking"

#### 3. Create an Xcode Project

#### 4. Register Bundle ID on App Store Connect

- Go to Xcode, in your app project go to Signing & Capabilities, click on Capabilities, search for In-App Purchase, and it should automatically create a provising profile/bundle id. Go back to App Store Connect and youn should see your app under the Identifiers section.

#### 5. Create a New App in App Store Connect

- Go to My Apps > add an app > check macOS > etc.

#### 6. Add App Icon Set to App Project in Xcode

- https://appicon.co

#### 7. Add Version and Build Number in Xcode

#### 8. Add Your Apple ID Under Xcode/Preferences/Accounts

#### 9. Test App in Xcode simulator

#### 10. Archive the Project

- Select the correct device
- Go to Product > Archive

#### 11. Validate App

- In Xcode go to Archives and click Validate App
- Keep default values
- Select automatically manage signing
- It may take some time
- It will say validation was successful in the end

#### 12. Distribute App

This basically uploads the app to App Store Connect.

- In Xcode got to Archives and click Distribute App
- Select App Store COnnect
- Select Upload
- Keep defaults
- Select Automatically manage signing
- Then an Upload progress bar should appear
- In the end it will say sucessfuly uploaded

#### 13. Check TestFlight in App Store Connect

- Go to App Store Connect and in the dropdown select TestFlight
- It will say the app is processing

#### 14. Select the Build To Be Reviewed

#### 15. Submit for Review
