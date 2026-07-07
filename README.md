# cloche-ffmpeg-kit-ios

Self-hosted iOS FFmpeg binaries for **Cloche** (Metaroots' restaurant clip
maker). Exists because [FFmpeg-Kit was retired in 2025](https://tanersener.medium.com/saying-goodbye-to-ffmpegkit-33ae939767e1)
and every official binary was deleted; this repo rebuilds them from the
archived official source under our own account so no third party can break
the app's build again.

## What it produces

`ffmpeg-kit-full-6.0-ios-xcframework.zip` — the eight FFmpeg-Kit
xcframeworks (`ffmpegkit`, `libavcodec`, `libavdevice`, `libavfilter`,
`libavformat`, `libavutil`, `libswresample`, `libswscale`), **"full" LGPL
variant**:

- All non-GPL external libraries (freetype + fontconfig for caption
  `drawtext`, and the rest of the standard full set).
- **No GPL code** — `ios.sh` refuses x264/x265/xvid/vid.stab/rubberband
  without `--enable-gpl`, which the workflow never passes. H.264 encoding in
  the app uses Apple's `h264_videotoolbox` hardware encoder instead.
- Suitable for paid / paywalled App Store distribution (LGPL v3 with dynamic
  frameworks; license texts ship inside each framework).

## How to build

1. Fork `arthenica/ffmpeg-kit` (archived) into this account — that fork is
   the source custody.
2. Actions → **build-ffmpeg-kit-ios-full-lgpl** → *Run workflow* (defaults
   build tag `v6.0` from the fork).
3. ~2–4 h later a Release appears with the zip + `SHA256SUMS.txt`.

The Cloche app's vendored podspec
(`third_party/ffmpeg-kit-react-native/ios/ffmpeg-kit-ios-full.podspec` in the
app repo) points at that Release URL and pins the SHA256.

## Licensing

The binaries are unmodified builds of FFmpeg-Kit v6.0 / FFmpeg 6.0. FFmpeg is
licensed under **LGPL v3.0** in this configuration; external libraries carry
their own licenses (all non-GPL), bundled inside each `.xcframework`. Source
code: the `ffmpeg-kit` fork under this account. This repo's own files (the
workflow and this README) are MIT.
