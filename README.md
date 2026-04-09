<div align="center">

# 强迫症.skill

> *「当存疑时，跟随项目现有风格。一致性高于任何单条规则。」*

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai/code)
[![Skills](https://img.shields.io/badge/skills.sh-Compatible-green)](https://skills.sh)

<br>

**让 AI 生成的代码符合各语言社区的格式化标准。**

<br>

6 种主流语言，7 大规则维度，一个文件搞定。<br>
不是写给人看的 lint 规则，是写给 AI 的代码风格约束。

[看效果](#效果示例) · [安装](#安装) · [它约束了什么](#它约束了什么) · [诚实边界](#诚实边界)

</div>

---

## 效果示例

```
用户      ❯ 帮我写一个 Python 函数，读取 CSV 并按日期分组

AI（无 Skill）❯
  import csv
  from collections import defaultdict
  def group_by_date(file):
      result = defaultdict(list)
      with open(file) as f:
          reader = csv.DictReader(f)
          for row in reader:
              result[row['date']].append(row)
      return result

AI（有 Skill）❯
  from __future__ import annotations

  import csv
  from collections import defaultdict
  from pathlib import Path

  def group_by_date(filepath: str | Path) -> dict[str, list[dict]]:
      """Read a CSV file and group rows by the 'date' column."""
      result: defaultdict[str, list[dict]] = defaultdict(list)

      with open(filepath, encoding="utf-8") as f:
          reader = csv.DictReader(f)
          for row in reader:
              # Use date column as grouping key; rows without date are skipped
              if date := row.get("date"):
                  result[date].append(row)

      return dict(result)
```

区别：导入排序（标准库 → 第三方）、类型注解、docstring、关键逻辑注释、命名风格、引号统一、空行规范。

---

## 安装

```bash
npx skills add shenzhihao/强迫症-skill
```

然后在 Claude Code 里触发：

```
> 代码强迫症
> code ocd
> 格式化规范
> 代码风格
```

激活后正常写代码，AI 会自动遵循规范。

### 手动安装

将 `SKILL.md` 复制到项目的 `.claude/skills/` 目录：

```bash
mkdir -p your-project/.claude/skills
cp SKILL.md your-project/.claude/skills/code-ocd.md
```

### 通用 AI 工具

将 `SKILL.md` 内容复制到任意 AI 工具的 System Prompt / Custom Instructions 中：

- ChatGPT → Custom Instructions
- Cursor → Rules for AI
- GitHub Copilot → .github/copilot-instructions.md
- 其他 → System Prompt

---

## 它约束了什么

7 个维度，全部基于各语言社区的官方/主流规范：

| 维度 | 一句话 |
|------|--------|
| **缩进** | TS/JS 2 空格、Python 4 空格、Go tabs、Java 4 空格、Rust 4 空格、Shell 2 空格 |
| **行宽与换行** | 各语言软硬限制 + 运算符/链式调用/函数参数的换行位置 |
| **空格与标点** | 分号、尾逗号、引号风格、大括号位置、空行规则 |
| **Import 排序** | 每种语言的分组与排列顺序（isort / goimports / Google Style 等） |
| **注释规范** | 注释 why 而非 what；自动检测项目注释语言；各语言 doc comment 格式 |
| **命名规范** | 变量/函数/类型/常量/文件名的命名风格速查表 |
| **格式化细则** | 运算符空格、括号、箭头函数、条件语句、严格模式等微规则 |

规则来源：

| 语言 | 参考标准 |
|------|---------|
| TypeScript / JavaScript | Prettier defaults, ESLint recommended |
| Python | PEP 8, Black, isort |
| Go | gofmt, goimports, Effective Go |
| Java | Google Java Style Guide |
| Rust | rustfmt, Rust API Guidelines |
| Shell | Google Shell Style Guide |

---

## 诚实边界

**这个 Skill 能做的：**
- 约束 AI 生成代码的格式、注释和组织风格
- 跨 6 种主流语言保持一致的规范意识
- 自动适配项目已有的注释语言和格式化配置

**做不到的：**

| 维度 | 说明 |
|------|------|
| 替代 Linter/Formatter | 这是 AI 提示词级别的约束，不能替代 ESLint、Black、rustfmt 等工具的实际运行 |
| 覆盖所有语言 | 仅覆盖 TS/JS、Python、Go、Java、Rust、Shell；其他语言需自行扩展 |
| 团队共识 | 具体风格偏好（分号/引号等）仍需团队约定，本 Skill 提供合理默认值 |
| 架构设计 | 仅关注代码格式层面，不涉及架构、设计模式等 |

**一个不告诉你局限在哪的 Skill，不值得信任。**

---

## 仓库结构

```
强迫症.skill/
├── SKILL.md        # 核心规范文件（AI 直接读这个）
├── README.md       # 本文件
└── LICENSE         # MIT 许可证
```

---

## 许可证

MIT — 随便用，随便改，随便造。

---

<div align="center">

**Linter** 告诉你哪里错了。<br>
**强迫症.skill** 让 AI 从一开始就写对。<br><br>
*一致性高于任何单条规则。*

</div>
