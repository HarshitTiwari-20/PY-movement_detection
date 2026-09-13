# iCloudEMS Campus Intelligence — Classroom Video Analytics Pipeline

An intelligent, real-time Computer Vision pipeline designed for fixed classroom CCTV streams. Built using **YOLOv8 Pose** and a customized **BoT-SORT** multi-object tracker, this system tracks occupants, classifies posture, monitors room occupancy, detects entry/exit events, and performs scene analysis.

---

## 🌟 Key Features

1. **Robust Detection & Tracking**
   - Powered by `yolov8m-pose.pt` with customized `track_buffer=120` (4s buffer at 30 FPS) to maintain identities across classroom occlusions and static seated posture.

2. **Hybrid 4-Signal Posture Classifier**
   - Classifies occupant posture into **Seated** or **Standing** using a multi-factor scoring mechanism:
     - Bounding box aspect ratio & normalized height
     - Keypoint body-completeness (hip-to-ankle ratios)
     - Desk row alignment detected from background analysis

3. **Zone-Aware Entry / Exit Detection & Re-ID**
   - Automatically detects door position (`left` / `right`) based on initial track entry locations.
   - Differentiates between door-zone exits and interior disappearances (occlusions).
   - Integrates lightweight **HSV Color Histogram Re-Identification (Re-ID)** to prevent double-counting when occupants momentarily leave and re-enter.

4. **Scene Understanding & Desk Row Detection**
   - Computes a temporal median background model from early frames.
   - Uses Canny edge detection & Hough line transform to detect horizontal desk rows automatically.

5. **Motion Gating & Quality Assessment**
   - **MOG2 Background Subtractor** with temporal gating for reliable motion presence checks.
   - **Adaptive Laplacian Variance** to flag blurry frames for manual review.

6. **Interactive OpenCV UI & Seekbar**
   - Real-time HUD overlay with live analytics (attendance, occupancy, posture split, motion, quality).
   - Interactive seekbar at the bottom supporting mouse scrubbing.
   - Keyboard shortcuts: press `L` to cycle door zones, `Q` to quit.

---

## 📁 Repository Structure

```
├── main.py                  # Primary video analytics & tracking pipeline
├── generate_pdf.py          # Utility script generating formatted PDF submissions
├── classroom_botsort.yaml   # Auto-generated BoT-SORT tracker configuration
├── requirements.txt         # Project dependencies
├── report.md                # Detailed technical report & architecture breakdown
├── submission_answers.md    # Answers to system design & edge-case engineering questions
├── submission_answers.pdf   # Formatted PDF report
└── video7.mp4               # Sample input footage (if present)
```

---

## ⚙️ Installation & Setup

### 1. Prerequisites
Ensure Python 3.8+ is installed on your system.

### 2. Install Dependencies
Install all required packages via `pip`:

```bash
pip install -r requirements.txt
```

---

## 🚀 Running the Pipeline

To launch the computer vision analytics pipeline on the default video stream (`video7.mp4`):

```bash
python main.py
```

### Interactive Controls
- **Mouse Click / Drag** on the bottom seekbar: Jump to any frame in the video.
- **`L`**: Toggle / cycle door zone placement (`left` → `right` → `none`).
- **`Q`**: Quit application.

---

## 📝 Generating PDF Reports

To compile the submission documentation into a PDF:

```bash
python generate_pdf.py
```

---

## 📊 System Performance & Edge Case Considerations

For detailed answers regarding scaling to 500+ live streams, handling occlusions & Re-ID, and managing degraded camera feeds, refer to [submission_answers.md](file:///home/harshit/Desktop/projects/assignment1/submission_answers.md) or [report.md](file:///home/harshit/Desktop/projects/assignment1/report.md).