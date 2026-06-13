# spank-kitten-sounds

> Slap your MacBook, hear a kitten. 🐱

A drop-in pack of **14 CC0 kitten meow sounds** for [taigrr/spank][spank] — the macOS CLI that detects physical hits on Apple Silicon MacBooks via the IOKit HID accelerometer and plays audio responses.

This repo doesn't replace `spank`. It only provides the audio. Install `spank` separately, then point it at this folder with `--custom`.

---

## What's in the box

14 royalty-free kitten meows sourced from [BigSoundBank][bsb] (CC0 / public domain), arranged roughly from softest to most expressive:

| File | Source | Vibe |
|---|---|---|
| `kitten_3weeks.mp3` | 3-week-old kitten | Tiny, helpless, devastating |
| `kitten_2months.mp3` | 2-month-old kitten | Squeaky, persistent |
| `small_mewing.mp3` | Small mewing of a cat | Crisp, short |
| `little_01.mp3` … `little_11.mp3` | 11 different "little meow of a cat" takes | Quick single-syllable meows, each a bit different |

Total: ~1.5 MB.

---

## Requirements

`spank` itself needs:

- macOS on Apple Silicon (**M2 or newer**, or **M1 Pro** specifically — base M1 / A-series don't work)
- `sudo` (for IOKit HID accelerometer access)

This pack itself needs nothing — it's just MP3 files.

---

## Install

### 1. Install `spank`

```bash
# Easiest: grab a prebuilt binary from the spank releases page
# https://github.com/taigrr/spank/releases/latest

# Or build from source (requires Go 1.26+):
go install github.com/taigrr/spank@latest
sudo cp "$(go env GOPATH)/bin/spank" /usr/local/bin/spank
```

### 2. Grab this sound pack

```bash
git clone https://github.com/aurthur/spank-kitten-sounds.git ~/.spank/cat
# or just download the sounds/ folder and put it anywhere
```

### 3. Run

```bash
sudo spank --custom ~/.spank/cat/sounds
```

Now slap your MacBook. You should hear a kitten.

---

## Recommended flag combos

```bash
# More sensitive — light taps trigger sounds
sudo spank --custom ~/.spank/cat/sounds --fast

# Volume scales with slap intensity (gentle pat = soft meow, hard slap = loud meow)
sudo spank --custom ~/.spank/cat/sounds --volume-scaling

# Lower-pitched, slightly slower playback — sounds dreamier
sudo spank --custom ~/.spank/cat/sounds --speed 0.8
```

`Ctrl + C` to stop.

---

## Caveats vs. `spank --sexy`

`spank`'s built-in `--sexy` mode has 60 levels of escalation based on slap frequency over a 5-minute window. Custom mode does **not** — it just picks a random clip from this folder each time. The escalation logic is hard-coded against the embedded `sexy` MP3 set inside the `spank` binary.

If you want a kitten version with frequency-based escalation, you'd need to fork `spank` itself and swap the embedded `audio/sexy/` directory before recompiling.

---

## 中文说明

这是一个给 [taigrr/spank][spank] 用的奶猫音频包。spank 是一个 macOS CLI 小工具 —— 拍一下你的 MacBook，它会用 Apple Silicon 加速度计感知，然后播一段声音。这个包就是 14 段从 [BigSoundBank][bsb] 下载的 CC0 奶猫叫声。

**装法**：

1. 先按上面 `Install` 步骤装 `spank`
2. 把本仓库 clone 到 `~/.spank/cat`
3. 跑 `sudo spank --custom ~/.spank/cat/sounds`
4. 拍 MacBook，听喵叫
5. `Ctrl + C` 退出

**要求**：M2 及以上（或 M1 Pro）的 Apple Silicon MacBook，需要 `sudo`。

---

## Credits

- **`spank`** — the actual brain behind this. Built by [@taigrr][taigrr]. Licensed under [MIT][spank-license]. Without it, this folder is just 14 MP3 files in a directory.
- **Audio** — all 14 clips are sourced from [BigSoundBank][bsb] under CC0 / Public Domain. Attribution is optional per their license, but they deserve it.

## License

CC0 1.0 Universal — Public Domain Dedication. See [LICENSE](./LICENSE).

Both the audio (originally CC0 from BigSoundBank) and the documentation / packaging in this repo are released into the public domain. Do whatever you want.

[spank]: https://github.com/taigrr/spank
[spank-license]: https://github.com/taigrr/spank/blob/master/LICENSE
[taigrr]: https://github.com/taigrr
[bsb]: https://bigsoundbank.com/
