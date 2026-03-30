# 演讲培训技能

一个辅助演讲者生成演讲和培训材料的 Claude Skill。

## 功能特点

- **完整工作流**：从主题大纲 → 名人名言筛选 → 演示脚本 → 逐字稿 → HTML PPT
- **口语化写作**：指导生成接地气、适合演讲的表达方式
- **结构化产出**：规范的文件命名和组织结构

## 核心产出物

| 产出物 | 说明 |
|--------|------|
| 演讲逐字稿 | 含章节结构的完整演讲内容 |
| 演示指令脚本 | 现场演示的分步指令 |
| HTML PPT | 独立运行的幻灯片 |

---

## 安装与使用

### 前置依赖

⚠️ **重要**：Step 5 生成 HTML PPT 需要用到 `frontend-slides` skill，**需提前安装**：

```bash
# 安装 frontend-slides skill（由 @zarazhangrui 提供）
/skill-install
# 输入：zarazhangrui/frontend-slides
```

安装完 `frontend-slides` 后，再安装本 skill。

### 安装方式

#### 方式一：安装到全局（推荐，一次安装长期使用）

1. 找到你的 Claude Code skills 目录：
   ```bash
   # 通常在用户目录下
   ~/.claude/skills/
   ```

2. 将 `speech-training` 文件夹复制到 skills 目录：
   ```bash
   cp -r speech-training ~/.claude/skills/
   ```

3. 重启 Claude Code，新 skill 即可使用

#### 方式二：通过 /install-github-app 安装（推荐）

Claude Code 支持直接从 GitHub 安装 skill：

1. 在 Claude Code 中输入 `/install-github-app`
2. 粘贴仓库地址：`https://github.com/Viy1204/speech-training-skill`
3. 按照提示完成安装

#### 方式三：通过 skill-install 安装

1. 在 Claude Code 中输入 `/skill-install`
2. 输入 GitHub 仓库地址：`Viy1204/speech-training-skill`
3. 完成安装

### 使用方法

安装完成后，**在 Claude Code 对话框中直接输入你的需求**，例如：

- "帮我做演讲"
- "准备培训材料"
- "生成 PPT"
- "做演讲用的大纲"

Claude 会按照工作流引导你完成所有材料的制作，包括：
1. 收集演讲主题、受众、时长等信息
2. 筛选相关名人名言
3. 生成演讲逐字稿
4. 制作演示指令脚本
5. 生成 HTML 格式 PPT（依赖 frontend-slides skill）

---

## 目录结构

```
speech-training-skill/
├── LICENSE          # MIT 开源协议
├── README.md        # 本文件
└── speech-training/
    └── SKILL.md     # 完整的 Skill 定义文件
```

## 依赖关系

| Step | 依赖 |
|------|------|
| Step 1-4 | 无额外依赖 |
| Step 5 (HTML PPT) | 需要提前安装 `frontend-slides` skill |

frontend-slides 仓库：https://github.com/zarazhangrui/frontend-slides

## 常见问题

**Q: 安装后找不到 skill？**
A: 重启 Claude Code，确保 skills 目录结构正确：`~/.claude/skills/speech-training/SKILL.md`

**Q: 生成 PPT 时报错？**
A: 检查是否已安装 `frontend-slides` skill。PPT 生成功能依赖该 skill。

**Q: 可以修改这个 skill 吗？**
A: 可以！MIT 协议允许自由使用、修改和分发。
