# 标准报告结构

建议代理最终同时输出三类产物：
- Markdown 报告
- CSV 证据矩阵
- JSON 摘要

## 一、Markdown 报告建议结构
1. 本轮目标
2. 输入材料完整性
3. 项目地图
4. P0 与主流程
5. 关键用例逐条判定
6. 未通过项清单
7. 复测建议
8. 人工补测建议
9. 总门禁结论

## 二、CSV 证据矩阵字段
- case_id
- module
- priority
- verification_target
- verification_method
- evidence_level
- evidence_type
- evidence_path
- status
- risk
- next_action

## 三、JSON 摘要字段建议
```json
{
  "project_name": "",
  "project_type": "",
  "stage": "",
  "mode": "executable|static",
  "total_status": "PASS|FAIL|BLOCKED|NEEDS_HUMAN_CHECK",
  "p0_total": 0,
  "p0_pass": 0,
  "p0_fail": 0,
  "p0_blocked": 0,
  "p0_needs_human_check": 0,
  "main_flow_status": "",
  "key_risks": [],
  "must_retest": [],
  "manual_checks": []
}
```

## 四、建议
如果代理能力有限，至少输出 Markdown + 表格。  
如果代理可写文件，推荐把 CSV 和 JSON 也一起产出。
