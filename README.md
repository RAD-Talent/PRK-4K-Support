# PRK 4K Support — readable UI on 4K / 5K monitors

Anarchy Online's 18.4 engine draws its UI in fixed pixels and has **no UI scaling** — on a 4K or 5K monitor everything becomes microscopic, and playing at a low resolution fullscreen looks blurry. This fix gives you a **large, sharp, screen-filling window with a readable UI**, using the dgVoodoo2 layer already bundled with the PRK launcher plus one tuned config file.

Tested on a 5K Apple Studio Display (5120×2880 @ 200% Windows scaling), Windows 11.

## Requirements
- PRK launcher (the DirectX layer feature ships with it)
- Windows 10/11 with display scaling at 125% or more (Settings → System → Display → Scale — 4K monitors typically run 150%, 5K runs 200%). The fix uses that scaling to enlarge the game window, so at 100% scale it won't do anything.

## Install (5 minutes, fully reversible)

**1. Install the config**
- Download `dgVoodoo.conf` from this repo
- Go to your PRK folder (the one containing `PRK.Launcher.exe`), then `dist\directx\dgVoodoo2_83_2\`
- Rename the existing `dgVoodoo.conf` there to `dgVoodoo.conf.backup`
- Copy the downloaded `dgVoodoo.conf` in its place
- (The launcher copies this file into your client folder on every launch — that's why it goes here and not in `client\`)

**2. Turn on the dgVoodoo layer**
- PRK Launcher → Settings → **Advanced** → DirectX layer: **dgVoodoo283_2** → Save

**3. One-time Windows setting (this is what makes the window big)**
- In your `client` folder, right-click **Anarchy.exe** → Properties → Compatibility → **Change high DPI settings** → tick **"Override high DPI scaling behavior"** and set the dropdown to **System** → OK → Apply
- Repeat the exact same steps for **AnarchyOnline.exe**

**4. Set your resolution**
- PRK Launcher → Settings → **Display** → Mode: **Windowed**, and set the resolution using this rule: **your monitor's native resolution ÷ your Windows scale factor, minus a little height for the title bar and taskbar.** Examples:

| Monitor | Windows scale | Launcher resolution |
|---|---|---|
| 5K (5120×2880) | 200% | **2500 × 1300** |
| 4K (3840×2160) | 150% | **2400 × 1330** |
| 4K (3840×2160) | 200% | **1850 × 1000** |

- Bigger numbers = more game on screen but smaller UI. Tweak to taste — the window size follows the launcher resolution directly.

**5. Play.** The window opens scaled up by your Windows scale factor (e.g. 2500×1300 displays at 5000×2600 on a 5K screen) — filling the screen with the taskbar still visible, UI at a comfortable size, and clean rendering (8× antialiasing + 16× texture filtering from the config).

## What the config changes (vs. the launcher's default)
- `Antialiasing = 8x`, `Filtering = 16` — smooth edges and sharp textures at any distance
- `KeepFilterIfPointSampled = true`, `RTTexturesForceScaleAndMSAA = false` — stops the enhancements from distorting UI elements (fixes line/edge artifacts)
- `FullScreenMode = false`, `AppControlledScreenMode = false` — keeps presentation windowed and well-behaved
- `dgVoodooWatermark = false` — no logo overlay
- `ExtraEnumeratedResolutions` — lets the game accept the non-standard resolutions this setup uses
- `ScalingMode = stretched_ar`, `CenterAppWindow = true` — sane scaling/positioning if you do use fullscreen

## Troubleshooting
- **Game opens tiny**: the DPI override (step 3) isn't applied to both exes, or your Windows scale is 100%.
- **Game opens at 800×600 in fullscreen mode**: your resolution isn't in the game's list — open `dgVoodoo.conf`, find `ExtraEnumeratedResolutions`, and add yours (comma-separated).
- **Settings seem to reset**: the launcher re-copies the config from `dist\directx\dgVoodoo2_83_2\` on every launch — make sure you edited THAT copy, not the one in `client\`.
- **Revert everything**: set DirectX layer back to None, restore `dgVoodoo.conf.backup`, and untick the DPI overrides on the two exes.

## Credits
- dgVoodoo2 by Dege (https://dege.fw.hu/) — bundled with the PRK launcher, used with permission by the PRK team
- Fix worked out and tested by **Everkill** — questions/improvements: ping **.everkill** on Discord

*See also: [PRK Map](https://github.com/RAD-Talent/PRK-Map) and the [PRK WhatBuffs & Item Database](https://github.com/RAD-Talent/PRK-Items).*
