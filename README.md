# Gesture-Controlled Lynxmotion LSS Robotic Arm

Real-time hand-gesture control of a Lynxmotion LSS 5-DOF robotic arm using
MediaPipe hand landmarks and a scikit-learn gesture classifier. Two control
modes are provided:

- **`gesture_pipeline/`** — discrete behaviour mode. Each recognised gesture
  fires a pre-programmed routine (HOME, WAVE, BOW, REACH, DANCE, WIGGLE,
  POINT UP, EMERGENCY STOP).
- **`jog_mode/`** — continuous jog mode. Holding a gesture moves one joint
  continuously; releasing it stops the motion.

## Hardware

- Lynxmotion LSS robotic arm (5 servos: base, bottom, top, wrist, gripper)
- LSS adapter board on a serial / USB port (default `COM10`, edit
  `config.SERIAL_PORT`)
- Webcam (default index `0`)

## Setup

```bash
python -m venv .venv
.venv\Scripts\activate          # Windows
# source .venv/bin/activate     # macOS / Linux
pip install -r requirements.txt
```

The MediaPipe task file `hand_landmarker.task` and the trained classifier
`model.pkl` are included in each mode's folder.

## Running

Behaviour mode:

```bash
cd gesture_pipeline
python main.py
```

Jog mode:

```bash
cd jog_mode
python main_jog.py
```

Press **Q** to quit, **C** to clear an emergency stop.

## Gesture map (behaviour mode)

| Gesture        | Action           |
| -------------- | ---------------- |
| OPEN_PALM      | HOME             |
| FIST           | EMERGENCY STOP   |
| PEACE          | WAVE             |
| THUMBS_UP      | BOW              |
| POINT          | REACH            |
| THREE_FINGERS  | POINT UP         |
| ROCK_ON        | DANCE            |
| PINKY_UP       | WIGGLE           |

## Retraining the classifier

Capture new samples and retrain from `gesture_pipeline/`:

```bash
python capture_landmarks.py   # appends to gestures_dataset.csv
python train_model.py         # writes model.pkl
```

## Configuration

All tunable parameters (serial port, servo limits, speeds, poses,
gesture→behaviour map, camera settings) live in `config.py` inside each mode.
