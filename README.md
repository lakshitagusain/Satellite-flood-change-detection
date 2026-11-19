# Satellite Flood Change Detection Prototype (Gandak River, 2026)

A lightweight computer vision prototype for automated pre-/post-disaster flood expansion detection using multi-temporal satellite imagery, Python, and OpenCV.

## Overview
Rapidly mapping inundated zones is a foundational step in post-disaster infrastructure assessment and rescue route planning. This prototype demonstrates a classical image-processing baseline using **NASA Terra/Aqua MODIS false-color imagery (Bands 7-2-1)** from the September 2026 Gandak River floods along the Nepal–India border.

## Pipeline
1. **Spatial Alignment:** Loads and aligns pre-flood (Sept 20, 2026) and during-flood (Sept 29, 2026) satellite frames.
2. **Spectral Change Detection:** Isolates newly inundated areas by detecting sharp drops in the Near-Infrared/Green channel (vegetation transitioning to water) while filtering out high-reflectance cloud cover.
3. **Morphological Filtering:** Applies opening and closing operations (`3x3` kernel) to suppress speckle noise and connect flood regions.
4. **Area Estimation & Overlay:** Quantifies the newly flooded area (`+2.08%` of the frame) and renders a visual change-detection overlay.

## Output
![Gandak River Flood Change Detection](gandak_flood_change_detection.png)

*Data Source: NASA Earth Observatory (MODIS Bands 7-2-1), September 20 & September 29, 2026.*
## Challenges & Iterative Refinements
- **Spectral Ambiguity in HSV Thresholding:** Initial single-frame HSV blue-range thresholding under-detected the flooded Gandak River (which appears dark teal/blue-green in MODIS 7-2-1 imagery) while triggering false positives on light-blue cloud edges.
- **Cloud Interference & Pivot to Differential Change Detection:** Rather than relying on static color bounds, the pipeline was refactored to detect sharp pre-to-post drops in the Green (Near-Infrared) channel—directly capturing the transition from bright green vegetation to water—while applying a Red-channel ceiling to suppress bright cloud cover.

## Limitations Observed & Next Steps
- **Spatial Resolution:** Moderate-resolution MODIS imagery captures regional floodplain expansion well, but cannot resolve narrow mountain roads or street-level blockages.
- **Cloud Occlusion:** While optical thresholding filters out visible clouds, dense monsoon cloud cover still obscures underlying terrain.
- **Future Direction:** Extending this baseline to deep semantic segmentation (U-Net / DeepLabV3+ in PyTorch), incorporating cloud-penetrating SAR radar, and exploring cross-regional transfer learning for mountainous disaster corridors.

## Tech Stack
- Python, OpenCV, NumPy, Matplotlib (Google Colab)
