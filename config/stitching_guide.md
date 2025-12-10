# Video Stitching Guide
# Instructions for combining 60 segments into final 10-minute video

## Prerequisites
- FFmpeg installed (version 4.0 or higher)
- All 60 segment files in output/segments/ directory
- Segments named: bhishma_segment_001.mp4 through bhishma_segment_060.mp4
- At least 20GB free disk space

## Method 1: Using FFmpeg Concat Demuxer (Recommended)

### Step 1: Generate Concat List File
Create a file named `concat_list.txt` in the config folder:

```bash
# Generate the concat list
cd /home/runner/work/Bhishma-ComfyUI-Video/Bhishma-ComfyUI-Video
python3 scripts/generate_concat_list.py
```

Or manually create `concat_list.txt` with this format:
```
file 'output/segments/bhishma_segment_001.mp4'
file 'output/segments/bhishma_segment_002.mp4'
file 'output/segments/bhishma_segment_003.mp4'
...
file 'output/segments/bhishma_segment_060.mp4'
```

### Step 2: Stitch Videos
```bash
ffmpeg -f concat -safe 0 -i config/concat_list.txt \
  -c:v libx264 -preset slow -crf 18 -profile:v high \
  -c:a aac -b:a 192k \
  -pix_fmt yuv420p \
  output/final/Bhishma_Epic_Final.mp4
```

## Method 2: Using FFmpeg with Transitions

### For Cross-Dissolve Transitions (1 second each):
```bash
ffmpeg -i output/segments/bhishma_segment_001.mp4 \
       -i output/segments/bhishma_segment_002.mp4 \
       -filter_complex "[0:v][1:v]xfade=transition=fade:duration=1:offset=9[v0]; \
                        [0:a][1:a]acrossfade=d=1[a0]" \
       -map "[v0]" -map "[a0]" output/temp/segment_01_02.mp4
```

Note: For 60 segments with transitions, use the batch script provided.

## Method 3: Using Python Script (Automated)

```bash
cd /home/runner/work/Bhishma-ComfyUI-Video/Bhishma-ComfyUI-Video
python3 scripts/stitch_videos.py --config config/rendering_settings.json
```

## Adding Background Music

### Option 1: During Stitching
```bash
ffmpeg -f concat -safe 0 -i config/concat_list.txt \
  -i assets/audio/bhishma_theme.mp3 \
  -filter_complex "[1:a]volume=0.3,afade=t=in:st=0:d=3,afade=t=out:st=595:d=5[music]; \
                   [0:a][music]amix=inputs=2:duration=first[aout]" \
  -map 0:v -map "[aout]" \
  -c:v libx264 -preset slow -crf 18 \
  -c:a aac -b:a 192k \
  output/final/Bhishma_Epic_Final_with_Music.mp4
```

### Option 2: After Stitching
```bash
ffmpeg -i output/final/Bhishma_Epic_Final.mp4 \
  -i assets/audio/bhishma_theme.mp3 \
  -filter_complex "[1:a]volume=0.3,afade=t=in:st=0:d=3,afade=t=out:st=595:d=5[music]; \
                   [0:a][music]amix=inputs=2:duration=first[aout]" \
  -map 0:v -map "[aout]" \
  -c:v copy -c:a aac -b:a 192k \
  output/final/Bhishma_Epic_Final_with_Music.mp4
```

## Quality Control Checks

### 1. Verify Segment Count
```bash
ls output/segments/bhishma_segment_*.mp4 | wc -l
# Should output: 60
```

### 2. Check Individual Segment Duration
```bash
ffprobe -v error -show_entries format=duration \
  -of default=noprint_wrappers=1:nokey=1 \
  output/segments/bhishma_segment_001.mp4
# Should output: ~10.000000
```

### 3. Verify Final Video Duration
```bash
ffprobe -v error -show_entries format=duration \
  -of default=noprint_wrappers=1:nokey=1 \
  output/final/Bhishma_Epic_Final.mp4
# Should output: ~600.000000 (10 minutes)
```

### 4. Check Video Properties
```bash
ffprobe -v error -select_streams v:0 \
  -show_entries stream=width,height,r_frame_rate,codec_name \
  output/final/Bhishma_Epic_Final.mp4
```

## Troubleshooting

### Issue: Segments have different resolutions
```bash
# Re-encode all segments to same resolution
for i in output/segments/*.mp4; do
  ffmpeg -i "$i" -vf scale=1280:720 -c:v libx264 -crf 18 \
    -c:a copy "output/temp/$(basename "$i")"
done
```

### Issue: Audio sync problems
```bash
# Re-encode with audio sync fix
ffmpeg -f concat -safe 0 -i config/concat_list.txt \
  -async 1 -c:v libx264 -preset slow -crf 18 \
  output/final/Bhishma_Epic_Final.mp4
```

### Issue: Color banding
```bash
# Add dithering during encoding
ffmpeg -f concat -safe 0 -i config/concat_list.txt \
  -vf "format=yuv420p,dither=sierra2_4a" \
  -c:v libx264 -preset slow -crf 18 \
  output/final/Bhishma_Epic_Final.mp4
```

## Performance Tips

1. **Use hardware acceleration** (if available):
   ```bash
   ffmpeg -hwaccel cuda -f concat -safe 0 -i config/concat_list.txt ...
   ```

2. **Parallel processing** for re-encoding:
   ```bash
   parallel -j 4 ffmpeg -i {} ... ::: output/segments/*.mp4
   ```

3. **Two-pass encoding** for better quality:
   ```bash
   # Pass 1
   ffmpeg -f concat -safe 0 -i config/concat_list.txt \
     -c:v libx264 -preset slow -b:v 8M -pass 1 -f mp4 /dev/null
   
   # Pass 2
   ffmpeg -f concat -safe 0 -i config/concat_list.txt \
     -c:v libx264 -preset slow -b:v 8M -pass 2 \
     output/final/Bhishma_Epic_Final.mp4
   ```

## Final Output Specifications

- **Resolution**: 1280x720 (HD)
- **Frame Rate**: 24 fps
- **Duration**: 600 seconds (10 minutes)
- **Video Codec**: H.264 (libx264)
- **Video Bitrate**: 8 Mbps
- **Audio Codec**: AAC
- **Audio Bitrate**: 192 kbps
- **File Size**: ~600-800 MB (estimated)
