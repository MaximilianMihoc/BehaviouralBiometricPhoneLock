# Behavioural Biometric Phone Lock

An Android research prototype that investigates whether the way a person naturally
uses a touchscreen can provide a continuous, unobtrusive layer of authentication.

I designed and developed this application as my final-year project at Dublin
Institute of Technology (DIT) during the 2015–2016 academic year. The project
received a strong final grade and remains one of the pieces of work from my degree
that I am most proud of.

## The idea

Passwords and PINs verify a user at a single moment. After a phone has been
unlocked, however, they do not establish whether the same person is still using
it. This project explored a complementary approach: learning a device owner's
behaviour from ordinary taps, scrolls, and flings, then continuously comparing new
interactions with that learned profile.

The goal was not to replace a password. It was to investigate whether behavioural
signals could help a phone recognise unusual use and respond by ending the session
or locking the device.

![Training and testing architecture](InterimReport/Figures/Figure%205.6.png)

## How it works

1. A user registers and completes a guided training activity.
2. The application records touchscreen events together with accelerometer and
   gyroscope readings.
3. Raw gestures are converted into normalised numerical features.
4. The owner's samples are labelled separately from guest samples and used to
   train a classifier on the device.
5. New taps and scroll/fling gestures are classified as owner-like or guest-like.
6. Predictions are accumulated into a configurable confidence decision. Depending
   on the selected setting, suspicious use can log the user out or invoke Android's
   device-administration lock mechanism.

The prototype includes two foreground experiences for collecting interactions in
more natural settings:

- A country-selection game used during guided training and testing
- A small Stack Overflow browser that produces realistic scrolling, tapping, and
  navigation behaviour

## Behavioural features

The system builds separate feature vectors for taps and scroll/fling gestures.
Features were normalised individually before being passed to the classifiers.

Every observation can include:

- Average linear acceleration during the interaction
- Average angular velocity from the gyroscope
- Gesture duration
- Normalised start and end coordinates
- Mid-stroke area, calculated with the shoelace formula

Scroll/fling observations also include:

- Direct start-to-end Euclidean distance
- The angle between the start and end vectors

The repository retains the feature-engineering experiments, including a mean
stroke-direction feature that was investigated and later excluded because it did
not improve identification.

## Machine-learning evaluation

The application uses OpenCV's Android machine-learning APIs and contains
implementations for three supervised classifiers:

- Support Vector Machine (SVM)
- k-Nearest Neighbours (k-NN)
- Random Trees

The evaluation screens compare classifier predictions independently for tap and
scroll/fling data. The project also includes anonymised training and test exports
for **25 participants**, classifier confusion matrices, and experiments using
different numbers of observations. These artefacts document the process of
selecting features, tuning the decision threshold, and balancing recognition
accuracy against the speed at which unusual behaviour is detected.

![Touch patterns from different users](InterimReport/Figures/Figure%203.2.PNG)

## Technical scope

This project brought together several areas of software engineering and applied
research:

- Native Android development in Java
- Touch and gesture processing with `MotionEvent` and `GestureDetector`
- Accelerometer and gyroscope data collection through Android sensor APIs
- Feature extraction and per-feature normalisation
- On-device classification with OpenCV
- Firebase Authentication and Realtime Database integration
- Android device-administration and lock-screen APIs
- REST integration with the Stack Exchange API
- Participant data collection, train/test separation, and classifier evaluation
- UML modelling, architecture design, and academic project documentation

## Repository structure

- `BehaviouralBiometricPhoneLock/` — the main Android application, OpenCV module,
  native libraries, activities, classifiers, and user-interface resources
- `FYP Data/` — anonymised per-user training and test exports plus evaluation
  spreadsheets and confusion matrices
- `LockScreenApp/` — an earlier standalone prototype of the Android lock mechanism
- `ModelOfActivityToGetTouchData/` — experiments for capturing touch input across
  activities
- `InterimReport/` — interim academic documentation, diagrams, presentation
  material, and supporting figures
- `BehaviouralBiometricsDiagram.suml` and `packageDiagram.suml` — UML design files

## Technology

- Java and the Android SDK
- OpenCV for Android
- Firebase Authentication and Realtime Database
- Android sensor, gesture, and device-administration APIs
- Gradle and Android Studio

## Historical project status

This repository is preserved as a record of the final-year project as it was built
and evaluated in 2015–2016. It targets Android API 23 and depends on legacy Android,
OpenCV, Gradle, and Firebase components. The original Firebase service is no longer
expected to be available, so the project will require dependency upgrades and new
backend configuration before it can run on a modern Android development setup.

The included datasets and evaluation artefacts are intended to document the
research process. Participant files use anonymous user identifiers rather than
names.

## License

The project is available under the [MIT License](LICENSE).
