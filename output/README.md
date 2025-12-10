# Output Folder Organization

This directory contains all generated video content for the Bhishma Epic project.

## Directory Structure

```
output/
├── segments/          # Individual 10-second video clips (60 files)
├── final/            # Final stitched 10-minute video
└── temp/             # Temporary files during processing
```

## Segments Directory (`segments/`)

Contains all 60 individual video segments, each 10 seconds long at 24 fps.

### Naming Convention:
- Format: `bhishma_segment_XXX.mp4`
- Range: `bhishma_segment_001.mp4` to `bhishma_segment_060.mp4`

### Segment Mapping (by Act):

**Act 1: Introduction & Youth** (Segments 1-15)
- `bhishma_segment_001.mp4` - Opening: The Vow (Scene 1)
- `bhishma_segment_002.mp4` - Opening: The Vow (Scene 2)
- `bhishma_segment_003.mp4` - Opening: The Vow (Scene 3)
- `bhishma_segment_004.mp4` - Youth as Devavrata (Scene 4)
- `bhishma_segment_005.mp4` - Youth as Devavrata (Scene 5)
- `bhishma_segment_006.mp4` - Youth as Devavrata (Scene 6)
- `bhishma_segment_007.mp4` - The Father's Dilemma (Scene 7)
- `bhishma_segment_008.mp4` - The Father's Dilemma (Scene 8)
- `bhishma_segment_009.mp4` - The Father's Dilemma (Scene 9)
- `bhishma_segment_010.mp4` - The Terrible Vow (Scene 10)
- `bhishma_segment_011.mp4` - The Terrible Vow (Scene 11)
- `bhishma_segment_012.mp4` - The Terrible Vow (Scene 12)
- `bhishma_segment_013.mp4` - Transformation to Bhishma (Scene 13)
- `bhishma_segment_014.mp4` - Transformation to Bhishma (Scene 14)
- `bhishma_segment_015.mp4` - Transformation to Bhishma (Scene 15)

**Act 2: The Guardian Years** (Segments 16-30)
- `bhishma_segment_016.mp4` - Protector of Hastinapura (Scene 16)
- `bhishma_segment_017.mp4` - Protector of Hastinapura (Scene 17)
- `bhishma_segment_018.mp4` - Protector of Hastinapura (Scene 18)
- `bhishma_segment_019.mp4` - Training the Princes (Scene 19)
- `bhishma_segment_020.mp4` - Training the Princes (Scene 20)
- `bhishma_segment_021.mp4` - Training the Princes (Scene 21)
- `bhishma_segment_022.mp4` - The Dice Game Silence (Scene 22)
- `bhishma_segment_023.mp4` - The Dice Game Silence (Scene 23)
- `bhishma_segment_024.mp4` - The Dice Game Silence (Scene 24)
- `bhishma_segment_025.mp4` - Preparation for War (Scene 25)
- `bhishma_segment_026.mp4` - Preparation for War (Scene 26)
- `bhishma_segment_027.mp4` - Preparation for War (Scene 27)
- `bhishma_segment_028.mp4` - Commander of Kaurava Army (Scene 28)
- `bhishma_segment_029.mp4` - Commander of Kaurava Army (Scene 29)
- `bhishma_segment_030.mp4` - Commander of Kaurava Army (Scene 30)

**Act 3: Kurukshetra War** (Segments 31-45)
- `bhishma_segment_031.mp4` - First Day of Battle (Scene 31)
- `bhishma_segment_032.mp4` - First Day of Battle (Scene 32)
- `bhishma_segment_033.mp4` - First Day of Battle (Scene 33)
- `bhishma_segment_034.mp4` - Facing Arjuna (Scene 34)
- `bhishma_segment_035.mp4` - Facing Arjuna (Scene 35)
- `bhishma_segment_036.mp4` - Facing Arjuna (Scene 36)
- `bhishma_segment_037.mp4` - Krishna's Intervention (Scene 37)
- `bhishma_segment_038.mp4` - Krishna's Intervention (Scene 38)
- `bhishma_segment_039.mp4` - Krishna's Intervention (Scene 39)
- `bhishma_segment_040.mp4` - The Shikhandi Strategy (Scene 40)
- `bhishma_segment_041.mp4` - The Shikhandi Strategy (Scene 41)
- `bhishma_segment_042.mp4` - The Shikhandi Strategy (Scene 42)
- `bhishma_segment_043.mp4` - The Fall (Scene 43)
- `bhishma_segment_044.mp4` - The Fall (Scene 44)
- `bhishma_segment_045.mp4` - The Fall (Scene 45)

**Act 4: The Bed of Arrows** (Segments 46-55)
- `bhishma_segment_046.mp4` - Lying on Arrows (Scene 46)
- `bhishma_segment_047.mp4` - Lying on Arrows (Scene 47)
- `bhishma_segment_048.mp4` - Lying on Arrows (Scene 48)
- `bhishma_segment_049.mp4` - Final Teachings (Scene 49)
- `bhishma_segment_050.mp4` - Final Teachings (Scene 50)
- `bhishma_segment_051.mp4` - Final Teachings (Scene 51)
- `bhishma_segment_052.mp4` - Waiting for Uttarayana (Scene 52)
- `bhishma_segment_053.mp4` - Waiting for Uttarayana (Scene 53)
- `bhishma_segment_054.mp4` - The Departure (Scene 54)
- `bhishma_segment_055.mp4` - The Departure (Scene 55)

**Act 5: Legacy** (Segments 56-60)
- `bhishma_segment_056.mp4` - Mourning (Scene 56)
- `bhishma_segment_057.mp4` - Mourning (Scene 57)
- `bhishma_segment_058.mp4` - Legacy Remembered (Scene 58)
- `bhishma_segment_059.mp4` - Legacy Remembered (Scene 59)
- `bhishma_segment_060.mp4` - Closing Credits Visual (Scene 60)

### Expected File Properties:
- **Format**: MP4 (H.264)
- **Resolution**: 1280x720 (16:9)
- **Frame Rate**: 24 fps
- **Duration**: 10 seconds (240 frames)
- **File Size**: ~8-12 MB per segment

## Final Directory (`final/`)

Contains the completed stitched video.

### Files:
- `Bhishma_Epic_Final.mp4` - Main 10-minute video (600 seconds)
- `Bhishma_Epic_Final_with_Music.mp4` - Version with background music (optional)

### Expected File Properties:
- **Format**: MP4 (H.264)
- **Resolution**: 1280x720 (16:9)
- **Frame Rate**: 24 fps
- **Duration**: 600 seconds (10 minutes)
- **File Size**: ~600-800 MB

## Temp Directory (`temp/`)

Used for intermediate processing files. Can be safely deleted after final video is created.

### May Contain:
- Re-encoded segments
- Transition clips
- Audio processing files
- Concat list files
- FFmpeg log files

## Workflow

1. **Generate Segments**: Use ComfyUI with provided workflows to create 60 segments
2. **Save to Segments**: Each clip goes to `output/segments/` with proper naming
3. **Process**: Use scripts/tools in `config/` to stitch segments
4. **Final Output**: Complete video saved to `output/final/`
5. **Cleanup**: Remove `output/temp/` contents when done

## Gitignore Note

The `.gitignore` file excludes all video files (*.mp4) to prevent large files from being committed to the repository. Only the folder structure and this README are tracked.
