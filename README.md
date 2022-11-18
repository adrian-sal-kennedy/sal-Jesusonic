# **sal-Jesusonic**
Just a bit of fiddling with JS plugins for Reaper. I like to stream all my system audio into Reaper via Jack, and also get sick of everything on the Internet being completely random loudness.

I frankenstein copypasted 3 plugins together and turned it into a long acting gated compressor that by default turns everything down to -21LUFS. By "gated" I mean it applies gating as per EBU r128 - anything 8dB (or LU) quieter than the target level is ignored, so gaps between tracks, long silences, etc will not result in the gain creeping back up, but a quieter passage will cause the AGC to release slowly until it's either -21LUFS or quieter. Currently anything too quiet will just not be touched - you can always lower the default threshold and increase default gain if you want to catch really quiet stuff, but the vast VAST majority of audio out there is much louder than -21.

**Credit goes to Michael Gruhn, Thomas Scott Stillwell, Robert Bristow-Johnson for actually conceiving and writing the code that I've here mashed together.**

## **LUFS-compressor**
### **K-weighted long acting compressor.**
- **Threshold** - The target max LUFS. Default is -21.
- **Ratio** - Compression ratio. This thing acts over such a long time that this setting isn't particularly useful, but it behaves exactly the same as any other compressor.
- **Attack** - Time in ms that the compressor will take to respond to loudness changes.
- **Release** - Time the compressor takes to get back to unity gain. It's worth noting that release is overridden if the input is too quiet, which prevents "noise suck-up" that naive AGC circuits might otherwise produce.
- **RMS Size** - length of the time window used to measure input loudness.
- **Auto Make-up** (to be removed) - compensating gain based on threshold. Not relevant to this plugin, but compressors use it so go nuts.
- **Output** - Apply extra gain after compression is applied.
- **Character** - Compress or limit. Makes very little difference in this use case, but with shorter RMS, Attack and Release will be more aggressive in Limit mode.
- **Detector Input** - Measure loudness off channels 1+2 (Normal) or 3+4 (Sidechain). Useful if you want to use another kind of weighting, or just use it with sidechaining for some reason.
- **Metering** - How to determine loudness. Naive RMS or K-Weighting as defined in EBU r128 (+3dB high shelf at 1kHz, highpass at 100Hz).
- **RMS Relative Gating** - Anything quieter than this over an RMS window will be ignored when tracking loudness over time. This stops quiet passages from artificially pumping the volume back up and causing a very loud pulse when the regular level resumes. EBU r128 says this should be 8LU.
- **Gate Hold Time** - Extra parameter to prevent immediate release. Works together with relative gating.