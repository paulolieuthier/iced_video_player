# Iced Video Player Widget

Composable component to play videos in any Iced application built on the excellent GStreamer library.

<img src=".media/screenshot.png" width="50%" />

## Overview

In general, this supports anything that [`gstreamer/playbin`](https://gstreamer.freedesktop.org/documentation/playback/playbin.html?gi-language=c) supports.

Features:
- Load video files from any file path **or URL** (support for streaming over network).
- Video buffering when streaming on a network.
- Audio support.
- Programmatic control.
- Can capture thumbnails from a set of timestamps.
- Good performance (i.e., comparable to other video players). GStreamer (with the right plugins) will perform hardware-accelerated decoding, and the color space (YUV to RGB) is converted on the GPU whilst rendering the frame.

Limitations (hopefully to be fixed):
- GStreamer is a bit annoying to set up on Windows.

The player **does not** come with any surrounding GUI controls, but they should be quite easy to implement should you need them.
See the "minimal" example for a demonstration on how you could implement pausing, looping, and seeking.

## Example Usage

```rust
use iced_video_player::{Video, VideoPlayer};

fn main() -> iced::Result {
    iced::run((), App::view)
}

struct App {
    video: Video,
}

impl Default for App {
    fn default() -> Self {
        App {
            video: Video::new(&url::Url::parse("file:///C:/my_video.mp4").unwrap()).unwrap(),
        }
    }
}

impl App {
    fn view(&self) -> iced::Element<std::convert::Infallible> {
        VideoPlayer::new(&self.video).into()
    }
}
```

## Cargo features

- `iced-defaults` *(enabled by default)* — enables iced's default features:
  the tiny-skia software renderer, web colors, the thread-pool executor,
  theme detection (with its zbus stack) and the platform backends. Disable
  it with `default-features = false` to trim the dependency graph; iced
  then needs a windowing backend and an executor enabled from another
  dependency in the build.

## Building

Follow the [GStreamer build instructions](https://github.com/sdroege/gstreamer-rs#installation). This should be able to compile on MSVC, MinGW, Linux, and MacOS.

## License

Licensed under either

- [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0)
- [MIT](http://opensource.org/licenses/MIT)

at your option.
