# AI Pose Tracker & Workout Assistant
### Real-Time Computer Vision, Biomechanical Motion Analysis & Safety Monitoring

An AI-powered desktop application that uses real-time pose estimation and geometric motion analysis to track exercises, analyze movement patterns, count repetitions, provide feedback, and monitor potential safety events.

## Overview

**AI Pose Tracker & Workout Assistant** is a Python desktop application that combines computer vision, pose estimation, and biomechanical analysis to support workout tracking and movement assessment.

The application processes live camera frames, detects human body landmarks, calculates joint angles, and analyzes exercise movements using custom state-based logic. It provides visual and audio feedback while monitoring potentially concerning movement patterns through an in-app safety alert workflow.

The project demonstrates the integration of real-time computer vision, GUI development, multithreaded processing, local database management, and modular software architecture.

## Key Features

- **Real-Time Pose Estimation** — Detects human body landmarks from a live camera feed using MediaPipe Pose Landmarker.
- **Biomechanical Motion Analysis** — Calculates joint angles and analyzes body positions using geometric and vector-based calculations.
- **Automated Repetition Counting** — Uses Finite State Machines (FSMs) to identify exercise phases and track repetitions.
- **Exercise-Specific Logic** — Supports movement tracking for exercises such as bicep curls, squats, push-ups, and knee extensions.
- **Movement Feedback** — Provides visual and voice-based feedback during exercise sessions.
- **Fall & Anomaly Detection** — Analyzes potentially concerning movement patterns, including sudden downward movement and prolonged inactivity.
- **In-App Safety Alerts** — Triggers a visual alert and voice feedback, with an emergency countdown interface.
- **Location Management** — Supports storing location information for display within the safety workflow.
- **Emergency Contact Management** — Stores emergency contact details associated with user profiles.
- **Workout Session Management** — Supports exercise sessions and local persistence of relevant user data.
- **Responsive Desktop Interface** — Runs video processing in a background thread to help prevent the GUI from freezing.

## Computer Vision Pipeline

The application follows a multi-stage processing pipeline:

```text
Live Camera Feed
       |
       v
OpenCV Frame Acquisition
       |
       v
MediaPipe Pose Landmarker
       |
       v
Body Landmark Extraction
       |
       v
Geometric & Joint-Angle Analysis
       |
       v
Exercise State Recognition (FSM)
       |
       v
Repetition Tracking & Movement Feedback
       |
       v
Safety Monitoring & In-App Alerts
```

### 1. Frame Acquisition

OpenCV captures video frames from the camera and provides the input for real-time pose processing.

### 2. Pose Estimation

MediaPipe Pose Landmarker detects body landmarks that represent key points of the human body.

### 3. Landmark Processing

Relevant landmark coordinates are extracted for subsequent geometric calculations and movement analysis.

### 4. Biomechanical Analysis

Custom angle-calculation functions use vector operations and NumPy to calculate angles between relevant body joints, including the knees, hips, elbows, and shoulders.

### 5. Exercise State Recognition

Exercise-specific state machines analyze movement transitions to recognize exercise phases and determine when a repetition is completed.

### 6. Feedback & Safety Monitoring

The application provides movement feedback and evaluates potentially concerning motion patterns through its safety-monitoring workflow.

## Technical Stack

| Category | Technologies |
|---|---|
| Programming Language | Python 3.10+ |
| Computer Vision | OpenCV |
| Pose Estimation | MediaPipe Pose Landmarker |
| Numerical Computing | NumPy |
| Desktop GUI | PyQt6 |
| Background Processing | QThread, PyQt Signals |
| Local Database | SQLite |
| HTTP Requests | Requests |
| Voice Feedback | pyttsx3 |
| Motion Analysis | Vector Geometry, Joint-Angle Calculations |
| Exercise Tracking | Finite State Machines (FSMs) |

## Architecture & Engineering

### Real-Time Processing & Threading

Video acquisition and pose-estimation tasks can be computationally intensive. To maintain a responsive desktop interface, the application uses a `QThread`-based worker to execute background processing.

PyQt Signals enable communication between the worker and the GUI, allowing processing results and events to be delivered to the interface without placing the main GUI thread in charge of the entire video-processing loop.

### Biomechanical Motion Analysis

The application uses geometric calculations to analyze relationships between body landmarks.

The angle-calculation module derives joint angles from landmark coordinates, providing measurements that can be used by exercise-specific logic to evaluate movement phases.

### Finite State Machines for Exercise Tracking

Each supported exercise uses state-based logic to recognize movement transitions and count completed repetitions.

The exercise modules include:

- `bicep_curl.py`
- `squat.py`
- `pushup.py`
- `knee_extension.py`

The state-based approach helps distinguish exercise phases and reduces reliance on a single fixed position or frame to determine whether a repetition has occurred.

### Safety Monitoring & Alert Workflow

The safety subsystem is designed to identify potentially concerning movement patterns.

Its workflow includes:

- **Movement Monitoring:** Evaluates changes in body position and movement patterns.
- **Potential Fall Detection:** Considers sudden downward movement and prolonged inactivity.
- **Alert Management:** Coordinates the in-app safety alert workflow.
- **Voice Feedback:** Provides audio feedback during relevant events.
- **Location Information:** Supports collecting and displaying stored location information.
- **Emergency Contacts:** Maintains emergency contact information associated with a user.

When a potential safety event is detected, the application can activate a visual alert and countdown interface.

**Important:** The current workflow provides in-app safety alerts. It does not automatically transmit the user's location or send live emergency notifications to emergency services. This functionality would require a separate, implemented and tested external notification integration.

### Local Database & Persistence

The application uses SQLite to store local application data.

The database includes records for:

- **Authentication (`auth`)** — User authentication-related data.
- **Profiles (`profiles`)** — User profile information and stored location fields.
- **Emergency Contacts (`emergency_contacts`)** — Emergency contact details associated with a user.

This local database architecture supports persistent application data without requiring a remote database server.

## Project Structure

The following is a high-level overview of the main modules described in the project documentation. The exact directory layout may vary.

```text
AI-Pose-Tracker/
├── main.py
├── fitness_app.db
├── pose_landmarker_full.task
└── src/
    ├── gui/
    │   └── styles/
    │       └── style.qss
    ├── angle_calculator.py
    ├── workout_worker.py
    ├── fall_detector.py
    ├── emergency_manager.py
    ├── nexus_alert.py
    ├── location_manager.py
    ├── voice_manager.py
    └── exercises/
        ├── bicep_curl.py
        ├── squat.py
        ├── pushup.py
        └── knee_extension.py
```


## Getting Started

### Prerequisites

- Python 3.10 or a compatible version
- A webcam or supported video input device
- Git
- The required MediaPipe pose model asset

### 1. Clone the Repository

Replace the placeholder URL with the actual repository URL.

```bash
git clone <YOUR_REPOSITORY_URL>
cd <YOUR_REPOSITORY_DIRECTORY>
```

### 2. Create a Virtual Environment

**Windows:**

```bash
python -m venv .venv
.venv\Scripts\activate
```

**macOS / Linux:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install PyQt6 opencv-python mediapipe numpy requests pyttsx3
```

### 4. Configure the Pose Model

Ensure that the MediaPipe model file:

`pose_landmarker_full.task`

is available at the path expected by the application.

### 5. Run the Application

```bash
python main.py
```

## Engineering Challenges

### Maintaining GUI Responsiveness

Real-time video processing can block user-interface updates if executed in the main thread. Using a background worker and PyQt Signals separates processing from interface updates and helps maintain a responsive experience.

### Stabilizing Pose-Based Measurements

Pose-estimation output can vary between frames. Confidence filtering, movement thresholds, and temporal considerations can help reduce unstable measurements and unwanted exercise-state transitions.

### Designing Reliable Repetition Tracking

Exercise movements contain multiple phases. State-based logic allows each exercise to recognize transitions between phases instead of relying on a single position to count repetitions.

### Reducing False Safety Alerts

Low body positions and pauses can occur during normal workouts. Evaluating movement changes and prolonged inactivity together provides a more considered basis for triggering a potential safety alert than relying on a single frame.

## Future Improvements

Potential directions for further development include:

- Improving pose-landmark stabilization and movement classification.
- Expanding support for additional exercises.
- Enhancing repetition tracking across different movement speeds and ranges.
- Adding workout analytics and progress visualization.
- Supporting optional cloud synchronization and remote data storage.
- Evaluating additional pose-estimation or exercise-classification models.
- Implementing and testing an external emergency-notification integration with explicit user consent and appropriate safeguards.

## What I Learned

This project provided hands-on experience with:

- Integrating real-time pose estimation into a Python application.
- Processing camera frames using OpenCV.
- Applying vector geometry and joint-angle calculations to movement analysis.
- Designing Finite State Machines for exercise tracking.
- Building a desktop application with PyQt6.
- Using background threads and event-driven communication.
- Persisting application data with SQLite.
- Designing a multi-stage safety-monitoring and alert workflow.
- Structuring software into separate processing, interface, and data-management modules.

## Author

**Noa Galer**

Software Developer | Computer Vision & AI

- GitHub: [NoaGaler](https://github.com/NoaGaler)
- LinkedIn: [https://www.linkedin.com/in/noa-galer]

## Disclaimer

This project is intended for educational and demonstration purposes. Pose estimation and movement analysis may be inaccurate, and safety alerts are not a substitute for professional supervision or a validated emergency-response system.

The application does not currently provide automatic emergency-service notification.
