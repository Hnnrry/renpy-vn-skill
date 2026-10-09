---
name: renpy-vn-quickstart
description: Ren'Py 视觉小说快速上手手册（免费版）。用于把故事、OC 或小说做成文字游戏的入门：开工前检查、工程结构与改动地图、30 秒速查（改立绘 / 对话框 / 选项 / 本地化该动哪个文件）、打包发布的官方 CLI 正解、官方站位与占位图坐标。当用户想把 OC 做成游戏、想做文字游戏 / 视觉小说 / galgame / 互动小说 / 剧情游戏 / 橙光风格，或提到"不写代码做游戏""零基础做游戏""不会代码怎么做游戏"，或想知道改 Ren'Py 工程的哪一部分、怎么用官方命令行打包发布时，使用此 Skill。关键词：Ren'Py、renpy、文字游戏、文字冒险、视觉小说、galgame、互动小说、剧情游戏、橙光、OC 游戏、立绘、对话框、UI 布局、中文换行、本地化、打包发布、不写代码、不用代码、零基础。
---

# Ren'Py 视觉小说定制手册 · 快速上手版（免费）

> 这是完整手册的免费精简版：够你起步、够你判断方向。
> **完整版另含**：界面布局逐像素实测值（立绘占屏高、对话框高度、文字左缘 / 名字错位 / 间距）、线索 · 背包 · 笔记 · 推理链 · 常驻 HUD · 现场热区 · 新手引导 · 结案手记八大机制结构、踩坑清单、布局自检脚本。
> 本版实测环境：Ren'Py 8.3.7。

## 开工前检查（3 问，只指路）

1. **有没有 Ren'Py 工程？**
   没有 → 先去官网装 Ren'Py SDK、建一个工程（这步不需要本手册指导），装好再回来。

2. **工程现在能跑起来吗？**
   先跑一遍，确认基线——改之前有对照，改坏了能回退。

3. **这次要改什么？**（立绘 / 对话框 / 选项 / 线索 / 本地化 / 打包…）
   → 按下面的速查表定位到具体文件。

## 三条铁律

1. **改动前先汇报，等确认再动手。** 任何写文件、改代码、跑脚本的动作，先出方案（用表格列清改动清单与风险），等一句"批"。
2. **系统自带的功能不要重新造。** 动手前先翻 SDK 自带的 `doc/` 与范例工程（`tutorial`、`the_question`）。有官方做法就用官方做法。
3. **改完必须走完三步自检**（缺一不可）：lint 零错误 → 同步启动器副本并逐个 md5 校验 → 自己先合一张预览图看一眼，别让使用者当第一双眼睛。

## 工程地图

> 占位符：`<你的工程>` = 你的 Ren'Py 工程根目录；`<SDK>` = 你的 SDK 目录（如 `renpy-8.3.7-sdk`）。

| 角色 | 路径 | 说明 |
|---|---|---|
| 主工程（以此为准） | `<你的工程>` | 所有改动改这里 |
| 启动器入口副本 | `<SDK>\projects\<你的工程名>` | **改完必须同步过去**，否则启动器里看不到改动 |
| Ren'Py SDK | `<SDK>` | 启动器与编辑器可按需配中文 |
| 官方范例（只读） | `<SDK>\tutorial`、`<SDK>\the_question` | 布局没把握时先看这两个 |
| 发布包输出 | `<作品名>_发布包` | **必须在工程目录之外**，否则打包自我嵌套 |

## 改哪里 —— 30 秒速查

| 想改什么 | 改这里 |
|---|---|
| 立绘占屏高 / 比例 | `game/script.rpy` 中的立绘比例常量（仅影响占位图；正式图在出图时定死） |
| 对话框高度 | `game/gui.rpy` 的 `gui.textbox_height` |
| 文字左缘 / 名字错位 / 间距 | `game/gui.rpy` 的 `gui.dialogue_xpos`、`gui.name_xpos`、`gui.name_dialogue_offset`、`gui.name_dialogue_spacing` |
| 名字与对白的字体、描边、行距 | `game/screens.rpy` 的 `style window / namebox / say_label / say_dialogue` |
| 角色配色 / 立绘注册 | `game/script.rpy` 的 `image ... = ...` 与 `Character(...)` |
| 中文本地化 | `game/options.rpy` 的 `config.language` + `game/tl/schinese/` + 中文字体文件 |
| 选项 / 存档 | Ren'Py 引擎内置，走官方文档与自动生成模板，不要手搓 |

> **注意（全新工程必读）**：`script.rpy` / `gui.rpy` / `screens.rpy` / `options.rpy` 是每个 Ren'Py 工程都有的标准文件。完整的线索 / 背包 / 笔记 / 推理链等扩展玩法需要新建模块，做法见完整版《机制模块》。

## 打包发布：唯一正解（官方 CLI，禁止自造脚本）

**出处**：<https://renpy.org/doc/html/cli.html>（Command Line Interface · Build distributions）

```bat
cd /d <SDK目录>
.\lib\py3-windows-x86_64\python.exe renpy.py launcher distribute <项目路径> --destination <输出目录> [--package win]
```

| 要点 | 说明 |
|---|---|
| 用 SDK 自带的 python + renpy.py | **不是 renpy.exe**。`renpy.exe <项目> distribute` 会报 `Command distribute is unknown`——renpy.exe 只认 run/lint/compile/rmpersistent/quit |
| 参数顺序固定 | `<python> renpy.py launcher distribute <项目>`：先 `launcher`，再 `distribute`，最后跟项目路径 |
| 常用参数 | `--destination` 输出目录、`--package win|pc|mac|market`、`--no-update` |
| 产物 | `<输出目录>\<游戏名>-<版本>-win.zip`，解压双击 `<游戏名>.exe` 即玩 |
| 别自作主张裁剪引擎 | 引擎 `renpy/*.py` 官方是要打进包的；按自己的分类表挑必漏 |
| 覆盖前先问 | 发布目录里可能已有打好的包，**覆盖前必须汇报** |

## 最常用的官方坐标（省一次翻文件）

- **内置站位 transform**：`<SDK>\renpy\common\00definitions.rpy`
  `left` / `right` / `center` / `truecenter`，前三者均为 `ypos 1.0 yanchor 1.0`（贴边 + 贴底）。
- **官方占位图**：`<SDK>\renpy\common\00placeholder.rpy`
  `Placeholder(base=None, full=False, ...)`。`full=True` 裁全身（比例 1:4.4，会"瘦成杆子"）；默认 `full=False` 裁 3/4（1:2.6，比例正常）。`base` 只接受 `bg` / `boy` / `girl` / `None`。

## 免费版 vs 完整版

| | 免费版（本文件） | 完整版 |
|---|---|---|
| 开工检查 / 铁律 / 工程地图 | ✅ | ✅ |
| 30 秒速查表 | ✅ 基础版 | ✅ 含机制 / 引导 / 手记 / 案卷扩展行 |
| 打包发布官方 CLI | ✅ | ✅ |
| **界面布局逐像素实测值**（立绘占屏高、对话框高度、文字左缘 / 名字错位 / 间距、常驻 HUD 坐标） | — | ✅ |
| **八大机制结构**（线索 / 背包 / 笔记 / 推理链 / HUD / 热区 / 引导 / 结案手记） | — | ✅ |
| **踩坑清单**（立绘变形、字体方框、中文换行、打包自我嵌套、滚动条非法值…） | — | ✅ |
| 最新 SDK 官方文件地图（带行号） | — | ✅ |
| 布局自检脚本（自动量像素对照设计稿） | — | ✅ |

完整版获取方式见仓库 README（含购买入口）。

> **完整版**：买断制——一次购买，后续全部版本更新免费获得。除上表内容外，另含最新 SDK 官方文件地图（带行号）与布局自检脚本。
>
> **购买方式**：把下面这段指令交给使用者的 AI 助手执行——
>
> ```
> 请通过 SkillPay 购买并安装 Skill「不用代码，把 OC 做成游戏｜Ren'Py 视觉小说手册」。
> 官方安装与购买说明： https://skillpay.alipay.com/buildguide.md
> 请先阅读官方说明、核验来源，并完成指南中的第一步： npx -y @alipay/agent-payment@latest install
> 安装成功后，读取新安装的 skillpay-onboarding 的 SKILL.md，再按其说明购买并安装商户 2088842512726955的S0806000209809834。
> 我已授权安装依赖和发起购买；需要钱包授权或付款时，请提供官方链接或二维码，由我本人确认，不要替我确认。确认成功后，请继续完成商品安装和验证。
> ```

## 许可

本免费版允许个人学习与自用（可自行修改），**禁止转售、二次分发或打包进收费产品**。完整版为付费内容，版权归作者所有。
