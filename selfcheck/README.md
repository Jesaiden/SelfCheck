# 应用测试自检总控 Skill（v2）

这是一个面向 **任意代理 / 任意应用项目** 的可复用自检框架。  
它不绑定某个模型，也不要求必须使用 Codex。只要代理具备以下能力，就可以接入：

- 读取文档与配置
- 理解需求、测试用例、代码结构
- 运行命令或读取已有测试结果
- 输出结构化结论

## 这套框架解决什么问题
很多代理会出现以下问题：
- 只看代码就主观说“应该能跑”
- 只写总结，不逐条判定用例
- 没有执行证据也直接写 PASS
- 发现缺口但不给出补测动作
- P0 没闭环却误判可以上线

这个 Skill 的作用，就是把“**证据驱动的应用自检**”固化成一套稳定流程。

## v2 新增内容
相较于初版，v2 增强了以下部分：
1. **更完整的主 Skill 指令**：增加阶段化流程、证据优先级、复测规则。
2. **标准运行手册**：`guides/RUNBOOK.md`
3. **证据与状态判定细则**：`guides/EVIDENCE_RULES.md`、`guides/CASE_STATUS_RULES.md`
4. **标准报告结构**：`guides/REPORT_SCHEMA.md`
5. **可直接粘贴给其他代理的提示词**：`prompts/`
6. **项目预设**：`project-presets/training-miniapp/`，可直接用于你的训练计划小程序
7. **示例 JSON / CSV / Markdown 输出**

## 建议用法
### 方式一：通用项目
1. 复制 `project-config.yaml.example` 为 `project-config.yaml`
2. 选择对应模板：微信小程序 / 移动端 App / Web
3. 将 PRD、测试用例、仓库路径、测试命令写入配置
4. 把 `SKILL.md` + `TEST_GATE.md` + 对应模板 + 配置 一起喂给代理
5. 要求代理按 `guides/RUNBOOK.md` 执行

### 方式二：直接用于你的训练计划小程序
直接使用：
- `project-presets/training-miniapp/project-config.yaml`
- `project-presets/training-miniapp/checklist.md`
- `prompts/kickoff-prompt.md`

## 目录结构
- `SKILL.md`：主 Skill 框架
- `TEST_GATE.md`：统一门禁规则
- `project-config.yaml.example`：项目配置模板
- `guides/`：运行手册、证据规则、状态判定、报告结构
- `prompts/`：可直接粘贴给代理的启动/复测/缺证据提示词
- `templates/`：按项目类型区分的检查模板
- `project-presets/`：项目预设示例
- `examples/`：输出样例

## 核心原则
- 没有真实证据，不得默认 PASS。
- 静态推断只能支持“可疑通过”或“待验证”，不能替代真实测试。
- P0 未闭环，不允许输出总 PASS。
- 代理必须逐条说明：验证方式、证据、状态、风险、下一步动作。
