# Kinema TDD Injector

一次性注入器，为目标仓库生成定制版 `CLAUDE.md` 记忆文件，植入 kinema 的 TDD 方法论。

## 功能

- 三阶测试体系（unit / dev-integration / e2e-integration）
- 分层 conftest 架构
- 网络/IO 边界规范
- 命名规则（测试路径三层命名）
- 覆盖率门槛配置
- Fixture 治理规则
- Commit message 规范

## 适用场景

- 在新仓库初始化 TDD 规范
- 把测试标准注入/导入到另一个仓库
- 在正式开发前确立测试约定

## 使用方式

本 skill 为 OpenClaw/Claude Code 技能，安装后可通过对话触发：

```
把测试规范注入到这个仓库
init tdd standard here
set up testing methodology
import kinema's test rules
```

## 文件结构

```
kinema-tdd-injector/
├── SKILL.md              # Skill 定义文件
├── assets/
│   └── claude_md.j2      # Jinja2 模板
├── scripts/
│   └── render.py         # 渲染脚本
└── evals/
    └── evals.json        # 评估配置
```

## 作者

- **Author**: [LeeShunEE](https://github.com/LeeShunEE)
- **Organization**: [KinemaClawWorkspace](https://github.com/KinemaClawWorkspace)

## 许可证

MIT