# 课堂同传 · Lecture Live Translate

一个单文件网页小工具：用麦克风听英语讲课，实时显示中文翻译。

有两种模式：

- **免费版**：不用注册、不用 API key，用 Chrome 打开就能用
- **API 版**：填入自己的 OpenAI API key，用 OpenAI 实时语音识别 + GPT / Claude 带上下文翻译，识别和翻译都准很多

**在线使用：** https://jiasendu.github.io/lecture-live-translate/

## 功能

- 🎙️ **实时识别**：持续听麦克风，边说边出英文，不用一句一句按按钮
- 🈯 **实时翻译**：英文转简体中文，先出草稿，说完一句再更新成正式翻译
- 🪟 **悬浮窗**：弹出一个始终置顶的小窗口，边看课件边看字幕
- 🔠 **可调字号**，可隐藏英文原文
- 💾 **导出文本**：一键把整节课的中英对照保存为 `.txt`，方便复习
- 🔌 **两种翻译引擎**：Chrome 内置离线翻译 / Google 在线翻译，自动选择
- 🗣️ **口音选择**（免费版）：美式 / 英式 / 新加坡 / 印度 / 澳洲
- 🚀 **API 版**：OpenAI `gpt-live-transcribe` 实时识别，GPT 或 Claude 结合前文翻译，可填写课程主题和专业词

## 使用方法

### 方式一：在线打开（推荐）

用 Chrome 打开上面的在线地址，点 **开始**，允许使用麦克风即可。

### 方式二：本地使用

1. 下载本仓库中的 `index.html`
2. 右键 → 打开方式 → **Google Chrome**
3. 点 **开始**，允许使用麦克风

## API 版

免费版识别不准时（口音重、专业词多、离讲台远），可以切到 **API 版**：

1. 到 [platform.openai.com](https://platform.openai.com/api-keys) 充值并创建一个 API key（ChatGPT 会员不包含 API，需要单独付费）
2. 右上角把“免费版”切到 **API 版**，在弹出的设置里填入 key
3. 翻译可以选 GPT（用同一个 key）、Claude（需要 [Claude API key](https://console.anthropic.com/settings/keys)）或者免费翻译
4. 可选：填上课程主题和专业词、人名，识别和翻译会更准

**费用**（以官方价格为准）：识别约 $0.017/分钟，一节 90 分钟的课约 $1.5；翻译通常只要几美分。页面右上角会显示本次已用时长和识别费用估算。

**Key 的安全**：key 只保存在你自己的浏览器里（可以选择不保存），页面直接连接 OpenAI / Anthropic，不经过任何其他服务器。公用电脑上请取消“记住设置”。

## 浏览器支持

| 浏览器 | 语音识别 | 翻译 | 悬浮窗 |
| --- | --- | --- | --- |
| Chrome（桌面版，最新） | ✅ | ✅ 内置离线 / Google 在线 | ✅ |
| Edge（最新） | ✅ | ✅ Google 在线 | ✅ |
| Firefox / Safari | ❌ | — | — |

## 常见问题

**点开始没反应 / 提示麦克风被拒绝**
点地址栏左边的图标，把“麦克风”改成“允许”，然后刷新页面。

**识别不出来或者识别很差**
语音识别使用的是系统默认麦克风。可以在 Windows“设置 → 系统 → 声音”里确认输入设备是否正确。教室里离讲台远的话，外接麦克风效果会好很多。

**提示网络错误**
Chrome 的语音识别需要联网。

**翻译失败**
在右上角切换翻译引擎后再试。

**说一会儿就停了**
Chrome 会在长时间静音后自动断开，页面会自动重新连接，一般不需要手动操作。

## 工作原理

整个工具只有一个 HTML 文件，没有后端，没有第三方依赖：

- 语音识别：免费版用浏览器的 [Web Speech API](https://developer.mozilla.org/docs/Web/API/Web_Speech_API)；API 版用 [OpenAI Realtime 转写](https://developers.openai.com/api/docs/guides/realtime-transcription)（WebSocket 直连，页面自己判断停顿并分句）
- 翻译：Chrome 内置的 [Translator API](https://developer.chrome.com/docs/ai/translator-api)（离线），不可用时使用 Google 翻译的公开接口
- 悬浮窗：[Document Picture-in-Picture API](https://developer.chrome.com/docs/web-platform/document-picture-in-picture)

## 隐私说明

本项目不收集、不上传任何数据，页面本身没有服务器。但请注意：

- Chrome 和 Edge 的语音识别由浏览器厂商的云端服务完成，音频会发送到 Google（Chrome）或 Microsoft（Edge）
- 使用“Google 在线”翻译时，识别出的文字会发送到 Google 翻译
- 使用“Chrome 内置”翻译时，翻译在本机完成
- 使用 API 版时，音频会发送到 OpenAI，识别出的文字会发送到你选择的翻译服务（OpenAI 或 Anthropic）

在课堂上使用前，请确认符合学校和老师对录音、转写的规定。

## License

[MIT](LICENSE)
