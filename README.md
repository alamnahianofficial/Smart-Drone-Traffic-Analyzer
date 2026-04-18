# Drone Traffic Analyzer

### Project Overview

This is a full-stack application built to analyze traffic from drone footage. The system detects vehicles, tracks them with unique IDs, and counts them as they cross a specific line. It was built using FastAPI for the backend and Next.js for the frontend. I used an NVIDIA RTX 2060 Super to handle the AI processing so it runs much faster than a standard CPU.

---

### Technical Logic

#### Adaptive Pipeline

The counting is not random or hardcoded to one video size. I built an adaptive system where the math changes based on the video resolution and length to keep the detection baseline stable:

- **Detection Gate:** The trigger line is always at 65% of the video height. This works for 720p, 1080p, or 4K because it uses a percentage instead of fixed pixels.
- **Dynamic Buffer:** The "catchment area" around the line scales with the video height (3% to 5%). This makes sure fast vehicles moving between frames don't skip over the line.
- **Processing Modes:** For long videos, the system shifts gears to process frames more efficiently (frame stepping) so the GPU doesn't struggle.

#### Vehicle Tracking

I used YOLOv8 with persistent tracking. The code is set to only look for specific classes:

- Cars, Motorcycles, Buses, and Trucks.

This prevents the system from counting things like people or trees, which keeps the Excel data clean.

---

### Key Features

- **Live Progress:** The frontend shows a real-time progress bar while the backend is processing heavy video files.
- **Excel Reports:** Once the video is done, you can download a report with the Vehicle ID, Type, Timestamp, and Frame number.
- **GPU Support:** The backend automatically checks for CUDA to use the GPU for faster results.

---

### Installation and Environment Setup

#### 1. Backend (Python Virtual Environment)

It is required to use a virtual environment to keep dependencies isolated and ensure the script runs correctly.

1. Navigate to the backend folder: `cd backend`
2. Create the venv: `python -m venv venv`
3. Activate the venv:
   - Windows: `.\venv\Scripts\activate`
   - Mac/Linux: `source venv/bin/activate`
4. Install requirements: `pip install -r requirements.txt`
5. Place the model file `yolov8n.pt` in `backend/models/`.

#### 2. Frontend Setup

1. Navigate to the frontend folder: `cd frontend`
2. Install dependencies: `npm install`

---

### How to Run

- **Start Backend Server:** `uvicorn app.main:app --reload --port 8000`
- **Start Frontend Dashboard:** `npm run dev`
- **Access the App:** Open `http://localhost:3000` in your browser.

---

### Engineering Assumptions

- The video is from a drone perspective (top-down view).
- Vehicles are counted when they hit the 65% height mark of the frame.
- The system expects standard video formats like .mp4 or .avi.
- If no GPU is found, the script will automatically switch to CPU (slower processing).

---

### Repository Details

- **Author:** alamnahianofficial
- **Repository Name:** drone-traffic-analyzer
