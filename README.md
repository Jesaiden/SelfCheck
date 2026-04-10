[README.md](https://github.com/user-attachments/files/26633863/README.md)
# SelfCheck - 应用测试自检总控 Skill

<p align="center">
  <strong>让 AI 代理做测试自检时，不再"想当然"</strong>
</p>

---

## 什么是 SelfCheck？

SelfCheck 是一套 **证据驱动的应用自检框架**，专为 AI 代理（如 Codex、Cursor Agent 等）设计。它将"看代码 → 跑测试 → 收集证据 → 逐条判定 → 输出结论"固化为一套稳定流程，确保 AI 给出的测试结论**可追溯、可复核、可信赖**。

### 解决的问题

| 问题 | SelfCheck 的做法 |
|------|------------------|
| 只看代码就说"应该能跑" | 强制要求真实执行证据 |
| 没有证据也写 PASS | 证据分级：A/B/C 级，C 级不能支撑 PASS |
| 发现问题不给修复建议 | 必须输出复测清单 + 人工补测清单 |
| P0 没闭环却误判可以上线 | P0 未闭环 = 总门禁不通过 |

## 特性

- **项目类型无关** — 支持微信小程序、移动端 App、Web 应用、多端项目等
- **双模式运行** — 可执行模式 / 静态模式自动适配
- **证据分级体系** — A(强) / B(中) / C(弱) 三级证据
- **标准化门禁** — PASS / FAIL / BLOCKED / NEEDS_HUMAN_CHECK 四态判定
- **即插即用提示词** — 可直接粘贴给任何 AI 代理使用
- **结构化输出** — Markdown 报告 + CSV 证据矩阵 + JSON 摘要

## 目录结构

```
selfcheck/
├── SKILL.md                        # 主 Skill 框架（核心指令集）
├── TEST_GATE.md                    # 统一门禁规则
├── RUNBOOK.md                      # 代理执行手册
├── project-config.yaml.example     # 项目配置模板
│
├── guides/                         # 详细规则文档
│   ├── RUNBOOK.md                  # 执行手册详解
│   ├── EVIDENCE_RULES.md           # 证据分级细则
│   ├── CASE_STATUS_RULES.md        # 状态判定细则
│   └── REPORT_SCHEMA.md            # 标准报告结构
│
├── templates/                      # 按项目类型的检查模板
│   ├── template-weapp.md           # 微信小程序模板
│   ├── template-mobile-app.md      # 移动端 App 模板
│   ├── template-web-app.md         # Web 应用模板
│   └── template-multi-platform.md  # 多端项目模板
│
├── prompts/                        # 即用提示词（可直接贴给代理）
│   ├── kickoff-prompt.md           # 启动自检提示词
│   ├── missing-evidence-prompt.md  # 缺失证据追问提示词
│   ├── retest-prompt.md            # 复测提示词
│   └── human-check-handoff-prompt.md  # 人工交接提示词
│
├── project-presets/                # 项目预设示例
│   └── training-miniapp/           # 训练计划小程序预设
│       ├── project-config.yaml
│       └── checklist.md
│
└── examples/                       # 输出样例
    ├── example-self-check-report.md  # Markdown 报告示例
    ├── example-evidence-matrix.csv   # CSV 证据矩阵示例
    └── example-summary.json          # JSON 摘要示例
```

## 快速开始

### 方式一：通用项目

1. 复制配置模板：
```bash
cp project-config.yaml.example project-config.yaml
```

2. 编辑 `project-config.yaml`，填写你的项目信息：
```yaml
project:
  name: your-project-name
  type: weapp          # weapp | mobile_app | web_app | multi_platform
  stage: pre-release-self-check
```

3. 选择对应的模板文件（如 `templates/template-weapp.md`）

4. 将以下内容喂给 AI 代理：
- `SKILL.md` + `TEST_GATE.md` + 对应模板 + 你的 `project-config.yaml`

5. 要求代理按 `RUNBOOK.md` 执行自检

### 方式二：直接使用预设

如果你有类似的项目，可以直接参考或使用已有的项目预设：

```bash
# 查看 训练计划小程序 预设
cat project-presets/training-miniapp/project-config.yaml
cat project-presets/training-miniapp/checklist.md
```

配合 `prompts/kickoff-prompt.md` 一并交给代理即可。

## 工作流程

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  阶段1      │    │  阶段2      │    │  阶段3      │
│  识别上下文  │───▶│  建立地图    │───▶│  逐条处理    │
│             │    │  (模块/页面/ │    │  关键用例    │
│ 项目类型     │    │  数据/P0)   │    │             │
│ 当前阶段     │    │             │    │ 验证目标     │
│ 执行能力     │    │             │    │ 证据等级     │
└─────────────┘    └─────────────┘    └─────────────┘
                                              │
┌─────────────┐    ┌─────────────┐            ▼
│  阶段6      │◀───│  阶段5      │    ┌─────────────┐
│  总门禁判定  │    │  逐条输出    │    │  阶段4      │
│             │    │  判定结果    │    │  执行与取证  │
│ PASS        │    │             │    │             │
│ FAIL        │    │ 用例编号     │    │ 构建验证     │
│ BLOCKED     │    │ 模块/优先级  │    │ 自动化测试   │
│ NEEDS_...   │    │ 证据/状态    │    │ 日志截图归档 │
└─────────────┘    └─────────────┘    └─────────────┘
```

## 证据分级体系

| 等级 | 说明 | 示例 |
|------|------|------|
| **A 级（强）** | 真实执行证据 | 自动化通过、构建成功、真机截图、闭环日志 |
| **B 级（中）** | 部分运行证据 | 历史报告、局部验证、半闭环结果 |
| **C 级（弱）** | 静态推断 | 代码审查、页面存在、口头说明 |

**核心规则**：
- 单独 C 级证据 **不能** 支撑 PASS
- P0 用例原则上至少需要 A 级或强 B 级组合证据
- 主流程闭环 **必须** 要求 A 级证据

## 门禁状态定义

| 状态 | 含义 | 判定条件 |
|------|------|----------|
| **PASS** | 通过 | 全部 P0 通过 + 主流程闭环 + 无阻断项 + 证据链完整 |
| **FAIL** | 不通过 | 任一 P0 失败 / 主流程断裂 / 数据丢失或崩溃 |
| **BLOCKED** | 阻塞 | 缺少关键环境 / 权限 / 依赖导致无法完成验证 |
| **NEEDS_HUMAN_CHECK** | 需人工确认 | 视觉体验 / 真机手感 / 兼容性等需人工兜底 |

## 使用提示词

`prompts/` 目录下的文件可以按场景直接粘贴给 AI 代理：

| 场景 | 提示词文件 | 说明 |
|------|-----------|------|
| 启动自检 | `kickoff-prompt.md` | 首轮自检的完整指令 |
| 证据不足时 | `missing-evidence-prompt.md` | 追问缺失证据的补充指令 |
| 修复后复测 | `retest-prompt.md` | 复测时的规范指令 |
| 移交人工 | `human-check-handoff-prompt.md` | 将剩余项转给人工测试 |

## 核心原则

1. **没有真实证据，不得默认 PASS**
2. **静态推断只能支持"可疑通过"或"待验证"，不能替代真实测试**
3. **P0 未闭环，不允许输出总 PASS**
4. **代理必须逐条说明：验证方式、证据、状态、风险、下一步动作**

## 适用对象

SelfCheck 不绑定特定 AI 模型或平台。只要代理具备以下能力即可接入：

- ✅ 读取文档与配置
- ✅ 理解需求、测试用例、代码结构
- ✅ 运行命令或读取已有测试结果
- ✅ 输出结构化结论

已验证兼容：Codex、Cursor Agent、ChatGPT、Claude、其他具备代码能力的 LLM 代理

## License

MIT License

---

<p align="center">
  <sub>Made with 🔧 for reliable AI-powered testing</sub>
</p>
