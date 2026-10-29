# Dream Reader

**梦境记录 + 严肃 AI 解析 · 每条结论挂在可查证的原文片段上**

<p align="center">
<b><a href="#简体中文">简体中文</a></b> ·
<b><a href="#english">English</a></b>
</p>

---

<a id="简体中文"></a>

## 简体中文

### 简介

Dream Reader 是一款**纯本地的梦境记录应用**，核心是它的解析引擎：

- **录音即录**：梦醒后点一下就开始录，全程无表单；文字、录音、时间、标签都在本机。
- **严肃解析，不做词典式胡编**：解析基于封闭的理论框架（精神分析），每条结论必须
  **引用口述原文里连续、逐字的一段话作为证据**——用户可以拿着这句话回原文里搜，
  搜不到即为不合格。这是本产品的立身之本。
- **置信度分级**：每条结论标注 A（文本直接可见）/ B（理论推演）/ C（开放假设），
  不假装确定。
- **离线语音识别**：本地 Paraformer 大模型（按需下载，约 320MB 含标点模型），
  长录音自动分段解码，无需网络、无厂商依赖。
- **BYOK 解析**：用户提供 API Key（DeepSeek / 智谱 / 通义 / Kimi / 硅基流动 / OpenAI /
  Gemini / OpenRouter / 本地 Ollama 预设），文本只发给你自己配置的供应商。

社区版（本仓库）使用精神分析框架；**[周公解梦版](https://github.com/nonetail22/dream-reader-zhougong)**
把解析流派换成古传周公解梦体系，其余功能完全一致，两者可在同一部手机上并存安装。

### 功能特性

- 🎙️ **点击式录音**，开始/结束一按即达；录音与转写文字都留在本机
- 🗣️ **三种转写路线**（系统识别 / 云端 / 离线），离线模型字节数 + SHA-256 双校验、可取消下载
- 🔍 **解析证据链**：`evidence` 字段必须是原文里连续的一段、逐字复制；省略号截断、
  标点拼接都被契约测试钉死为不合格
- 📊 **置信度三级**（A/B/C）+ 术语人话桥 +「无法判定」转成追问而非结论
- 🗂️ **时间线管理**：状态筛选（未解析/已解析/结论过期/解析失败）+ 关键字搜索
- ✏️ **原文随时可改**，改后自动标记旧解析「已过期」——但绝不静默重解析（那是花你的钱）
- 🌙 **5 套夜间配色** + 跟随系统（夜间默认暖棕烛光），三语界面（简中/繁中/English）
- 🔒 **零后端、零埋点**：没有服务器，没有账号，数据就是你手机上的 SQLite

### 隐私

- 录音、转写文字、解析结果**只存在本机**（SQLite + 系统安全存储），没有服务器、没有遥测。
- 使用解析功能时，梦境文本只会发送到**你自己配置的 API 供应商**；应用本身不转发、不留存。
- 离线识别模型从开源镜像下载（下载时校验哈希），识别全程不联网。

### 免责声明

- 所有解析结果均由 AI 生成，**仅供娱乐参考**，不构成任何预言、占卜、医学或心理学建议，
  不能替代专业心理帮助。若梦境长期困扰你，请寻求专业人士支持。
- 理论框架是解析的**思维工具**，不构成对现实的断言；请勿依据解析结果做出医疗、财务
  或其他重大决策。

### 下载

从 [Releases](https://github.com/nonetail22/dream-reader/releases) 下载 APK（arm64-v8a）。
当前版本 **0.7.5**，应用内「设置 → 关于与说明」可查看版本号。

### 从源码构建

```bash
git clone https://github.com/nonetail22/dream-reader.git
cd dream-reader/app
flutter pub get
flutter build apk --release --split-per-abi
# 产物：build/app/outputs/flutter-apk/app-arm64-v8a-release.apk
```

环境要求：Flutter（开发时使用 3.47.5）/ JDK 17 / Android SDK（minSdk 24）。
运行测试：

```bash
flutter analyze   # 静态检查
flutter test      # 全量测试（含解析框架契约测试）
```

### 解析引擎

解析范式的单一真源是 [`app/assets/framework_prompt.md`](app/assets/framework_prompt.md)
（由 [`docs/解析理论框架_Prompt_v1.md`](docs/解析理论框架_Prompt_v1.md) 同步生成），
配套契约测试 [`app/test/prompt_builder_test.dart`](app/test/prompt_builder_test.dart)
钉住所有防胡编护栏：

| 护栏 | 内容 |
|---|---|
| 证据可核对 | `evidence` 必须是原文连续逐字片段，禁省略、禁拼接，子串匹配必须命中 |
| 置信度诚实 | A/B/C 逐条标注；「凡引用 T4（开放联想）必须 ≤C 级」是硬规则 |
| 说人话 | 白话结论（headline/point）禁止术语；术语必须在 `terms` 里给一句人话解释 |
| 不交付无知 | 「无法判定」必须改写为具体追问（`open_questions`），且问完了要有明确表达 |
| 输出可解析 | JSON 字符串值禁未转义引号（否则整份输出作废）；重试指令点名转义规则 |

### 与周公解梦版的关系

| | 社区版（本仓库） | [周公解梦版](https://github.com/nonetail22/dream-reader-zhougong) |
|---|---|---|
| 解析流派 | 精神分析（弗洛伊德/荣格/HVdC） | 古传周公解梦（Z1-Z4 典籍体系） |
| 包名 | `com.dreamreader.dream_reader` | `com.dreamreader.dream_reader_zhougong` |
| 其余功能 | — | 完全一致，可并存安装 |

两版共享同一套工程护栏与输出契约；解析流派差异全部收敛在各自仓库的
`app/assets/framework_prompt.md` 一个文件里。

### 贡献

欢迎 Issue 与 PR，见 [CONTRIBUTING.md](CONTRIBUTING.md)。
Bug 报告请附设备信息与复现步骤；解析质量的反馈尤其宝贵。

### 维护说明

- 本项目由个人**业余时间维护**，**无法承诺 Issue 的回复时间**——可能几天，也可能几周。
  遇到问题欢迎先翻历史 Issue，也欢迎社区互相解答。
- 标题带 `[critical]`（数据丢失 / 隐私 / 崩溃）的 Issue 会优先处理。
- PR 不会无人问津，但合并节奏可能较慢，请耐心。
- 需要确定性支持或想按自己的节奏演进：本项目是 AGPL，欢迎直接 fork 自行维护。

### 许可证

本项目以 [**GNU AGPL-3.0-or-later**](LICENSE) 开源。使用到的关键开源组件：
[Flutter](https://flutter.dev)、[sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx)
（Apache-2.0）、FunASR Paraformer 模型（按需下载，遵循其自身许可证）。

---

<a id="english"></a>

## English

**Dream journaling + serious AI interpretation — every conclusion anchored to a verifiable quote from your own words**

### About

Dream Reader is a **fully local dream journal app**. Its core is the interpretation engine:

- **One-tap recording**: wake up, tap once, start talking. No forms. Everything stays on-device.
- **Serious interpretation, no dictionary-style nonsense**: the engine is bound to a closed
  theoretical framework (psychoanalysis). Every conclusion must cite **a continuous, verbatim
  span of your own dictated text as evidence** — you can search that exact sentence in your
  transcript. If it can't be found, the output is rejected. This is the product's foundation.
- **Honest confidence levels**: every point is graded A (visible in text) / B (theoretical) /
  C (open hypothesis). No fake certainty.
- **Offline speech recognition**: a local Paraformer model (downloaded on demand, ~320MB
  including punctuation), long recordings decoded in segments. No cloud dependency.
- **BYOK interpretation**: you provide your own API key (DeepSeek / Zhipu / Qwen / Kimi /
  SiliconFlow / OpenAI / Gemini / OpenRouter / local Ollama presets). Dream text is only
  sent to the provider **you** configured.

This community edition uses the psychoanalytic framework. The companion
**[Zhou Gong edition](https://github.com/nonetail22/dream-reader-zhougong)** swaps the
interpretation school for the classical Chinese dream tradition — everything else is
identical, and both apps can be installed side by side.

### Features

- 🎙️ **Tap-to-record**; audio and transcripts stay on your device
- 🗣️ **Three transcription routes** (system / cloud / offline), model downloads verified by
  size + SHA-256, cancellable
- 🔍 **Traceable evidence chain**: the `evidence` field must be a verbatim, contiguous quote
  from your transcript — ellipsis truncation and punctuation-splicing are pinned as failures
  by contract tests
- 📊 **Three confidence grades** + plain-language glossary bridge + "cannot determine"
  becomes a follow-up question, not a non-answer
- 🗂️ **Timeline**: filter by status (unanalyzed / analyzed / stale / failed) + keyword search
- ✏️ **Editable transcripts**; edits automatically mark old interpretations as stale — and
  never silently re-analyze (that costs your money)
- 🌙 **5 dark palettes** + follow-system mode (warm candlelight at night by default), and a
  trilingual UI (Simplified Chinese / Traditional Chinese / English)
- 🔒 **No backend, no telemetry**: no server, no account — your data is a SQLite file on
  your phone

### Privacy

- Recordings, transcripts and interpretations **exist only on your device**
  (SQLite + system secure storage). No server, no analytics.
- When analyzing, dream text is sent **only to the API provider you configured**. The app
  never forwards or retains it.
- The offline model is downloaded from open mirrors (hash-verified); recognition itself is
  fully offline.

### Disclaimer

- All interpretations are AI-generated, **for entertainment only**. They are not prophecy,
  divination, medical or psychological advice, and cannot replace professional help. If
  distressing dreams persist, please seek professional support.
- The theoretical framework is a **thinking tool**, not a claim about reality — do not make
  medical, financial or other major decisions based on an interpretation.

### Download

Grab the APK (arm64-v8a) from the
[Releases](https://github.com/nonetail22/dream-reader/releases) page. Current version:
**0.7.5** (also visible in-app under *Settings → About*).

### Building from source

```bash
git clone https://github.com/nonetail22/dream-reader.git
cd dream-reader/app
flutter pub get
flutter build apk --release --split-per-abi
# Output: build/app/outputs/flutter-apk/app-arm64-v8a-release.apk
```

Requirements: Flutter (developed against 3.47.5) / JDK 17 / Android SDK (minSdk 24).

```bash
flutter analyze   # static analysis
flutter test      # full test suite, incl. interpretation-framework contract tests
```

### The interpretation engine

The single source of truth is
[`app/assets/framework_prompt.md`](app/assets/framework_prompt.md)
(generated from [`docs/解析理论框架_Prompt_v1.md`](docs/解析理论框架_Prompt_v1.md)),
guarded by contract tests in
[`app/test/prompt_builder_test.dart`](app/test/prompt_builder_test.dart):

| Guard | Content |
|---|---|
| Verifiable evidence | `evidence` must be a contiguous verbatim span; no ellipsis, no splicing; substring match must hit |
| Honest confidence | A/B/C per point; "anything citing T4 (open association) must be ≤C" is a hard rule |
| Plain language | Plain-language conclusions ban jargon; every term used must get a one-line explanation in `terms` |
| Don't ship ignorance | "Cannot determine" must become a concrete follow-up question; "no more questions" is an explicit state |
| Parseable output | No unescaped quotes inside JSON strings (otherwise the whole output is void); retry instruction names the escaping rule |

### The Zhou Gong edition

| | Community edition (this repo) | [Zhou Gong edition](https://github.com/nonetail22/dream-reader-zhougong) |
|---|---|---|
| Interpretation school | Psychoanalysis (Freud / Jung / Hall–Van de Castle) | Classical Zhou Gong tradition (Z1–Z4 classics) |
| Application ID | `com.dreamreader.dream_reader` | `com.dreamreader.dream_reader_zhougong` |
| Everything else | — | Identical; installs side by side |

Both editions share the same engineering guards and output contract; the school difference
lives entirely in each repo's `app/assets/framework_prompt.md`.

### Contributing

Issues and PRs are welcome — see [CONTRIBUTING.md](CONTRIBUTING.md). Bug reports with
device info and repro steps, and quality feedback on interpretation output, are
especially valuable.

### Maintenance expectations

- This project is maintained **in spare time by one person**. There is **no SLA for
  issue replies** — it may take days or weeks. Please check existing issues first;
  community answers are very welcome.
- Issues titled with `[critical]` (data loss / privacy / crashes) are prioritized.
- PRs will never be ignored, but merging can be slow — please be patient, and feel
  free to ping in the comment thread.
- If you need guaranteed support or want to move at your own pace: this project is
  AGPL — forking and maintaining your own branch is a perfectly legitimate option.

### License

Released under the [**GNU AGPL-3.0-or-later**](LICENSE). Key open-source components:
[Flutter](https://flutter.dev),
[sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx) (Apache-2.0),
FunASR Paraformer models (downloaded on demand; subject to their own licenses).
