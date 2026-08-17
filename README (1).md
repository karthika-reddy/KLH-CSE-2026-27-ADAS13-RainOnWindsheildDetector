# Rain-on-Windshield Detector
### A Real-Time Image-Processing System for Automated Wiper Control

**Domain:** Perception & Environmental Sensing (Advanced Driver Assistive Systems – ADAS)
**Team:** Karthika · Likitha · Arjun · Abhilash

---

## Abstract

Reduced visibility due to rain is a major contributor to degraded driver
perception and delayed manual wiper activation, both of which increase
accident risk. This project presents a **lightweight, classical
image-processing pipeline** that detects raindrops on a windshield-facing
camera feed, classifies the current rain intensity into one of four levels
(No Rain, Light, Moderate, Heavy), and automatically maps that level to a
wiper-speed control signal (Off / Low / Medium / High). Unlike deep-learning
raindrop detectors, the proposed system relies only on grayscale conversion,
denoising, edge detection, and local texture/contrast analysis — making it
computationally cheap, fully explainable, and deployable on low-cost
embedded hardware without a large labelled training set. The system is
evaluated on the publicly available **RaindropsOnWindshield** dataset
(Soboleva & Shipitko, 2021) for detection accuracy and per-frame response
time across varying lighting and rain conditions.

---

## Objectives

1. **Capture & Pre-process** — Acquire live windshield-facing video and
   prepare each frame (grayscale conversion, noise removal, and cropping to
   the windshield region of interest).
2. **Detect Raindrops** — Identify raindrops using classical image
   processing (edge detection, texture/contrast analysis, and optional
   frame-to-frame motion cues) — lightweight and easy to explain, with no
   deep learning required.
3. **Classify & Control** — Grade rain intensity as No Rain / Light /
   Moderate / Heavy, and map each level to a corresponding wiper-speed
   control signal.
4. **Evaluate the System** — Measure detection accuracy and response time
   across different lighting and rain conditions using a labelled dataset.

---

## Methodology

The system follows a single, linear pipeline from camera frame to wiper
signal:

| Stage | Technique | Output |
|---|---|---|
| 1. Capture | Onboard/windshield camera frame acquisition | Raw BGR frame |
| 2. Pre-processing | Grayscale conversion → Gaussian blur (denoise) → ROI crop | Cleaned grayscale ROI |
| 3. Raindrop Detection | Canny edge detection + Laplacian-based local texture/contrast analysis, combined and cleaned with morphological opening/closing | Binary raindrop mask |
| 4. Intensity Classification | Threshold the raindrop pixel density (% of ROI flagged) into 4 buckets | Rain level (No Rain / Light / Moderate / Heavy) |
| 5. Wiper Control Mapping | Direct lookup from rain level to control signal | Wiper signal (Off / Low / Medium / High) |

**Why classical image processing instead of deep learning?**
It runs on low-cost hardware, needs no large labelled training set, and
every step (edges, texture, density thresholding) can be explained in plain
terms — making the system easier to build, demonstrate, debug, and defend
in a project review, while still meeting real-time performance needs.

**Evaluation approach:** the pipeline is run over a labelled set of frames
(real dataset samples plus controlled synthetic raindrop overlays for
ground-truth density control), recording predicted vs. actual rain level
and per-frame processing time to report classification accuracy, a
confusion matrix, and average FPS.

---

## System Architecture

```
Camera Frame
     │
     ▼
Pre-process  (grayscale → denoise → ROI crop)
     │
     ▼
Detect        (edge detection + texture/contrast → binary mask)
     │
     ▼
Classify      (drop density → No Rain / Light / Moderate / Heavy)
     │
     ▼
Control       (rain level → wiper signal: Off / Low / Medium / High)
```

---

## Repository Structure

```
project/
├── README.md
├── src/
│   └── rain_detector.py      # Core detection & classification pipeline
├── data/
│   ├── clean/                # Clean base road/windshield frames
│   ├── synthetic/            # Frames with controlled synthetic raindrops
│   └── real_official/        # Official dataset sample images + masks
└── outputs/
    ├── figures/               # Pipeline-stage visualizations, confusion matrix
    └── ...                    # Evaluation CSV / summary reports
```

---

## Applications

- **Rain / Weather Detection** — auto-trigger wipers and adjust ADAS
  confidence when raindrops are detected.
- **Lane & Object Detection** — support continued lane/vehicle/pedestrian
  detection as visibility drops.
- **Sensor Fusion** — combine with radar/LiDAR for a weather-robust
  environmental model.

## Future Scope

- **Sensor Fusion for Redundancy** — combine with humidity/rain sensors for
  higher reliability.
- **Edge-AI Deployment** — run on low-cost edge hardware for real-time,
  low-latency response.
- **Predictive Control** — integrate visibility-based speed advisories and
  predictive wiper-speed control.

---

## Dataset Links
- **Raindrop Clarity** (Jin et al., 2024)
  - GitHub: https://github.com/jinyeying/RaindropClarity
---

## Research Paper References

> *Add or update paper links below as the literature survey evolves.*

- Soboleva, V., & Shipitko, O. (2021). *Raindrops on Windshield: Dataset and
  Lightweight Gradient-Based Detection Algorithm.* arXiv:2104.05078.
  https://arxiv.org/abs/2104.05078
- Li, C.-H. G., Chen, K.-W., Lai, C.-C., & Hwang, Y.-T. (2021). *Real-Time
  Rain Detection and Wiper Control Employing Embedded Deep Learning.* IEEE
  Transactions on Vehicular Technology, 70(4), 3256–3266.
  DOI: https://doi.org/10.1109/TVT.2021.3066677 ·
  IEEE Xplore: https://ieeexplore.ieee.org/document/9380922/
- Jin, Y., Li, X., Wang, J., Zhang, Y., & Zhang, M. (2024). *Raindrop
  Clarity: A Dual-Focused Dataset for Day and Night Raindrop Removal.* ECCV
  2024, LNCS vol. 15064.
  arXiv: https://arxiv.org/abs/2407.16957 ·
  GitHub: https://github.com/jinyeying/RaindropClarity
- Zhang, G., Wang, G., Chen, J., Jiang, W., Hao, X., & Deng, T. (2025). *An
  Automatic Control System Based on Machine Vision and Deep Learning for
  Car Windscreen Clean.* Scientific Reports, 15, Article 4857.
  https://www.nature.com/articles/s41598-025-88688-9

---

## Team

| Name | ID |
|---|---|
| Karthika | 2420030076 |
| Likitha | 2420030028 |
| Arjun | 2420030559 |
| Abhilash | 2420030026 |
