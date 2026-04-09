---
name: code-ocd
description: |
  代码强迫症：AI 代码风格约束，确保生成的代码符合各语言社区格式化标准。
  覆盖语言：TypeScript/JavaScript、Python、Go、Java、Rust、Shell。
  规则范围：缩进、行宽换行、标点空格、Import 排序、注释规范、命名规范。
  触发词：「代码强迫症」「code ocd」「格式化规范」「代码风格」
  原则：项目已有 formatter 配置优先于本文档。一致性高于任何单条规则。
---

# Code OCD — AI 代码风格约束

> 当项目中已有格式化配置（`.prettierrc`、`rustfmt.toml`、`pyproject.toml`、`.editorconfig` 等）时，**项目配置优先于本文档**。
> 当存疑时，跟随项目现有风格。**一致性高于任何单条规则。**

---

## 注释语言规则

检测项目中已有注释的自然语言（中文/英文/其他），所有新注释使用相同语言。若无已有注释，默认使用英文。

---

## 1. 缩进

| 语言 | 风格 | 宽度 | 续行缩进 |
|---|---|---|---|
| TypeScript / JavaScript | Spaces | 2 | 2 |
| Python | Spaces | 4 | 4（或 hanging） |
| Go | Tabs | 1 tab | 1 tab |
| Java | Spaces | 4 | 8 |
| Rust | Spaces | 4 | 4 |
| Shell (Bash/Zsh) | Spaces | 2 | 4 |

- 同一文件内禁止混用 tabs 和 spaces
- 存在 `.editorconfig` 时以其配置为准
- 嵌套超过 3 层时考虑提取函数

---

## 2. 行宽与换行

### 2.1 行宽限制

| 语言 | 软限制 | 硬限制 |
|---|---|---|
| TypeScript / JavaScript | 80 | 100 |
| Python | 79（PEP 8） | 99（Black） |
| Go | 无（gofmt） | 120 |
| Java | 100（Google） | 120 |
| Rust | 100（rustfmt） | 100 |
| Shell | 80 | 100 |

### 2.2 换行规则

- **运算符**：多数语言在运算符**后**换行；Python 在二元运算符**前**换行（PEP 8 W504）
- **链式调用**：每个 `.method()` 前换行，缩进一级
- **函数参数**：一行放不下时，每个参数独占一行，右括号与左侧关键字对齐
- **三元表达式**：超出行宽时拆成 `if/else` 或将 `?` `:` 放在续行行首
- **import 语句**：超出行宽时使用多行语法（Python 括号导入、JS/TS 多行解构、Rust 嵌套 use）

---

## 3. 空格与标点

### 3.1 分号

| 语言 | 规则 |
|---|---|
| TypeScript / JavaScript | 跟随项目现有风格；无约定时省略分号 |
| Java | 必须 |
| Rust | 语句末尾 `;`，块末尾表达式省略 `;` 以返回值 |
| Go / Python / Shell | 不适用 |

### 3.2 尾逗号

| 语言 | 规则 |
|---|---|
| TypeScript / JavaScript | 多行时始终添加 |
| Python | 多行集合/参数始终添加 |
| Rust | 多行时始终添加 |
| Go | 多行复合字面量必须（编译器要求） |
| Java | 不支持 |

### 3.3 引号

| 语言 | 规则 |
|---|---|
| TypeScript / JavaScript | 单引号（跟随项目）；模板字符串用于插值 |
| Python | 跟随项目（Black 默认双引号）；docstring 始终双引号 |
| Go | 双引号；反引号用于原始/多行字符串 |
| Java | 双引号 |
| Rust | 双引号 |
| Shell | 双引号用于变量展开，单引号用于字面量 |

### 3.4 大括号

- 开括号与语句**同行**（K&R / 1TBS 风格）
- Go：**必须**同行（编译器要求）
- Python：不适用
- 空块：同行 `{}`；Rust 中待实现逻辑使用 `todo!()`

### 3.5 空行

- Python：顶层定义间 **2** 行空行，类内方法间 **1** 行（PEP 8）
- 其他语言：顶层声明间 **1** 行空行
- 函数体内最多 **1** 行连续空行
- 文件末尾以**单个换行符**结束，无多余空行

---

## 4. Import 排序

通用原则：按分组排列，组间空行分隔，组内按字母排序。

### TypeScript / JavaScript

1. Node 内置模块（`node:fs`、`path`）
2. 外部包（`react`、`lodash`）
3. 内部别名 / monorepo 包（`@app/...`、`@shared/...`）
4. 相对父级导入（`../`）
5. 相对同级/子级导入（`./`）
6. 副作用导入（`import './styles.css'`）

### Python（isort 兼容）

1. `__future__` 导入
2. 标准库（`os`、`sys`、`typing`）
3. 第三方库（`requests`、`fastapi`）
4. 本地 / 项目内（`from . import`、`from app import`）

### Go（goimports）

1. 标准库
2. 第三方（非项目域名）
3. 项目内部

### Java（Google 风格）

1. `java.*`
2. `javax.*`
3. 第三方（按字母序）
4. 项目包
- 禁止通配符导入（`*`）
- 静态导入单独分组，置于非静态导入之后

### Rust

1. `std` / `core` / `alloc`
2. 外部 crate
3. `crate::`（当前 crate）
4. `self::` / `super::`
- 尽量使用嵌套 use 组：`use std::{collections::HashMap, io};`

### Shell

- `source` / `.` 语句置于脚本顶部（shebang 和 `set` 选项之后）
- 顺序：系统脚本在前，项目脚本在后

---

## 5. 注释规范

### 5.1 关键变量注释

每个导出的/公开的常量或变量，若命名不足以自解释，必须添加注释。

| 语言 | 格式 |
|---|---|
| TypeScript / JavaScript | `/** JSDoc */` 置于声明上方 |
| Python | `# 注释` 或模块级 docstring |
| Go | `// VarName ...`（godoc 约定） |
| Java | `/** Javadoc */` |
| Rust | `/// doc comment`（支持 Markdown） |
| Shell | `# 注释` 置于变量上方 |

### 5.2 关键逻辑注释

- 注释 **why**（为什么），而非 **what**（做了什么）
- 非显而易见的算法附简要说明或参考链接
- 复杂条件（3+ 个谓词）添加一行意图总结
- 统一使用 `TODO(author):`、`FIXME(author):`、`HACK:` 前缀

### 5.3 文件 / 模块头注释

- 非显而易见用途的文件建议添加头注释
- Python：模块 docstring 置于文件顶部
- Go：package comment 在包的主文件中
- Rust：`//!` 模块级文档注释

### 5.4 不该注释的内容

- 闭合大括号标记（`} // end if` — 删除）
- 显而易见的 getter/setter
- 通过清晰命名已自解释的代码

---

## 6. 命名规范

| 元素 | TS/JS | Python | Go | Java | Rust | Shell |
|---|---|---|---|---|---|---|
| 变量 | `camelCase` | `snake_case` | `camelCase` | `camelCase` | `snake_case` | `snake_case` |
| 函数 | `camelCase` | `snake_case` | `PascalCase`(导出) / `camelCase` | `camelCase` | `snake_case` | `snake_case` |
| 类型/类 | `PascalCase` | `PascalCase` | `PascalCase` | `PascalCase` | `PascalCase` | N/A |
| 常量 | `UPPER_SNAKE` | `UPPER_SNAKE` | `PascalCase` | `UPPER_SNAKE` | `UPPER_SNAKE` | `UPPER_SNAKE` |
| 文件名 | `kebab-case.ts` | `snake_case.py` | `snake_case.go` | `PascalCase.java` | `snake_case.rs` | `kebab-case.sh` |

---

## 7. 格式化细则

- 二元运算符两侧加空格：`a + b`，非 `a+b`
- 冒号前无空格，冒号后加空格（对象/字典字面量）
- 逗号后加空格
- 括号内侧无空格：`foo(a, b)` 非 `foo( a, b )`
- 方括号内侧无空格：`arr[0]` 非 `arr[ 0 ]`
- JS/TS 箭头函数：单参数省略括号 `x => x + 1`，零/多参数加括号
- Python：`if` 条件无括号（`if x > 0:` 非 `if (x > 0):`）
- Go：`if` / `for` 条件无括号
- Rust：`if` / `while` 条件无括号
- Shell：变量始终加双引号 `"$var"`；优先使用 `[[ ]]` 而非 `[ ]`
- 严格模式：
  - Shell：`set -euo pipefail`
  - TypeScript：`tsconfig.json` 中 `"strict": true`
  - Python：鼓励类型注解

---

## 诚实边界

**这个 Skill 能做的：**
- 约束 AI 生成代码的缩进、换行、标点、Import 顺序、注释、命名风格
- 跨 6 种主流语言保持一致的规范意识
- 自动适配项目已有的注释语言和格式化配置

**做不到的：**

| 维度 | 说明 |
|------|------|
| 替代 Linter/Formatter | 本文档是 AI 提示词级别的约束，不能替代 ESLint、Prettier、Black、rustfmt 等工具的实际运行 |
| 覆盖所有语言 | 仅覆盖 TS/JS、Python、Go、Java、Rust、Shell；其他语言需自行扩展 |
| 团队共识 | 具体项目的风格偏好（如分号/引号）仍需团队自行约定，本文档提供合理默认值 |
| 架构设计 | 仅关注代码格式层面，不涉及架构模式、设计原则等更高层次的决策 |

---

## 规则来源

| 语言 | 参考标准 |
|------|---------|
| TypeScript / JavaScript | Prettier defaults, ESLint recommended |
| Python | PEP 8, Black, isort |
| Go | gofmt, goimports, Effective Go |
| Java | Google Java Style Guide |
| Rust | rustfmt, Rust API Guidelines |
| Shell | Google Shell Style Guide |
