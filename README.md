# spank-kitten-sounds

> 拍一下你的 MacBook，它会发出奶猫的叫声。 🐱

## 这是个什么东西

一个**给打工人续命**的小玩意儿。

困的时候、累的时候、emo 的时候、被老板狠批之后、改了一晚上 bug 还没修好的时候、PPT 怎么都过不了审的时候 —— 别再灌咖啡了。**伸手拍一下你的 MacBook**，它会"喵"一声回应你。

- 真的"喵"。3 周龄奶猫那种细嫩到融化的喵。
- 没有任何屏幕弹窗、没有任何提示音、没有任何 App 图标在 dock 里晃。
- 就只是一只看不见的小猫，安安静静地住在你电脑里。
- 你拍它，它叫一声。你不拍它，它不打扰你。

这不是猫狗模拟器，也不是桌宠。它没有 UI，跑在后台，永远沉默 —— **除非你主动去拍它**。这就是它治愈的地方：你随时都能用一巴掌换一声"喵"。

---

## 它是怎么知道我在拍的

苹果在 M 系列芯片里塞了一颗 Bosch BMI286 加速度计，本来是给 MacBook 做开盖检测、跌落保护、姿态识别用的。

有个叫 [@taigrr](https://github.com/taigrr) 的开发者写了一个叫 [`spank`][spank] 的 CLI 小工具，把这颗传感器**当成了"拍打麦克风"**：直接读 IOKit HID 三轴加速度流，跑 4 个信号处理算法（STA/LTA、CUSUM、Kurtosis、Peak/MAD）叠加判断"这是不是一次真正的拍打"，识别成功就播个 MP3。

**本仓库不做任何技术上的事**。它就是 14 段 CC0 奶猫叫声。`spank` 自带的音色是各种"嗷""疼""停手"，本仓库把它换成奶猫，让这个工具从恶搞玩具变成治愈神器。

---

## 装它

需要：**M2 及以上**（或 M1 Pro）的 Apple Silicon MacBook + `sudo` 权限。基础 M1 / Intel Mac 都不行（没有那颗传感器或 IOKit 路径不对）。

### 第一步：装 `spank`

去 [spank releases 页][spank-releases]下载对应你系统的预编译二进制，解压后放到 `/usr/local/bin/spank`。或者用 Go 自己编：

```bash
go install github.com/taigrr/spank@latest
sudo cp "$(go env GOPATH)/bin/spank" /usr/local/bin/spank
```

### 第二步：克隆本仓库

```bash
git clone https://github.com/aurthur/spank-kitten-sounds.git ~/.spank/cat
```

### 第三步：开启猫猫

```bash
sudo spank --custom ~/.spank/cat/sounds
```

现在拍一下你的 MacBook。

按 `Ctrl + C` 退出。

---

## 进阶玩法

```bash
# 更灵敏 —— 轻拍也能触发，适合手轻的人
sudo spank --custom ~/.spank/cat/sounds --fast

# 力度感应 —— 轻拍小声喵、用力拍大声喵
sudo spank --custom ~/.spank/cat/sounds --volume-scaling

# 慢半拍 —— 喵叫变低沉绵长，更梦幻
sudo spank --custom ~/.spank/cat/sounds --speed 0.8
```

### 让它开机自启（推荐）

每次开机自动跑、永远在后台听你拍打。打开 Terminal 跑一次：

```bash
sudo tee /Library/LaunchDaemons/com.spank.kitten.plist > /dev/null << 'EOF'
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
  "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.spank.kitten</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/local/bin/spank</string>
        <string>--custom</string>
        <string>/Users/YOUR_USERNAME/.spank/cat/sounds</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
    <key>KeepAlive</key>
    <true/>
</dict>
</plist>
EOF

sudo launchctl load /Library/LaunchDaemons/com.spank.kitten.plist
```

> 记得把 `YOUR_USERNAME` 换成你自己的用户名。装完就再也不用手动跑了 —— 任何时候伸手拍一下，都会有猫。

要关掉：

```bash
sudo launchctl unload /Library/LaunchDaemons/com.spank.kitten.plist
```

---

## 音色清单

14 段奶猫，从最嫩到稍微大一点：

| 文件 | 来历 | 体感 |
|---|---|---|
| `kitten_3weeks.mp3` | 3 周龄小奶猫 | 最嫩，奶到化 |
| `kitten_2months.mp3` | 2 月龄小猫 | 嫩 + 绵长 |
| `small_mewing.mp3` | 小奶猫细叫 | 细脆 |
| `little_01.mp3` ~ `little_11.mp3` | 11 段"小猫一声喵" | 短促可爱，每段不同 |

不喜欢哪段直接删掉对应 mp3，`spank` 启动时扫目录，少一个就少一个。

---

## In English

A drop-in pack of 14 CC0 kitten meow sounds for [taigrr/spank][spank] — the macOS CLI that detects physical hits via the Apple Silicon accelerometer.

Slap your MacBook when you're tired / sad / burned out — hear a kitten meow back. No UI, no dock icon, no notifications. Just a tiny invisible cat living in your laptop.

```bash
git clone https://github.com/aurthur/spank-kitten-sounds.git ~/.spank/cat
sudo spank --custom ~/.spank/cat/sounds
```

Requires `spank` installed separately, Apple Silicon (M2+ or M1 Pro), and `sudo`.

---

## 致谢

- **[`spank`][spank]** — 真正干活的工具，由 [@taigrr](https://github.com/taigrr) 用 Go 写的，MIT License。没有它本仓库就是 14 个 mp3 躺在文件夹里。
- **音频** — 全部来自 [BigSoundBank][bsb]，CC0 公共领域。法律上不要求署名，但他们做了很多年值得提一句。

## License

CC0 1.0 Universal —— 公共领域。音频是 CC0、文档也是 CC0、整个仓库都是 CC0。**随便用、随便改、随便商用，不用问任何人。**

详见 [LICENSE](./LICENSE)。

[spank]: https://github.com/taigrr/spank
[spank-releases]: https://github.com/taigrr/spank/releases/latest
[bsb]: https://bigsoundbank.com/
