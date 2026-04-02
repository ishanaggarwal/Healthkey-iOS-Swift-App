# HealthKey — iOS Healthcare Data Sharing Platform

> A role-based iOS application that empowers patients, hospitals, and insurance companies to securely share and manage healthcare data.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Features](#features)
3. [Screenshots / Demo](#screenshots--demo)
4. [Tech Stack](#tech-stack)
5. [Architecture / Folder Structure](#architecture--folder-structure)
6. [Requirements](#requirements)
7. [Installation / Setup](#installation--setup)
8. [Running the App](#running-the-app)
9. [Testing](#testing)
10. [Troubleshooting](#troubleshooting)
11. [Contributing](#contributing)
12. [License](#license)

---

## Project Overview

**HealthKey** is an iOS application built with Swift and UIKit that creates a unified healthcare data-sharing ecosystem. It connects three key stakeholders — **Patients**, **Hospitals**, and **Insurance Companies** — under one platform with role-based access control.

Patients maintain full ownership of their medical records and control who can view them. Hospitals can add clinical records for patients once access is granted. Insurance companies can view approved medical data to process claims and approvals. All data is persisted in real-time via **Firebase Firestore**, and authentication is handled by **Firebase Auth**.

This project was developed as part of the **CS 5520** coursework at Northeastern University.

---

## Features

### 👤 Patient
- **Medical Record Management** — Add, view, and manage personal medical records (blood glucose, cholesterol, blood pressure, and more).
- **Hospital Access Control** — Grant or revoke hospital read access to your records.
- **Insurance Integration** — Submit approval requests to insurance providers and track their status.
- **Nearby Hospitals Map** — View nearby hospitals on an interactive map using address geocoding and MapKit.
- **Profile Management** — Update personal details including date of birth, gender, blood group, and profile picture.

### 🏥 Hospital
- **Patient Management** — Search and manage a list of linked patients.
- **Clinical Record Entry** — Add new medical records on behalf of patients.
- **Insurer Access Control** — Grant insurance companies permission to view specific patient records.
- **Patient Profile Viewing** — Access full patient profiles and medical history.

### 🏢 Insurance Company
- **Patient Portfolio** — View all patients linked to the insurance company.
- **Medical Record Access** — View patient records when the patient has granted access.
- **Approval Granting** — Approve patient insurance requests and update approval status.
- **Record History** — Track and manage all insurance-related approvals.

### 🔐 Authentication & Session
- Email/password sign-up and login powered by Firebase Authentication.
- Password reset via email.
- Persistent session using `UserDefaults` — users remain logged in across app launches.
- Role-based navigation (Patient / Hospital / Insurance) on login.

---

## Screenshots / Demo

> 📹 A demo walkthrough video (`ProjectVideo.mp4`) is included in the repository root.

| Login | Patient Home | Hospital Home | Insurance Home |
|-------|-------------|---------------|----------------|
| _screenshot placeholder_ | _screenshot placeholder_ | _screenshot placeholder_ | _screenshot placeholder_ |

> **TODO**: Replace placeholder cells above with actual screenshots once the app is run on a simulator or device. Screenshots can be added to an `assets/screenshots/` folder and referenced here.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Language** | Swift 5 |
| **UI Framework** | UIKit (programmatic — no Storyboard UI logic) |
| **Authentication** | Firebase Authentication |
| **Database** | Firebase Firestore (NoSQL real-time database) |
| **Maps** | MapKit + CoreLocation (geocoding) |
| **Photo Picker** | PhotosUI (`PHPickerViewController`) |
| **Dependency Manager** | Swift Package Manager (via Xcode) |
| **Minimum iOS** | iOS 18.0 |
| **Xcode** | Xcode 15+ |

---

## Architecture / Folder Structure

The project follows a **MVC pattern with dedicated View classes** — each screen is split into a Controller (business logic) and a View (programmatic UIKit layout). This keeps view construction separate from state management.

```
Healthkey-iOS-Swift-App/
├── cs5520-project/                  # Main app target
│   ├── AppDelegate.swift            # App entry point, Firebase initialization
│   ├── SceneDelegate.swift          # Scene & window lifecycle
│   ├── Info.plist                   # App configuration
│   ├── GoogleService-Info.plist     # Firebase project config (git-ignored)
│   │
│   ├── Enums/
│   │   └── Enums.swift              # Shared enums and data model structs
│   │
│   ├── FirebaseHelper/
│   │   ├── AuthHelper.swift         # Firebase Auth wrapper (login, signup, reset)
│   │   └── FirestoreGenericHelpers.swift  # Firestore CRUD helpers
│   │
│   ├── MainScreen/                  # Login screen
│   │   ├── ViewController.swift
│   │   └── LoginView.swift
│   │
│   ├── SignupScreen/                # Registration screen
│   │   ├── SignupController.swift
│   │   └── SignupView.swift
│   │
│   ├── ResetScreen/                 # Password reset screen
│   │   ├── ResetPasswordViewController.swift
│   │   └── ResetPasswordView.swift
│   │
│   ├── PatientScreens/              # Screens accessible to Patient role
│   │   ├── HomeScreen/              # Map view with nearby hospitals
│   │   ├── ProfileScreen/           # Patient profile editing
│   │   ├── AddMedicalRecordScreen/  # Add new medical records
│   │   ├── ViewMedicalRecords/      # Browse all medical records
│   │   ├── ProvideHospitalAccess/   # Grant hospital access
│   │   ├── ReviewHospitalAccess/    # Review / revoke hospital access
│   │   └── ViewInsurerApproval/     # Track insurance approval requests
│   │
│   ├── HospitalScreen/              # Screens accessible to Hospital role
│   │   ├── HospitalHomeScreen/      # Patient list
│   │   ├── HPAddRecordScreen/       # Add records for a patient
│   │   ├── HPViewMedicalRecords/    # View patient medical records
│   │   ├── HViewInsurerApproval/    # Manage insurer approvals
│   │   ├── ViewPatientDetails/      # Patient detail view
│   │   └── HospitalProfileScreen/  # Hospital profile
│   │
│   └── InsuranceScreens/            # Screens accessible to Insurance role
│       ├── InsuranceHomeScreen/     # Linked patients list
│       ├── IPViewMedicalRecords/    # View patient records
│       ├── InsurerGrantApproval/    # Grant patient approvals
│       └── InsuranceProfileScreen/ # Insurance company profile
│
├── cs5520-projectTests/             # Unit tests
├── cs5520-projectUITests/           # UI tests
└── ProjectVideo.mp4                 # App demo video
```

### Firestore Data Model

| Collection | Description |
|-----------|-------------|
| `users` | User profiles (name, email, role, DOB, gender, blood group) |
| `medicalRecords` | Patient medical records (type, value, date, notes) |
| `hospitalRecords` | Hospital info and linked patient UIDs |
| `insurersRecords` | Insurance company info and linked patient UIDs |
| `approveRequest` | Access request/approval documents |

---

## Requirements

| Requirement | Version |
|-------------|---------|
| **Xcode** | 15.0 or later |
| **iOS Deployment Target** | iOS 18.0 |
| **Swift** | 5.0+ |
| **macOS** (for development) | macOS Ventura 13.0+ |
| **Firebase Project** | Required (see [Installation / Setup](#installation--setup)) |

> **Note:** The app has not been tested on iOS versions below 18.0. It may function on earlier versions, but this has not been validated.

---

## Installation / Setup

### 1. Clone the Repository

```bash
git clone https://github.com/ishanaggarwal/Healthkey-iOS-Swift-App.git
cd Healthkey-iOS-Swift-App
```

### 2. Firebase Setup

HealthKey requires a Firebase project to handle authentication and data storage.

1. Go to the [Firebase Console](https://console.firebase.google.com/) and create a new project.
2. Register an iOS app with bundle ID `com.nuios.cs5520-project` (or update it to your own bundle ID).
3. Download the generated `GoogleService-Info.plist` file.
4. Place `GoogleService-Info.plist` in the `cs5520-project/` folder alongside `Info.plist`.
   > ⚠️ `GoogleService-Info.plist` is excluded from version control. You **must** add your own copy.

#### Enable Firebase Services

In the Firebase Console, enable:

- **Authentication** → Sign-in method → Email/Password
- **Firestore Database** → Start in production or test mode

#### Firestore Security Rules (recommended for development)

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

> 🔒 Tighten these rules before deploying to production.

### 3. Firebase SDK via Swift Package Manager

The Firebase SDK is integrated via Swift Package Manager. Xcode should resolve packages automatically when you open the project. If it doesn't:

1. Open `cs5520-project.xcodeproj` in Xcode.
2. Go to **File → Packages → Resolve Package Versions**.

### 4. Permissions & Entitlements

The app uses the following device capabilities. Ensure these are configured in your Xcode project:

| Permission | Usage | Where to configure |
|-----------|-------|--------------------|
| **Location (When In Use)** | Geocode hospital addresses for map display | `Info.plist` → `NSLocationWhenInUseUsageDescription` |
| **Photo Library** | Select a profile picture from the photo library | `Info.plist` → `NSPhotoLibraryUsageDescription` |

> **HealthKit**: The app name references "HealthKey" but **HealthKit is not currently integrated** in the codebase. No HealthKit entitlements or API calls were found. Future versions may add HealthKit support.

> **TODO**: Verify whether `NSLocationWhenInUseUsageDescription` and `NSPhotoLibraryUsageDescription` keys are present in `Info.plist` and add them if missing, to avoid runtime crashes on iOS 14+.

---

## Running the App

### Simulator

1. Open `cs5520-project.xcodeproj` in Xcode.
2. Select any **iPhone simulator** running **iOS 18.0+** from the scheme selector.
3. Press **⌘ R** (or click the ▶ Run button).

> 📍 MapKit and location geocoding work in the simulator but may default to Apple HQ (Cupertino, CA) unless a custom location is set via **Features → Location** in the simulator menu.

### Physical Device

1. Connect your iPhone (iOS 18.0+) via USB.
2. In Xcode, go to **Signing & Capabilities** for the `cs5520-project` target.
3. Set your **Team** (Apple Developer account) and ensure the bundle identifier is unique.
4. Select your device as the run destination and press **⌘ R**.

> ⚠️ A paid Apple Developer account may be required for full functionality on a physical device. Free accounts support limited capabilities and require device re-signing every 7 days.

---

## Testing

The project includes both unit and UI test targets.

| Test Target | Location | Runner |
|-------------|----------|--------|
| Unit Tests | `cs5520-projectTests/` | Xcode Test Navigator |
| UI Tests | `cs5520-projectUITests/` | Xcode Test Navigator |

### Run Tests in Xcode

1. Open `cs5520-project.xcodeproj`.
2. Press **⌘ U** to run all tests, or use the **Test Navigator** (⌘ 6) to run individual tests.

> **TODO**: The test targets currently contain the default Xcode template tests. Contributions that add meaningful unit and UI test coverage are welcome (see [Contributing](#contributing)).

---

## Troubleshooting

| Problem | Solution |
|---------|---------|
| `GoogleService-Info.plist` not found | Add your Firebase config file to `cs5520-project/` — see [Firebase Setup](#2-firebase-setup) |
| App crashes on launch | Ensure Firebase is initialized in `AppDelegate.swift` (`FirebaseApp.configure()`) and `GoogleService-Info.plist` is present |
| Firebase Auth errors (code 17009) | Verify that Email/Password sign-in is enabled in the Firebase Console |
| Map shows wrong location | In the Simulator, go to **Features → Location → Custom Location** and set a location |
| Photos picker not appearing | Add `NSPhotoLibraryUsageDescription` to `Info.plist` |
| Package resolution fails | Go to **File → Packages → Resolve Package Versions** or **Reset Package Caches** |
| Signing error on device | Set a valid Apple Developer Team in **Signing & Capabilities** |
| Build fails with Swift version error | Ensure you are using Xcode 15 or later |

---

## Contributing

Contributions are welcome! Please follow these steps:

1. **Fork** the repository.
2. **Create a feature branch**: `git checkout -b feature/your-feature-name`
3. **Commit your changes** with clear, descriptive messages.
4. **Push** to your fork: `git push origin feature/your-feature-name`
5. **Open a Pull Request** targeting the `main` branch with a description of your changes.

### Code Style Guidelines

- Follow [Swift API Design Guidelines](https://www.swift.org/documentation/api-design-guidelines/).
- Separate UI layout code into dedicated `*View.swift` files following the existing pattern.
- Use the existing `FirestoreGenericHelpers` and `AuthHelper` wrappers for Firebase operations.
- Avoid force-unwrapping (`!`) unless absolutely necessary — prefer `guard let` or `if let`.

### Areas for Improvement

- [ ] Add comprehensive unit tests for Firebase helper functions
- [ ] Add UI tests for critical user flows (login, add record, grant access)
- [ ] Integrate HealthKit for reading/writing native health data
- [ ] Add offline support via Firestore caching
- [ ] Localization support for multiple languages
- [ ] Accessibility improvements (VoiceOver labels, Dynamic Type)

---

## License

> **No license file is currently present in this repository.**
>
> Until a license is added, this codebase defaults to **All Rights Reserved** under copyright law. If you are a contributor or the maintainer, consider adding an open-source license (e.g., MIT, Apache 2.0) to make the intended usage clear.
>
> **TODO**: Add a `LICENSE` file to the repository root.

---

<div align="center">
  <sub>Built with ❤️ for CS 5520 at Northeastern University</sub>
</div>
