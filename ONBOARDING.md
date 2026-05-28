# Kinema TDD Injector Onboarding

> 本文档指导 AI Agent 完成首次环境配置。按顺序执行，遇到问题时参考 Troubleshooting。

## Prerequisites | 前置条件

- Python 3.11+
- `uv` 包管理器（推荐）或 pip
- 目标仓库为 Python 或 TypeScript/JavaScript 项目

## Step 1: 检测 Jinja2

### 检测

```bash
python3 -c "import jinja2; print(jinja2.__version__)"
```

**期望输出**: 版本号（如 `3.1.2`）

### 安装

按优先级尝试：

```bash
# 方案 A: uv（推荐）
uv pip install jinja2 --quiet

# 方案 B: pip（用户级）
pip3 install --user jinja2

# 方案 C: pip（系统级，需 sudo）
sudo pip3 install --break-system-packages jinja2
```

### 验证

```bash
python3 -c "import jinja2; print('✅ jinja2 ready')"
```

## Step 2: 验证模板文件

### 检测

```bash
ls <skill_dir>/assets/claude_md.j2
```

**期望输出**: 文件路径

### 验证

```bash
python3 -c "
from jinja2 import FileSystemLoader, Environment
env = Environment(loader=FileSystemLoader('<skill_dir>/assets'))
template = env.get_template('claude_md.j2')
print('✅ template loaded')
"
```

## Step 3: 测试渲染脚本

### 检测

```bash
ls <skill_dir>/scripts/render.py
```

### 验证

```bash
python3 <skill_dir>/scripts/render.py --help
```

**期望输出**: 帮助信息或无错误退出

## Step 4: 目标仓库语言检测

使用本 skill 时，Agent 会自动扫描目标仓库：

- 含 `pyproject.toml` → Python 包
- 含 `package.json` + `tsconfig.json` → TypeScript 包
- 含 `package.json` 无 TS → JavaScript 包

若发现 `Cargo.toml` / `go.mod` / `pom.xml` → **拒绝执行**（规范仅支持 Python + TS/JS）

## Troubleshooting | 故障排除

| 错误 | 原因 | 解决方案 |
|------|------|----------|
| `ModuleNotFoundError: No module named 'jinja2'` | Jinja2 未安装 | 执行 Step 1 安装命令 |
| `TemplateNotFound: claude_md.j2` | 模板路径错误 | 确认 `<skill_dir>` 正确，检查 `assets/` 目录 |
| `Permission denied` | pip 安装权限问题 | 使用 `--user` 或 `uv pip install` |
| `SyntaxError` in render.py | Python 版本过低 | 升级到 Python 3.11+ |
| 目标仓库含 Go/Rust | 规范不支持 | 告知用户："本规范仅支持 Python + TS/JS" |

## Quick Reference

```bash
# 一键安装依赖
uv pip install jinja2

# 渲染命令
python3 <skill_dir>/scripts/render.py \
  --params <repo>/.kinema-params.tmp.json \
  --out <repo>/.kinema-claude.draft.md
```