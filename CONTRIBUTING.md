# 贡献指南 / Contributing

<p>
<b><a href="#中文">简体中文</a></b> · <b><a href="#english">English</a></b>
</p>

> **维护预期**：本项目由个人业余维护，Issue 回复与 PR 合并**没有时限承诺**——
> 可能几天，也可能几周，但不会被无视。需要确定性支持请自行 fork（AGPL 允许）。
> **Maintenance note**: maintained in spare time; no SLA for replies or merges.
> Nothing is ignored, but patience is appreciated. Forks are legitimate.

---

<a id="中文"></a>

## 中文

欢迎 Issue 与 PR。语言不限，中文 / English / 繁體中文都可以。

### 提 Issue

- 报 bug 请附：设备与系统版本、复现步骤、预期 vs 实际、（如涉及 UI）截图。
- 涉及解析质量的，请附**完整口述原文与解析输出**（可隐去隐私内容，但请保持
  JSON 结构与引号原样——很多问题出在转义与引用上）。

### 提 PR 前必须通过

```bash
cd app
flutter analyze    # 零告警
flutter test       # 全量通过
```

三个硬红线（CI 与 reviewer 都会盯）：

1. **解析框架契约测试不许跳过或删除断言**。`app/test/prompt_builder_test.dart` 钉住了
   全部防胡编护栏（evidence 逐字可搜、未转义引号、条数上限、术语桥……），它们对应的是
   真实模型实测翻过的车。改 `app/assets/framework_prompt.md` 必须同步跑这组测试；
   改动护栏句子必须同步更新测试钉点，并在 PR 里说明理由。
2. **落盘键（SharedPreferences / 数据库字段名）不可更改**。改了会丢用户数据。
3. **不许添加任何形式的遥测/上报**。本项目零后端零埋点，这是产品承诺。

### 开发环境

- Flutter（开发基线 3.47.5）/ JDK 17 / Android SDK（minSdk 24）。
- 国内构建建议配置 Gradle/(pub.dev) 镜像源，避免网络问题误判为代码问题。
- 解析框架文档 `app/assets/framework_prompt.md` 由 `docs/解析理论框架_Prompt_v1.md`
  经 `app/tools/sync_prompt.mjs` 同步生成——**两份必须逐字节一致**（有测试钉住）。

### 行为准则

保持友善与就事论事；讨论聚焦代码与产品。对梦境话题保持尊重——这是每个人的私事。

---

<a id="english"></a>

## English

Issues and PRs are welcome in 中文 / English / 繁體中文.

### Filing issues

- For bug reports, please include: device and OS version, steps to reproduce,
  expected vs actual behavior, and (for UI issues) screenshots.
- For interpretation-quality issues, please attach the **full dictated transcript and
  the interpretation output** (you may redact private content, but keep the JSON
  structure and quotation marks intact — many bugs live in escaping and quoting).

### Before opening a PR

```bash
cd app
flutter analyze    # zero warnings
flutter test       # full suite must pass
```

Three hard lines (CI and reviewers will enforce them):

1. **Never skip or delete assertions in the interpretation-framework contract tests.**
   `app/test/prompt_builder_test.dart` pins all anti-hallucination guards
   (verbatim searchable evidence, unescaped quotes, the 2–5 item cap, the terminology
   bridge…). They exist because real models failed those exact ways in real testing.
   Any change to `app/assets/framework_prompt.md` must re-run this suite; changing a
   guard sentence requires updating the pinned assertion and explaining why in the PR.
2. **Persistence keys (SharedPreferences / database field names) must never change.**
   Changing them destroys user data.
3. **No telemetry of any kind.** This project has no backend and no analytics — that is
   a product promise.

### Development environment

- Flutter (baseline 3.47.5) / JDK 17 / Android SDK (minSdk 24).
- If you build from mainland China, consider Gradle/pub.dev mirror settings so network
  issues are not mistaken for code issues.
- The interpretation framework document `app/assets/framework_prompt.md` is generated
  from `docs/解析理论框架_Prompt_v1.md` via `app/tools/sync_prompt.mjs` — **the two
  copies must stay byte-identical** (a test enforces this).

### Code of conduct

Be kind and stay on topic; keep discussions focused on code and product. Treat dream
topics with respect — dreams are everyone's private life.
