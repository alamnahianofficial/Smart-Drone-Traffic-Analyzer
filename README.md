# Smart Drone Traffic Analyzer

### Computer Vision Automation Full-Stack Development

## 1. Project Overview

This is a professional full-stack application built to analyze traffic from aerial drone footage. The system identifies vehicles, tracks them with unique IDs, and implements custom logic to ensure accurate counting across variable resolutions and edge cases.

## 2. Technical Architecture

[cite_start]In compliance with the assessment requirements, this project uses a **decoupled Full-Stack architecture**:

- **Frontend:** Built with **Next.js** (React) to handle the UI. [cite_start]It includes explicit **loading states and progress indicators** [cite: 23] to maintain a responsive UX during the heavy background processing of video files.
- [cite_start]**Backend:** Powered by **FastAPI** (Python) to manage the computer vision pipeline and data extraction[cite: 24].
- [cite_start]**Communication:** File uploads and detection data are handled via **REST APIs**[cite: 25].

## 3. Unique Adaptive Pipeline & Logic

[cite_start]To solve the **Core Challenge** of preventing double-counting and handling occlusions, I developed an **Adaptive Detection Pipeline**:

- **Resolution-Agnostic Gate:** Instead of fixed pixel coordinates, the "Detection Gate" is set at a relative **65% of the frame height**. This ensures the logic scales perfectly whether the input is 720p, 1080p, or 4K.
- **Dynamic Catchment Buffer:** The system calculates a "trigger zone" (3%–5% of frame height) that scales with the video resolution. This prevents high-speed vehicles from "skipping" the detection line between frames.
- **ID State Persistence:** Once a vehicle crosses the gate, its unique ID is stored in a session-specific "Counted" set. [cite_start]The system cross-references this set to prevent re-counting vehicles that stop, slow down, or are temporarily hidden by obstacles like lampposts.

## 4. Automation and Reporting

[cite_start]The application automatically generates a structured **CSV/Excel report** [cite: 15, 37] containing:

- [cite_start]**Total unique vehicle count**[cite: 38].
- [cite_start]**Breakdown by vehicle type** (Car, Bus, Truck, etc.)[cite: 39].
- [cite_start]**Processing Duration:** The total time taken to analyze the file[cite: 40].
- [cite_start]**Frame and timestamp data** for every detection event[cite: 41].

## [cite_start]5. Engineering Assumptions [cite: 57, 67]

- [cite_start]The footage is from a top-down or high-angle drone perspective[cite: 5].
- Vehicles are counted upon crossing the 65% height threshold.
- The system utilizes GPU acceleration (CUDA) if available, falling back to CPU if necessary.

## [cite_start]6. Local Setup & Installation [cite: 64]

### Backend

1. `cd backend`
2. `python -m venv venv`
3. `source venv/bin/activate` (Windows: `.\venv\Scripts\activate`)
4. `pip install -r requirements.txt`
5. `uvicorn app.main:app --reload`

### Frontend

1. `cd frontend`
2. `npm install`
3. `npm run dev`
