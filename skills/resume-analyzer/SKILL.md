---
name: resume-analyzer
description: >
  Analyze a resume to extract structured insights: career trajectory, skill inventory,
  strengths/weaknesses, development directions, and job search keywords. Use when the user
  wants to: (1) Analyze their resume for job search preparation, (2) Extract search keywords
  for job platforms, (3) Identify career strengths and gaps, (4) Get development direction
  recommendations, (5) Prepare for resume-job matching analysis.
  Triggers: "简历分析", "分析简历", "resume analysis", "搜索关键词", "求职方向",
  "职业发展", "简历提炼", "我的优势劣势".
---

# Resume Analyzer

Analyze resumes to produce structured insights for job search preparation.

## Workflow

### Step 1: Read Resume & Completeness Check

Read resume from `data/resume/` (supports `.md`, `.docx`, `.pdf`).

**After reading, perform a completeness check** using the following checklist. Mark each item as ✅ Present or ❌ Missing.

#### Resume Completeness Checklist

> **注意**：薪资信息和离职原因**不适合**写在简历中。薪资在面试沟通阶段讨论，离职原因在面试中被问到时口头回答。以下清单仅包含适合补充到简历中的内容。

| 优先级 | 检查项 | 具体说明与补充方法 |
|--------|--------|-------------------|
| 🔴 高 | **量化成果** | 为每个项目补充可量化的结果。例如：不是写"搭建了流程系统"，而是写"搭建流程系统，将任务并行处理能力从百级提升至万级，降低PM管理成本约40%"。适合补充的数据类型：效率提升百分比、覆盖人数/部门数、节省的时间/成本、系统处理量级、项目规模（如"支持XX人团队协作"）。如果某些成果确实难以量化，可以用"首个"、"从零到一"、"覆盖全公司"等定性描述代替。 |
| 🔴 高 | **管理规模** | 为每个管理岗位补充团队人数和汇报关系。例如："管理8人技术团队，向CTO汇报"。如果没有直接管理经验，可以写"协调跨部门XX人团队"。不适合编造数字，但可以根据实际协作范围合理估算。 |
| 🟡 中 | **作品集链接** | 补充可展示的技术成果链接。适合补充的内容：GitHub仓库（有实际代码的）、技术博客文章（CSDN/掘金/知乎）、Demo视频（B站/YouTube）、在线作品集网站。不适合放的内容：公司内部系统的截图（可能涉密）、无法公开的项目。如果没有现成的，建议花1-2周整理2-3个代表性项目的技术文档或Demo。 |
| 🟡 中 | **语言能力** | 补充语言水平描述。例如："英语：读写熟练（在外资公司3年全英文工作环境），口语可进行技术讨论"。如果有托福/雅思/六级成绩，直接写分数。不适合写"英语良好"这种模糊描述，要给出具体场景证据。 |
| 🟡 中 | **项目时间线** | 为每个项目补充起止时间。格式："2022.07 - 2023.12"。如果项目是业余时间做的，标注"业余项目"。时间线帮助HR判断经验的新鲜度和项目并行情况。不适合编造时间，如果记不清可以写年份（如"2022年"）。 |
| 🟢 低 | **行业认证** | 补充相关认证或获奖记录。例如：AWS认证、PMP、行业技术大会演讲、公司内部技术奖项。如果没有，不需要为了补充而考证——把精力放在量化成果上收益更大。 |
| 🟢 低 | **社区贡献** | 补充开源项目或技术分享记录。例如：GitHub开源项目（标明star数和贡献角色）、技术大会演讲、技术社区活跃度。如果没有，建议从写1-2篇技术博客开始，比考证门槛更低、见效更快。 |

**不适合写入简历的内容**（不要建议用户补充）：
- ❌ 薪资信息/期望薪资 — 在面试沟通阶段讨论，不写在简历上
- ❌ 离职原因 — 在面试中被问到时口头回答，不写在简历上
- ❌ 个人联系方式中的非必要信息（如身份证号、详细家庭住址）

**Output**: Include a "简历完整性评估" section in the analysis report, listing missing items with specific suggestions for how to supplement them. This section feeds into the resume-job-matcher's "简历调整策略" dimension.

### Step 2: Extract Structured Data

Parse the resume into these categories:

#### 2.1 Career Timeline

| Field | Description |
|-------|-------------|
| Companies | Name, role, duration, industry |
| Trajectory pattern | Upward/lateral/downward/stable |
| Industry switches | Which industries and when |
| Tenure length | Average tenure per company |

#### 2.2 Skill Inventory

Categorize every mentioned skill:

| Category | Examples |
|----------|---------|
| Programming languages | Python, C++, C#, JavaScript |
| 专业工具/软件 | 行业专业软件 (e.g., IDE, design tools, domain-specific platforms) |
| Frameworks/platforms | Docker, K8s, Qt, Flask |
| Domain expertise | 技术负责人, 研发, AI应用, 数据工程 |
| Databases | MySQL, MongoDB, Redis |
| Project tools | Jira, Git, SVN, etc. |
| Soft skills | Team management, cross-team communication |

#### 2.3 Project Highlights

For each significant project:
- Project name and scale
- Candidate's specific role and contribution
- Technologies used
- Measurable outcomes (if mentioned)

### Step 3: Analyze Strengths & Weaknesses

#### Strengths Analysis

Evaluate across four dimensions:

1. **Technical depth**: Years of experience in core skills, complexity of projects handled
2. **Breadth**: Range of skills across different domains (技术 + 工具 + 管理)
3. **Leadership**: Team management, cross-team coordination, mentoring
4. **Industry recognition**: Awards, certifications, notable projects/companies

#### Weaknesses Analysis

Evaluate across four dimensions:

1. **Skill gaps**: Skills commonly required in target roles but missing from resume
2. **Skill stagnation**: Skills not used recently (see decay timeline in analysis-dimensions.md)
3. **Career trajectory risks**: Frequent job changes, lack of promotion progression, industry stagnation
4. **Visibility gaps**: Lack of measurable outcomes, no open-source/community contributions, no thought leadership

### Step 3.5: Resume Vulnerability Analysis (简历漏洞分析)

> **目的**：站在HR/面试官的角度审视简历，找出可能引发质疑的地方。这不是"信息缺失"（已在Step 1处理），而是"逻辑冲突、可信度问题、潜在疑虑"。

**分析维度**：

| 维度 | 具体检查项 | 疑虑类型 |
|------|-----------|---------|
| **时间线矛盾** | 各段工作经历的时间是否有重叠或空缺？频繁跳槽的时间间隔是否合理？ | 逻辑冲突 |
| **职级合理性** | 从初级到高级/管理的晋升速度是否合理？是否有"跳级"现象？ | 可信度 |
| **能力与职级匹配** | 简历声称的管理能力是否与实际管理年限匹配？技术深度是否与工作年限匹配？ | 可信度 |
| **行业跨度** | 跨行业跳槽是否有合理的逻辑？是否显得"什么都做过但都不深"？ | 逻辑冲突 |
| **项目规模真实性** | 项目描述的规模和影响力是否与公司规模匹配？（如10人公司声称"全行业领先"） | 可信度 |
| **技能广度疑虑** | 列出的技能是否过多过杂？是否像"什么都懂一点"而非"有核心竞争力"？ | 差异化 |
| **频繁跳槽信号** | 2年内换了3家以上公司？是否有稳定工作的记录？ | 稳定性 |
| **描述模糊** | 是否有大量形容词但缺乏具体事实？（如"负责核心系统开发"但没说是什么系统） | 可信度 |
| **年龄/经验匹配** | 工作年限与技能深度是否匹配？是否有"10年经验但只有3年水平"的嫌疑？ | 可信度 |
| **信息一致性** | 不同段落描述的同一经历是否一致？时间线、职责范围是否有矛盾？ | 逻辑冲突 |

**输出格式**：

对每个发现的漏洞，提供：
1. **问题描述**：具体是什么疑虑
2. **严重程度**：🔴高（很可能被质疑）/ 🟡中（可能被注意到）/ 🟢低（小问题）
3. **应对建议**：如何在面试中解释或如何在简历中调整

**注意**：不是所有简历都有漏洞。如果分析后没有发现明显问题，明确说明"未发现明显逻辑冲突或可信度疑虑"。

### Step 4: Development Direction Analysis

Based on strengths and weaknesses, identify 2-4 viable career directions:

For each direction, assess:
- **Fit score** (0-100%): How well current skills map to this direction
- **Growth potential**: Market demand, salary ceiling, career longevity
- **Investment needed**: Skills to learn, time to transition
- **Risk level**: Competition, industry stability
- **Policy alignment**: Whether this direction aligns with national policy tailwinds (see resume-job-matcher's `references/policy-analysis.md`)

### Step 5: Generate Search Keywords

Produce three tiers of keywords for job platforms:

#### Tier 1: Primary Keywords (必搜)
Direct matches from resume job titles and core skills.
- Example: "技术负责人", "流程开发", "研发工程师"

#### Tier 2: Extended Keywords (扩展搜)
Adjacent roles that leverage existing skills.
- Example: "技术美术", "TA", "AI应用开发", "数据平台"

#### Tier 3: Aspirational Keywords (探索搜)
Roles one step beyond current profile, requiring moderate upskilling.
- Example: "技术总监", "研发负责人", "技术主管"

Also generate **negative keywords** to filter out irrelevant results:
- Example: "半导体测试开发", "人力资源发展", "企业培训"

### Step 6: Output

Generate a DOCX report with:
1. **Resume completeness assessment** (from Step 1 checklist, with supplement suggestions)
2. **Resume vulnerability analysis** (from Step 3.5, HR perspective credibility check)
3. Career trajectory summary
4. Skill inventory table (with timeliness status)
5. Strengths & weaknesses analysis
6. Development direction recommendations
7. Search keyword tiers
8. Company type fit analysis (see references/company-types.md)

**Output**: Save to workspace as `resume_analysis_report.docx`

## Integration with resume-job-matcher

The output of this skill feeds into `resume-job-matcher`:
- Search keywords → used to filter job listings
- Weaknesses → referenced in per-job match analysis
- Development directions → used to evaluate company type fit
- Skill timeliness → shared with job matcher's skill assessment
