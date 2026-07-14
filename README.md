# 演讲培训技能 (speech-training)

一个辅助演讲者生成演讲和培训材料的 Claude Skill。从主题问诊、名人名言核实、逐字稿、演示脚本到 HTML PPT，提供完整工作流。

内容由本 skill 负责，视觉由 [frontend-slides](https://github.com/zarazhangrui/frontend-slides) 负责，两者通过"交接包"衔接：本 skill 收集完信息后直接跳过 frontend-slides 的提问阶段，用户不用回答两遍同样的问题。

## 功能特点

- **完整工作流**：快速模式判断 → 动态问诊 → 名人名言联网核实 → 逐字稿 → 演示脚本 → HTML PPT → 分发
- **快速模式**：首条消息已含主题+受众+时长时，跳过详细问诊，一次确认直接开工
- **断点续传**：进度保存到 `{日期}培训素材/.progress.json`，中断后可恢复
- **受众风格映射**：技术(干货型)、业务(故事型)、园区(轻松型)、高管(精炼型)，逐字稿语气和 PPT 风格随受众切换
- **联网核实名言**：通过 `WebSearch` + `WebFetch` 核对原话、头衔、出处，核查不过直接弃用，禁止幻觉引用
- **frontend-slides 深度对接**：交接包传递 Purpose/Length/Content/Density，复用其风格预览、固定 16:9 stage、Vercel 部署、PDF 导出全链路
- **降级方案**：`frontend-slides` 未安装时，生成 Markdown 大纲 + 基础 HTML PPT，并标记 `DONE_WITH_CONCERNS`
- **完成状态协议**：`DONE` / `DONE_WITH_CONCERNS` / `BLOCKED`，结束时明确交代产出质量

## 核心产出物

| 产出物 | 文件命名规范 | 说明 |
|--------|-------------|------|
| 演讲逐字稿 | `{日期}培训素材/演讲逐字稿.md` | 含章节结构与 Timing 标注 |
| 演示指令脚本 | `{日期}培训素材/演示指令脚本.md` | 现场演示分步指令，含降级预案 |
| HTML PPT | `{标题}-{日期}.html` | frontend-slides 生成的独立幻灯片 |
| PPT 降级大纲 | `{日期}培训素材/PPT-大纲.md` | `frontend-slides` 缺失时的 Markdown 版 |

## 安装与使用

### 前置依赖（强烈推荐）

PPT 生成依赖 [frontend-slides](https://github.com/zarazhangrui/frontend-slides) skill。未安装时会降级为基础版 HTML，效果差不少，建议先装：

```bash
git clone https://github.com/zarazhangrui/frontend-slides.git
cp -r frontend-slides ~/.claude/skills/
```

或在 Claude Code 中用 `/skill-install` 输入 `zarazhangrui/frontend-slides`。

### 安装本 skill

#### 方式一：全局安装（推荐）

```bash
# 1. 克隆仓库
git clone https://github.com/Viy1204/speech-training-skill.git

# 2. 复制到 Claude Code skills 目录
cp -r speech-training-skill/speech-training ~/.claude/skills/

# 3. 重启 Claude Code 即可使用
```

#### 方式二：通过 skill-install 安装

1. 在 Claude Code 中输入 `/skill-install`
2. 输入：`Viy1204/speech-training-skill`
3. 完成安装

### 更新到最新版

如果你已经安装过，想同步 GitHub 上的最新改动，直接运行：

```bash
curl -sL -o ~/.claude/skills/speech-training/SKILL.md \
  https://raw.githubusercontent.com/Viy1204/speech-training-skill/main/speech-training/SKILL.md
```

更新后重启 Claude Code 即可生效。

### 使用方法

安装完成后，**在 Claude Code 对话框中直接输入你的需求**，例如：

- "帮我做演讲"
- "准备培训材料"
- "写逐字稿"
- "做演讲用的大纲"

Claude 会引导你完成：

1. **Step 0（快速模式 + 断点检测）**：信息充分则跳过问诊；有未完成进度则询问是否续传
2. **Step 1（动态信息收集）**：两批结构化提问 + 按受众类型智能追问
3. **Step 2（名人名言核实）**：联网搜索真实出处，核查不过直接弃用
4. **Step 3（演讲逐字稿）**：章节按时长比例分配，风格随受众切换
5. **Step 4（演示指令脚本）**：分步指令 + 降级预案
6. **Step 5（HTML PPT）**：交接包传给 `frontend-slides`，从风格预览开始；缺失时自动降级
7. **Step 6（分发）**：部署 Vercel URL / 导出 PDF / base64 嵌入版单文件

## 依赖关系

| Step | 依赖 | 说明 |
|------|------|------|
| Step 0-4 | 无（Step 2 需联网） | 名言核实用 WebSearch/WebFetch |
| Step 5-6 | `frontend-slides`（可选） | 安装后获得风格预览、动画、部署、PDF 导出；未安装自动降级 |

## 目录结构

```
speech-training-skill/
├── LICENSE              # MIT 开源协议
├── README.md            # 本文件
└── speech-training/
    └── SKILL.md         # 完整的 Skill 定义文件 (v1.2.0)
```

## 完成状态说明

工作流结束时会报告以下状态之一：

- **DONE** — 所有产出物生成完毕，名言已核查，素材就位。
- **DONE_WITH_CONCERNS** — 主要产物已生成，但存在已知问题（如缺少图片、`frontend-slides` 降级等），会逐条列出。
- **BLOCKED** — 因缺少必填信息或不可恢复的依赖缺失，无法继续。

## 常见问题

**Q: 安装后找不到 skill？**
A: 重启 Claude Code，并确认 skills 目录结构为：`~/.claude/skills/speech-training/SKILL.md`

**Q: 生成 PPT 时报错没有 frontend-slides？**
A: 会自动降级：生成 Markdown 大纲 + 基础 HTML PPT，不影响使用。但推荐安装 frontend-slides 获得完整效果。

**Q: 对话中途中断了怎么办？**
A: 重新触发 skill，它会检测 `{日期}培训素材/.progress.json` 并询问是否继续上次进度。

**Q: 为什么不让 speech-training 自己生成 PPT，非要依赖 frontend-slides？**
A: 分工明确：本 skill 专注内容质量（问诊、核实、逐字稿），frontend-slides 专注视觉工程（固定 16:9 stage、风格模板库、部署链路）。重复造轮子只会两头都做不好。

**Q: 可以修改这个 skill 吗？**
A: 可以！MIT 协议允许自由使用、修改和分发。欢迎 Fork 和 PR。
