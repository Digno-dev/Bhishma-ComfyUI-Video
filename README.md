# Bhishma-ComfyUI-Video

Epic 10-minute video project telling the legendary story of Bhishma from the Mahabharata using ComfyUI and Wan 2.1 AI video generation models.

## 📋 Project Overview

This project uses AI-powered video generation to create a cinematic retelling of Bhishma's story, from his sacred vow as Devavrata to his final moments on the bed of arrows at Kurukshetra. The complete video consists of 60 segments (10 seconds each) organized into 5 acts.

### Story Structure (10 Minutes Total)

- **Act 1: Introduction & Youth** (2.5 min) - Devavrata's vow and transformation to Bhishma
- **Act 2: The Guardian Years** (2.5 min) - Protector of Hastinapura and mentor to princes
- **Act 3: Kurukshetra War** (2.5 min) - Epic battle scenes and confrontations
- **Act 4: The Bed of Arrows** (1.67 min) - Final teachings and departure
- **Act 5: Legacy** (0.83 min) - Mourning and eternal legacy

## 🗂️ Project Structure

```
Bhishma-ComfyUI-Video/
├── workflows/                    # ComfyUI workflow templates
│   ├── wan_2.1_t2v_workflow.json    # Text-to-Video workflow
│   └── wan_2.1_i2v_workflow.json    # Image-to-Video workflow
│
├── prompts/                      # Scene scripts and character profiles
│   ├── bhishma_character_profile.md # Detailed character description
│   └── scene_scripts.md             # 60 scenes with detailed prompts
│
├── config/                       # Rendering and stitching configuration
│   ├── rendering_settings.json      # Video encoding settings
│   └── stitching_guide.md          # FFmpeg stitching instructions
│
├── output/                       # Generated video clips
│   ├── segments/                    # Individual 10-second clips (60 files)
│   ├── final/                       # Stitched 10-minute video
│   └── temp/                        # Temporary processing files
│
└── README.md                     # This file
```

## 🚀 Setup Instructions

### Prerequisites

1. **ComfyUI Installation**
   ```bash
   # Clone ComfyUI
   git clone https://github.com/comfyanonymous/ComfyUI.git
   cd ComfyUI
   
   # Install dependencies
   pip install -r requirements.txt
   ```

2. **Wan 2.1 Model Installation**
   ```bash
   # Navigate to ComfyUI models directory
   cd ComfyUI/models/checkpoints/
   
   # Download Wan 2.1 models from official sources
   # Option 1: From Hugging Face (if available)
   # huggingface-cli download [model-repository] --local-dir ./
   
   # Option 2: Manual download
   # Visit the official Wan 2.1 repository or model hosting platform
   # Download both T2V (Text-to-Video) and I2V (Image-to-Video) models
   # Place the model files in ComfyUI/models/checkpoints/
   
   # Expected files:
   # - wan_2.1_t2v.safetensors (or .ckpt)
   # - wan_2.1_i2v.safetensors (or .ckpt)
   ```
   
   **Note**: Check the official Wan 2.1 documentation or ComfyUI community forums for the latest model download links and installation instructions.

3. **FFmpeg Installation**
   ```bash
   # Ubuntu/Debian
   sudo apt update && sudo apt install ffmpeg
   
   # macOS
   brew install ffmpeg
   
   # Windows
   # Download from https://ffmpeg.org/download.html
   ```

4. **Clone This Repository**
   ```bash
   git clone https://github.com/Digno-dev/Bhishma-ComfyUI-Video.git
   cd Bhishma-ComfyUI-Video
   ```

### System Requirements

- **GPU**: NVIDIA GPU with 12GB+ VRAM (RTX 3090, RTX 4090, or better)
- **RAM**: 32GB+ recommended
- **Storage**: 100GB+ free space for models and output
- **OS**: Linux (Ubuntu 20.04+), Windows 10/11, or macOS

## 🎬 Video Generation Workflow

### Step 1: Import Workflows to ComfyUI

1. Launch ComfyUI:
   ```bash
   cd ComfyUI
   python main.py
   ```

2. Open ComfyUI in browser (default: http://localhost:8188)

3. Import workflows:
   - Click "Load" button
   - Select `workflows/wan_2.1_t2v_workflow.json` or `wan_2.1_i2v_workflow.json`

### Step 2: Generate Video Segments

Generate each of the 60 segments following the scene scripts in `prompts/scene_scripts.md`:

#### For Text-to-Video Segments (Most Scenes):

1. Load `wan_2.1_t2v_workflow.json` in ComfyUI
2. For each scene (1-60):
   - Copy the **Prompt** from `prompts/scene_scripts.md`
   - Paste into "Positive Prompt" node
   - Copy the **Negative Prompt** 
   - Paste into "Negative Prompt" node
   - Adjust seed for variation if needed
   - Click "Queue Prompt" to generate
   - Save output as `bhishma_segment_XXX.mp4` in `output/segments/`

#### For Image-to-Video Segments (Optional Enhancement):

1. Load `wan_2.1_i2v_workflow.json` in ComfyUI
2. Load a key frame image (e.g., from a T2V-generated segment)
3. Use motion prompts from scene scripts
4. Generate enhanced version with controlled motion

### Step 3: Organize Segments

Ensure all 60 segments are in `output/segments/` with correct naming:
```bash
# Verify segment count
ls output/segments/*.mp4 | wc -l
# Should show: 60

# Check naming pattern
ls output/segments/ | head -5
# Should show:
# bhishma_segment_001.mp4
# bhishma_segment_002.mp4
# bhishma_segment_003.mp4
# ...
```

### Step 4: Stitch Videos Together

Follow the detailed instructions in `config/stitching_guide.md`:

```bash
# Quick method using FFmpeg concat
cd Bhishma-ComfyUI-Video

# Generate concat list
for i in {1..60}; do 
  printf "file 'output/segments/bhishma_segment_%03d.mp4'\n" $i >> config/concat_list.txt
done

# Stitch videos
ffmpeg -f concat -safe 0 -i config/concat_list.txt \
  -c:v libx264 -preset slow -crf 18 -profile:v high \
  -c:a aac -b:a 192k -pix_fmt yuv420p \
  output/final/Bhishma_Epic_Final.mp4
```

### Step 5: Add Background Music (Optional)

```bash
ffmpeg -i output/final/Bhishma_Epic_Final.mp4 \
  -i assets/audio/bhishma_theme.mp3 \
  -filter_complex "[1:a]volume=0.3,afade=t=in:st=0:d=3,afade=t=out:st=595:d=5[music]; \
                   [0:a][music]amix=inputs=2:duration=first[aout]" \
  -map 0:v -map "[aout]" \
  -c:v copy -c:a aac -b:a 192k \
  output/final/Bhishma_Epic_Final_with_Music.mp4
```

## 📝 Segment Sequence Reference

### Complete Scene List (60 Segments)

| Segment | Act | Scene Name | Duration | Time Range |
|---------|-----|------------|----------|------------|
| 001-003 | 1 | Opening: The Vow | 30s | 00:00-00:30 |
| 004-006 | 1 | Youth as Devavrata | 30s | 00:30-01:00 |
| 007-009 | 1 | The Father's Dilemma | 30s | 01:00-01:30 |
| 010-012 | 1 | The Terrible Vow | 30s | 01:30-02:00 |
| 013-015 | 1 | Transformation to Bhishma | 30s | 02:00-02:30 |
| 016-018 | 2 | Protector of Hastinapura | 30s | 02:30-03:00 |
| 019-021 | 2 | Training the Princes | 30s | 03:00-03:30 |
| 022-024 | 2 | The Dice Game Silence | 30s | 03:30-04:00 |
| 025-027 | 2 | Preparation for War | 30s | 04:00-04:30 |
| 028-030 | 2 | Commander of Kaurava Army | 30s | 04:30-05:00 |
| 031-033 | 3 | First Day of Battle | 30s | 05:00-05:30 |
| 034-036 | 3 | Facing Arjuna | 30s | 05:30-06:00 |
| 037-039 | 3 | Krishna's Intervention | 30s | 06:00-06:30 |
| 040-042 | 3 | The Shikhandi Strategy | 30s | 06:30-07:00 |
| 043-045 | 3 | The Fall | 30s | 07:00-07:30 |
| 046-048 | 4 | Lying on Arrows | 30s | 07:30-08:00 |
| 049-051 | 4 | Final Teachings | 30s | 08:00-08:30 |
| 052-053 | 4 | Waiting for Uttarayana | 20s | 08:30-08:50 |
| 054-055 | 4 | The Departure | 20s | 08:50-09:10 |
| 056-057 | 5 | Mourning | 20s | 09:10-09:30 |
| 058-059 | 5 | Legacy Remembered | 20s | 09:30-09:50 |
| 060 | 5 | Closing Credits Visual | 10s | 09:50-10:00 |

**Total Duration**: 10 minutes (600 seconds)

## 🎨 Character & Visual Guidelines

Refer to `prompts/bhishma_character_profile.md` for detailed character descriptions including:
- Physical appearance and attire
- Personality traits for visual expression
- Armor and weapon details
- Visual keywords for AI generation

## ⚙️ Configuration Files

### `config/rendering_settings.json`
Complete rendering configuration including:
- Video resolution and frame rate settings
- Encoding parameters (codec, bitrate, quality)
- Act markers and timestamps
- Color grading presets per act
- Audio settings

### `config/stitching_guide.md`
Comprehensive guide for video stitching with:
- Multiple stitching methods (concat, transitions)
- Quality control checks
- Troubleshooting solutions
- Performance optimization tips

## 🎯 Tips for Best Results

1. **Consistent Style**: Use similar prompt structure across all segments
2. **Seed Management**: Keep seeds consistent for related scenes
3. **Quality Check**: Preview each segment before proceeding
4. **Batch Processing**: Generate similar scenes in batches
5. **Color Grading**: Apply act-specific color grading during stitching
6. **Transitions**: Use 1-second cross-dissolves between scenes
7. **Audio Sync**: Align background music with act changes

## 🔧 Troubleshooting

### Common Issues:

**Issue**: Out of memory during generation
- **Solution**: Reduce resolution temporarily or use fp16 precision

**Issue**: Segments have different aspect ratios
- **Solution**: Re-encode with consistent settings (see `config/stitching_guide.md`)

**Issue**: Color inconsistency between segments
- **Solution**: Apply color grading presets from `config/rendering_settings.json`

**Issue**: Long generation times
- **Solution**: Use batch processing and optimize GPU settings

## 📄 License

This project structure is provided as-is for creative and educational purposes. Please respect the terms of use for ComfyUI and the Wan 2.1 model.

## 🙏 Acknowledgments

- ComfyUI team for the amazing workflow interface
- Wan 2.1 model developers
- The timeless epic of Mahabharata for the story

## 📧 Contact & Contribution

For issues, suggestions, or contributions, please open an issue or pull request on GitHub.

---

**Note**: This project requires substantial computational resources and time. Plan for several hours of generation time for all 60 segments depending on your hardware.