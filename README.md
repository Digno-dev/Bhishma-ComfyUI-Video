# Bhishma Mahabharata - AI-Generated 10-Minute Video

> A cinematic AI-powered video project depicting the legendary warrior Bhishma from Mahabharata, created using ComfyUI with Wan 2.1/2.2 video models.

## Overview

This project generates a **10-minute continuous narrative** video featuring Bhishma, the immortal warrior and grandfather figure from the Hindu epic Mahabharata. The video combines **text-to-video (T2V)** and **image-to-video (I2V)** generation using Wan 2.1/2.2 models for a seamless, cinematic experience.

**Key Features:**
- 30+ video segments (20-25 seconds each, 24fps)
- Detailed scene prompts with cinematic descriptions
- Wan 2.1/2.2 text-to-video & image-to-video workflows
- Seamless segment stitching with overlap blending
- Full rendering configuration for GPU optimization
- Support for RTX 3060 to RTX 4090 GPUs

## Project Structure

```
Bhishma-ComfyUI-Video/
├── workflows/                    # ComfyUI Workflow JSON templates
│   ├── wan2.1_bhishma_t2v.json  # Text-to-Video workflow
│   └── wan2.1_bhishma_i2v.json  # Image-to-Video workflow
├── prompts/                      # Scene scripts & visual prompts
│   └── bhishma_scene_scripts.txt # 15 detailed segments with full prompts
├── config/                       # Configuration files
│   └── render_config.yaml        # Rendering settings, model params, optimization
├── output/                       # Generated video segments & final output
│   ├── segments/                 # Individual MP4 clips
│   └── bhishma_final_10min.mp4   # Final stitched video
├── README.md                     # This file
└── .gitignore                    # Git ignore file
```

## Installation & Setup

### Prerequisites
- **ComfyUI** (v0.3.68 or latest)
- **Python 3.9+**
- **GPU with 8GB+ VRAM** (RTX 3060 or better recommended)
- **PyTorch 2.0+**
- **ffmpeg** for video processing

### Step 1: Install ComfyUI

```bash
# Clone ComfyUI
git clone https://github.com/comfyanonymous/ComfyUI.git
cd ComfyUI

# Install dependencies
pip install -r requirements.txt

# For NVIDIA GPUs (recommended)
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

### Step 2: Download Models

Place models in ComfyUI folders:

```
ComfyUI/
├── models/
│   ├── checkpoints/
│   │   ├── wan2.1_t2v_1.3B_fp16.safetensors
│   │   └── wan2.1_i2v_1.3B_fp16.safetensors
│   ├── text_encoders/
│   │   └── umt5_xxl_fp8_e4m3fn_scaled.safetensors
│   ├── vae/
│   │   └── wan_2.1_vae.safetensors
│   └── clip_vision/
│       └── clip_vision_h.safetensors
```

**Download Sources:**
- Hugging Face: `Comfy-Org/Wan_2.1/2_ComfyUI_Repackaged`
- NVIDIA Hugging Face repos

### Step 3: Install ComfyUI Extensions

```bash
# Inside ComfyUI directory
cd custom_nodes

# VideoHelperSuite (essential for MP4 output)
git clone https://github.com/Kosinkadink/ComfyUI-VideoHelperSuite.git

# KJNodes (utility nodes)
git clone https://github.com/kijai/KJNodes.git
```

### Step 4: Clone This Project

```bash
git clone https://github.com/Digno-dev/Bhishma-ComfyUI-Video.git
cd Bhishma-ComfyUI-Video
```

## Workflow Usage

### Text-to-Video (T2V) Workflow

**File:** `workflows/wan2.1_bhishma_t2v.json`

1. Open ComfyUI web interface (http://localhost:8188)
2. Load workflow: `Workflow → Load` → Select `wan2.1_bhishma_t2v.json`
3. Update **CLIP Text Encode** node with desired prompt (see `prompts/bhishma_scene_scripts.txt`)
4. Adjust **KSampler** parameters:
   - **Steps:** 25-35 (higher = better quality, slower)
   - **CFG Scale:** 7.5-8.5 (higher = more prompt adherence)
   - **Seed:** Change for variations
5. Click **Queue** (Ctrl+Enter) to generate
6. Output MP4 saved to `output/segments/`

### Image-to-Video (I2V) Workflow

**File:** `workflows/wan2.1_bhishma_i2v.json`

1. First, generate reference image using SDXL/Flux or load existing Bhishma image
2. Load I2V workflow
3. Connect **Load Image** node to reference image path
4. Update **CLIP Text Encode** with motion prompt (e.g., "slowly raises Trishul")
5. Adjust sampler parameters (I2V usually needs fewer steps: 20-30)
6. Generate and save output

## Segment Breakdown (10-Minute Video)

### Segments 1-3: Introduction (60 seconds)
- **Segment 1:** Ethereal Entry - Bhishma materializes from divine light
- **Segment 2:** Standing in Glory - Wide shot on Kurukshetra battlefield
- **Segment 3:** Close-up Wisdom - Face reveal showing ancient knowledge

### Segments 4-6: Warrior Manifestation (60 seconds)
- **Segment 4:** Trishul Activation - Weapon glows with divine power
- **Segment 5:** Battle Ready Stance - Full warrior regalia display
- **Segment 6:** Celestial Transformation - Form shimmers with energy

### Segments 7-10: Battlefield Memories (80 seconds)
- **Segment 7:** Kurukshetra Memory Flash - Ancient war scene
- **Segment 8:** Past Glory - Young Bhishma commanding armies
- **Segment 9:** Sacrifice Moment - Emotional farewell scene
- **Segment 10:** Return to Present - Transition back to current form

### Segments 11-15: Philosophical Revelation (100 seconds)
- **Segment 11:** Ancient Teachings - Sanskrit verses floating in air
- **Segment 12:** Divine Connection - Cosmic energy threads
- **Segment 13:** Inner Glow - Third eye activation
- **Segment 14:** Truth Manifestation - Visual cosmic patterns
- **Segment 15:** Eternal Guardian - Final peaceful stance

### Segments 16-30+: Grand Finale (remaining 360+ seconds)
- Ascension sequence
- Multi-dimensional forms
- Celestial archives
- Final blessings
- Return to silence

## Rendering Configuration

Edit `config/render_config.yaml` to customize:

```yaml
# Video output settings
video_settings:
  resolution: "1024x576"    # Can change to 1280x720 or 1920x1080
  fps: 24                   # Video framerate
  codec: "h264"             # H.264 compression
  bitrate: "8000k"          # Output bitrate

# Generation parameters
generation_params:
  steps: 30                 # Sampling steps (25-40 recommended)
  cfg_scale: 7.5-8.5       # Classifier-free guidance
  sampler: "euler"         # Sampling method
  frames_per_segment: 20-25 # Frames per clip

# Optimization for GPU
optimization:
  use_fp8_quantization: true  # Memory efficient
  memory_efficient: true      # Enable attention optimization
  chunk_processing: true      # Process in chunks
```

## GPU Memory Requirements

| GPU Model | VRAM | Recommended Config |
|-----------|------|--------------------|
| RTX 3060 | 12GB | FP8, 1024x576, steps:25 |
| RTX 4070 | 12GB | FP16, 1024x576, steps:30 |
| RTX 4080 | 16GB | FP16, 1280x720, steps:35 |
| RTX 4090 | 24GB | FP16, 1920x1080, steps:40 |

## Prompt Engineering Tips

### Effective Prompt Structure
```
[CHARACTER] [ACTION] [SETTING] [LIGHTING] [STYLE] [QUALITY]
```

Example:
```
Bhishma slowly raises massive Trishul above head, sacred cosmic energy 
crackles around the prongs, Kurukshetra battlefield at golden hour sunset, 
dramatic volumetric lighting, epic cinematic cinematography, 8K quality
```

### Negative Prompts (What to Avoid)
```
blurry, distorted, cartoonish, low quality, modern, static, artifacts
```

## Video Stitching

To stitch all segments into final 10-minute video:

```bash
# Using ffmpeg
ffmpeg -f concat -safe 0 -i filelist.txt -c copy output/bhishma_final_10min.mp4
```

Segment overlap (3 frames) + fade transition (2 frames) handled in VHS_VideoCombine nodes.

## Performance & Estimated Times

**Single Segment (20 frames, 24fps ~0.83s duration):**
- RTX 3060: 4-6 minutes (steps:25)
- RTX 4080: 2-3 minutes (steps:35)

**Full 10-Minute Video (30 segments):**
- RTX 3060: 18-24 hours
- RTX 4080: 8-12 hours

**Time-Saving Options:**
- Use fewer steps (20-25 instead of 35)
- Lower resolution (768x432 instead of 1024x576)
- Reduce segment count (20 instead of 30)
- Parallel processing on multiple GPUs

## Troubleshooting

### Out of Memory (OOM) Error
```
Solution: Enable fp8_quantization in config, reduce resolution, fewer steps
```

### Low Quality Output
```
Solution: Increase steps (30→40), increase CFG (7.5→8.5), better prompts
```

### Video Codec Error
```
Solution: Install ffmpeg, install VHS extension, check codec compatibility
```

### Prompt Not Reflected
```
Solution: Increase CFG scale (7.5→8.5), use more descriptive prompts
```

## Advanced Usage

### Custom Character Reference

1. Generate Bhishma reference image (SDXL/Flux)
2. Load into I2V workflow
3. Add motion prompt describing desired movement
4. Generate smooth transitions between segments

### Audio Synchronization

```bash
# Add background music/narration
ffmpeg -i bhishma_final_10min.mp4 -i bhagavad_gita_narration.mp3 \
  -c:v copy -c:a aac -shortest final_with_audio.mp4
```

### Upscaling Final Video

```python
# Using Real-ESRGAN node in ComfyUI
# Load final video → Ultimate Upscale (2x) → save 1440p version
```

## Project Timeline

- **Phase 1:** Workflow setup & testing (Completed)
- **Phase 2:** Generate 30 video segments (In Progress)
- **Phase 3:** Seamless stitching & transitions (Planned)
- **Phase 4:** Post-processing & upscaling (Planned)
- **Phase 5:** Audio synchronization (Planned)

## Contributing

Contributions welcome! Please:
1. Create new prompts in `prompts/` folder
2. Optimize workflows for different GPUs
3. Add post-processing scripts
4. Improve documentation

## License

MIT License - See LICENSE file

## References

- **ComfyUI:** https://github.com/comfyanonymous/ComfyUI
- **Wan 2.1 Paper:** https://arxiv.org/abs/2408.09111
- **Mahabharata:** Hindu epic Sanskrit text

## Contact

For questions or suggestions, open an issue or reach out via GitHub.

---

**Created:** December 2025  
**Last Updated:** December 10, 2025  
**Status:** Active Development
