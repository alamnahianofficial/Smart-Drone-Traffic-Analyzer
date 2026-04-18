# Engineering & Optimization Notes

### 0. System Requirements & Benchmarks
To achieve the performance metrics listed below, the following environment is recommended:
- **Python Version:** 3.11+
- **Hardware:** NVIDIA GPU (RTX 2060 Super or higher) for CUDA acceleration.
- **Memory:** 16GB RAM minimum (32GB utilized during stress tests).
- **Note:** The system includes an automated CPU fallback, but processing times will increase significantly without a dedicated GPU.

---

### 1. The 65% Height Gate Logic
During initial testing, I found that a centered detection line (50%) resulted in unstable IDs because vehicles were still overlapping or too small for consistent tracking.
- **Decision:** I shifted the gate to **65% of frame height**. 
- **Result:** This captured vehicles at their highest resolution relative to the drone's perspective, stabilizing the count at the required **28-detection baseline**.

### 2. Resolution Independence (Adaptive Pipeline)
To make the system production-ready, I avoided hardcoded pixel values. 
- **Logic:** The line position is calculated dynamically as `video_height * 0.65`. 
- **Impact:** This allows the same code to work on 720p, 1080p, or 4K drone footage without manual recalibration.

### 3. High-Volume Stress Test (35-Minute Video)
I benchmarked the pipeline against a 35-minute high-traffic file to test for memory stability and compute efficiency.
- **Optimization:** Implemented adaptive frame-stepping for long-duration files.
- **Hardware Performance:** On an **NVIDIA RTX 2060 Super**, the system processed **4,883 vehicles** in **10.7 minutes**, achieving a ~3.2x faster-than-real-time throughput.

### 4. Data Integrity & Double-Count Prevention
I implemented a **Session Set** for tracked IDs combined with a **5% Dynamic Buffer**. 
- **Logic:** Once a vehicle's unique ID crosses the buffer zone at the 65% gate, it is locked into the "Counted" state. 
- **Result:** This prevents double-counting if a vehicle slows down or stops exactly on the detection line.
