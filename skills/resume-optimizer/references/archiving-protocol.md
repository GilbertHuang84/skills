# 存档协议

> 本文件定义 resume-optimizer 的存档格式和模板。
> 注意：内部简历（internal-resume.md）的详细模板见 [resume-analyzer 的存档协议](../../resume-analyzer/references/archiving-protocol.md)。

---

## 存档目录结构

```
data/
└── {用户标识}/
    ├── internal-resume.md              # 内部简历（由 resume-analyzer 维护，本 SKILL 可回写）
    ├── profile.md                      # 用户画像（本 SKILL 可回写）
    ├── external-resumes/               # 外部简历（按目标分别存储，本 SKILL 产出）
    │   ├── resume-{目标1}.md
    │   └── resume-{目标2}.md
    └── resume-reviews/                 # 审阅报告（由 resume-analyzer 产出，本 SKILL 读取）
        └── review-{日期}.md
```

---

## 外部简历模板

以下为 `external-resumes/resume-{目标}.md` 的完整模板。

```markdown
# 外部简历：{目标岗位/公司/方向}

## 定制信息
- 目标：[具体岗位或方向]
- 目标公司类型：[互联网大厂/外企/创业公司/不限，可选]
- 创建日期：[日期]
- 版本：[正式版 / 预览版]
- 基于审阅报告：[审阅报告日期]

## 四维对齐策略

### HR 视角
- 审阅判定：[✅通过 / ⚠️薄弱 / ❌缺失]
- 对齐策略：[采取了什么措施确保此维度有证据]

### 技术/专业视角
- 审阅判定：[✅通过 / ⚠️薄弱 / ❌缺失]
- 对齐策略：[采取了什么措施确保此维度有证据]

### 业务视角
- 审阅判定：[✅通过 / ⚠️薄弱 / ❌缺失]
- 对齐策略：[采取了什么措施确保此维度有证据]

### 决策层视角
- 审阅判定：[✅通过 / ⚠️薄弱 / ❌缺失]
- 对齐策略：[采取了什么措施确保此维度有证据]

## 简历内容

[实际的简历内容]

## 与内部简历的差异
- 保留的经历：[列表]
- 过滤的经历：[列表及原因]
- 重组/强化的内容：[描述]

## 预览版备注（仅预览版）
- 信息不足的薄弱点：[列表]
- 建议补充的内容：[列表]
- 建议先运行 resume-analyzer 获取审阅报告以提升质量
```

---

## 回写标记规范

当 resume-optimizer 向 internal-resume.md 或 profile.md 回写内容时，使用以下标记格式：

```markdown
<!-- resume-optimizer YYYY-MM-DD 回写开始 -->
回写的内容...
<!-- resume-optimizer YYYY-MM-DD 回写结束 -->
```

**回写原则：**
- 只追加，不删除已有内容
- 每次回写使用独立的标记块
- 标记中包含日期，便于追溯
- 与 resume-analyzer 的回写标记区分（resume-analyzer 使用 `resume-analyzer review {日期}` 格式）

---

## 存档操作规范

1. **每次对话结束前必须存档**
2. **外部简历按目标分别存储**，同一用户可以有多份
3. **外部简历文件名规则**：`resume-{目标}.md`，目标用简短标识（如岗位名或方向）
4. **存档内容使用中文**（与用户对话语言一致）
5. **所有生成必须基于审阅报告中的具体证据**
6. **回写时必须标注来源**，便于区分是 resume-analyzer 还是 resume-optimizer 的更新
