# TV Calibrator

Calibrate your TV's picture with your iPhone. The phone's camera is the meter, and the test
patterns are videos you play on the TV. The app walks you through the TV's own menus, showing
which control to move, which way, and when it's on target.

By [Slidebean](https://www.youtube.com/@Slidebean). Open source under [CC BY-NC 4.0](LICENSE) (attribution, non-commercial).

**Get the app:** coming soon to the App Store. · [Support](mailto:calibrator@slidebean.com) ·
[Privacy policy](https://slidebean.com/tv-calibrator-privacy-policy)

## How it works

1. **Pattern videos on the TV.**
   - **SDR and HDR10** patterns are in the TV Calibrator playlists on the Slidebean YouTube channel.
     Play them in the TV's own YouTube app, or cast them from your phone.
   - **HDR10+ and Dolby Vision** patterns aren't supported by YouTube, so they're a download. Play
     them from a USB stick or a media app.
2. **The iPhone as the meter.** The phone sits on a stand facing the screen and reads the centre of
   each pattern from the camera's RAW sensor data. Lens, focus, ISO and, on the iPhone 18 Pro,
   aperture are all locked.
3. **Matched to a professional colorimeter.** Each iPhone model's camera has been measured
   side-by-side with a Calibrite Display Plus HL on several TVs. The result is a small **camera
   profile** that turns the sensor's numbers into real colour, so readings mean the same thing on
   every supported iPhone. Profiles are built into the app; newer ones (new models, retrained
   versions) download from the profile server. An iPhone without its own profile uses the
   **standard profile** (the iPhone 18 Pro's) and its readings are marked approximate.
4. **Guided steps.** Set up → Before → Bright end (Gain) → Dark end (Offset) → Check → Colours →
   After. There's also optional 10/20-point fine-tuning, with a "point finder" that works out which
   grey each of the TV's multipoint controls really moves. The app has menu paths for LG, Samsung,
   Sony, TCL, Hisense, Panasonic, Philips, Vizio, Toshiba, Sharp and Roku TVs. It works for SDR and
   HDR10 first (patterns on YouTube), then HDR10+ and Dolby Vision as advanced steps (pattern files).

Users don't need a Mac, a meter or an account. A **demo TV** (simulated readings and an on-screen
"remote") lets anyone try the app without a TV.

## What's in this repo

| Folder | What it is |
|---|---|
| `TV Calibrator.xcodeproj`, [`TV Calibrator/`](TV%20Calibrator) | The iPhone app (SwiftUI, iOS 18+). |
| [`CalibrationKit/`](CalibrationKit) | The calibration engine. The app builds its sources as a framework; `Package.swift` makes it a Swift package too, for the tests. |
| [`Config/`](Config) | The app's Info.plist additions (App Store build and Lab build). |
| [`server/`](server) | What the app downloads: `index.json` (supported iPhones), `profiles/` (one camera profile per model) and `patterns.json` (links to the pattern videos), Published by the push worker (`push-worker/`) on Cloudflare. |
| [`mac-lab/`](mac-lab) | The Mac lab tool, used with a Display Plus HL to create camera profiles for new iPhone models. Not needed by users. |
| [`tools/`](tools) | `build_catalog.py` publishes new camera profiles into `server/`; `make_fixtures.py` regenerates the engine's test data. |
| [`docs/`](docs) | How-tos: [adding an iPhone model](docs/ADDING-AN-IPHONE.md), [hosting on Cloudflare](docs/HOSTING.md), [the pattern videos](docs/PATTERN-VIDEOS.md), [how the engine works](docs/HOW-IT-WORKS.md), [testing and releasing](docs/RELEASE.md), [the launch checklist](docs/LAUNCH-CHECKLIST.md), [the App Store listing](docs/APP-STORE-LISTING.md), [publishing on GitHub](docs/PUBLISHING-ON-GITHUB.md). |

## Building the app

You need a Mac with **Xcode 27** (the iPhone 18 Pro's aperture control uses the iOS 27 SDK).

1. Open `TV Calibrator.xcodeproj`.
2. Pick the **TV Calibrator** scheme and your iPhone. In the target's **Signing & Capabilities**,
   choose your team.
3. Run.

The camera only works on a real iPhone. In the Simulator, use the demo TV.

The **TV Calibrator Lab** scheme builds a separate developer app ("TV Cal Lab") that can pair with
the Mac lab tool to train new camera profiles. That code isn't compiled into the App Store build.
See [docs/ADDING-AN-IPHONE.md](docs/ADDING-AN-IPHONE.md).

The server address isn't in the repo. To use downloads and notifications, copy
`Config/ServerConfig.example.plist` to `TV Calibrator/Resources/ServerConfig.plist` and put your own
worker's address in it (see [docs/HOSTING.md](docs/HOSTING.md)). Without it the app runs on its
built-in profiles.

## Tests

The engine is a Swift port of the Mac lab tool's Python engine. The tests feed both the same inputs
and check that every answer matches: colour maths, pattern timelines, sync detection, analysis,
guided hints and the point finder.

Open `CalibrationKit/Package.swift` in Xcode and press ⌘U, or from the terminal:

```sh
cd CalibrationKit
swift test
```

After changing either engine, regenerate the expected answers with
`python3 tools/make_fixtures.py`.

## Contributing

Issues and pull requests are welcome, especially:

- Menu paths for more TV brands: `TV Calibrator/Storage/Brands.swift`.
- Camera profiles for more iPhone models. You need a colorimeter; see
  [docs/ADDING-AN-IPHONE.md](docs/ADDING-AN-IPHONE.md).

## Community ports

- **Android:** [tv-calibrator-android](https://github.com/saqlain1020/tv-calibrator-android) by @saqlain1020. Unofficial and
  community-maintained; not made or supported by Slidebean.

## Reporting problems

Use [GitHub Issues](https://github.com/slidebean/tv-calibrator/issues). Include your iPhone model and your TV's brand and
model.

## License

TV Calibrator is licensed under [Creative Commons Attribution-NonCommercial 4.0](LICENSE)
([summary](https://creativecommons.org/licenses/by-nc/4.0/)). You're free to use, study, change and
share it, as long as you:

- **Give credit:** "TV Calibrator by Slidebean", with a link to https://github.com/slidebean/tv-calibrator, and say if you changed it.
- **Don't use it commercially:** no selling it, charging for it, or using it in a paid product or
  service. For commercial use, email calibrator@slidebean.com.

The Slidebean name and logo, and the TV Calibrator app icon, are Slidebean's trademarks and aren't
covered by the license: if you publish your own version of the app, give it your own name and icon.
