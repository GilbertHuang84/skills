---
name: resume-job-matcher
description: >
  Analyze resume-job matching and generate professional DOCX reports. Use when the user wants to:
  (1) Match a resume against job listings (from CSV data OR auto-searched from job platforms),
  (2) Analyze company background, type, culture and hiring intent,
  (3) Evaluate skill timeliness and career competitiveness,
  (4) Perform dual SWOT analysis (personal vs company),
  (5) Assess company type fit for candidate's career stage,
  (6) Incorporate national strategy and policy analysis for forward-looking career guidance,
  (7) Generate a comprehensive job matching analysis report as a .docx file.
  Triggers: "简历匹配", "岗位分析", "job match", "resume analysis", "投递建议",
  "简历和岗位", "匹配度分析", "找工作分析", "帮我找工作", "分析岗位".
---

# Resume-Job Matcher

Generate professional resume-job matching analysis reports in DOCX format.

## 设计哲学

**本 SKILL 专注于岗位匹配与公司分析，简历分析（Part A）由 `resume-analyzer` 完成。** 本 SKILL 在 Phase 0.5 读取 `resume-analyzer` 的审阅报告，将其中的技能时效性、架构适配性、简历漏洞、公司类型适配等分析结果直接引用到最终报告中，避免重复分析。如果尚未运行 `resume-analyzer`，本 SKILL 会提示用户先完成简历审阅。

**职责分工：**
- `resume-analyzer`：负责简历的结构化分析（经历梳理、四维审视、技能提取、漏洞检测等）
- `resume-job-matcher`（本 SKILL）：负责岗位搜索、公司调研、SWOT 分析、政策分析、DOCX 报告生成

## Prerequisites

Run `resume-analyzer` skill first if not already done. Its output provides:
- Search keywords for job filtering
- Personal SWOT baseline
- Development direction recommendations
- Company type fit assessment
- Skill timeliness analysis results
- Technical architecture fit assessment
- Resume vulnerability analysis
- Four-dimensional review results (HR, technical, business, executive)

## Workflow

### Phase 0.5: 读取 resume-analyzer 审阅报告

**在开始任何分析之前，必须先读取 `resume-analyzer` 的审阅结果。**

1. **定位审阅报告**：从 `data/{用户标识}/resume-reviews/` 目录下查找最新的审阅报告（`review-*.md`，按日期排序取最新）
2. **如果没有审阅报告**：
   - 提示用户先运行 `resume-analyzer` 对简历进行审阅
   - 提供审阅报告的预期路径：`data/{用户标识}/resume-reviews/review-{日期}.md`
   - **不要跳过此步骤继续执行**
3. **如果有审阅报告**，提取以下信息用于后续分析：
   - **搜索关键词**：从经历和技能中提取的岗位搜索关键词（用于 Phase 1 岗位筛选）
   - **技能时效性分析**：Active/Stagnant/Outdated 评级（直接引用到报告 Part A）
   - **技术架构适配性**：架构模式与市场需求的匹配评估（直接引用到报告 Part A）
   - **简历漏洞分析**：HR 视角的可信度检查结果（直接引用到报告 Part A）
   - **公司类型适配分析**：适合候选人的公司类型排序（直接引用到报告 Part A）
   - **四维审视结果**：HR/技术/业务/决策层视角的判定和发现（用于 SWOT 分析的输入）
   - **经历信息提取**：核心贡献、能力证据、可迁移能力等（用于岗位匹配度评估）

### Phase 1: Data Collection

1. **Read resume** from `data/{用户标识}/` (supports `.md`, `.docx`, `.pdf`)
2. **Check for job data** in `data/{用户标识}/` (CSV format, typically `jobs_YYYYMMDD.csv`)

#### Path A: Job data file exists

3a. **Read job data** from the CSV file
4a. **Filter relevant jobs**: Exclude jobs unrelated to the candidate's field

Use search keywords from `resume-analyzer` output if available.

#### Path B: No job data file provided

If no CSV/JSON job data file is found in `data/{用户标识}/`, automatically search for jobs:

3b. **Run `resume-analyzer` skill first** (if not already done) to extract:
   - Search keyword tiers (primary, secondary, negative)
   - Target company types
   - Development direction recommendations

4b. **Search job platforms** using the extracted keywords:
   - Use WebSearch with queries like: `"{primary keyword} 招聘 {location}" site:zhipin.com`
   - Search budget: 2-3 searches across different keyword combinations
   - Target platforms: BOSS直聘, 拉勾, 猎聘, LinkedIn (choose based on candidate's industry)

5b. **Collect job listings**: For each relevant job found, extract:
   - Job title, company name, location, salary range
   - Job description (full text if available)
   - Contact person info if visible

6b. **Save collected data** to `data/{用户标识}/job-matching/jobs_auto_{YYYYMMDD}.csv` for reproducibility

7b. **Continue with Phase 2** using the auto-collected job data

**Note**: Auto-collected data may be less complete than CSV exports. Mark any missing fields as "待确认" in the report.

**Phase 1 完成后，保存中间结果**：将原始岗位数据保存到 `data/{用户标识}/job-matching/raw-jobs.md`，包含所有采集到的岗位信息（标题、公司、薪资、地点、描述等），便于后续回溯和调试。

### Phase 1.5: Job Pre-Filtering & Transparency

**Critical**: The filtering process must be fully transparent in the report. Candidates need to see exactly how many jobs were collected, how many were filtered at each stage, and why.

#### Filtering Steps

1. **Remove irrelevant jobs**: Compare each job's description against the candidate's resume. If the job description does not overlap with any of the candidate's skills, work experience, or target directions, exclude it. Common examples include jobs in completely different industries (e.g., HR/training, semiconductor, general software development) that happen to share abbreviations with the candidate's field.
2. **Remove junior/intern roles**: Filter out internships, campus recruitment, and roles requiring significantly less experience than the candidate has.
3. **Remove unrelated industries**: Filter out jobs from industries that have no overlap with the candidate's career history, even if they share keywords with the candidate's target roles.
4. **Score and rank remaining jobs**: Use a relevance scoring system based on how well each job's requirements match the candidate's skills and experience (derived from the resume, not from generic keyword templates).
5. **Deduplicate**: Remove duplicate entries for the same company + job title combination.

#### Required Report Output

The report MUST include a **"岗位预筛说明"** section (between Part A: Resume Analysis and Part B: Company Analysis) containing:

1. **Filtering funnel table** showing:
   - Total jobs crawled
   - Jobs removed at each filtering stage (with reason)
   - Final count of relevant jobs
   - Count of jobs selected for deep analysis

2. **Complete job summary table** listing ALL pre-filtered relevant jobs (typically 15-25 jobs) with:
   - Job title, company, salary, experience requirement, location
   - Relevance score
   - Target city indicator (★ for candidate's preferred cities)
   - Sort by relevance score descending

3. **Explanation of deep analysis selection**: Clearly state which jobs were selected for deep SWOT analysis and why, so the candidate understands that the remaining jobs were not "ignored" but are available for reference.

**Why this matters**: Without this transparency, candidates may think the agent discarded most of their job data. Showing the full filtering pipeline builds trust and allows candidates to spot any filtering errors. The filtering criteria should always be described in terms of the candidate's resume and experience, not in terms of generic industry keywords.

**Phase 1.5 完成后，保存中间结果**：将筛选后的岗位列表保存到 `data/{用户标识}/job-matching/filtered-jobs.md`，包含筛选漏斗统计、筛选后岗位汇总表、相关性评分和深度分析选择说明。

### Phase 2: Company Research

For each relevant company, search the web for:

- Company founding year, scale, funding status
- Core business and products
- Recent projects, partnerships, or media coverage
- Development stage assessment (startup/growth/mature/declining)
- **Company type classification** (SOE/MNC/Private/Unicorn/Specialized SME/Outsourcing/Agency)

See `references/company-types.md` for the full classification framework and its implications for interview rounds, challenges, and growth curves.

**Search budget**: Max 3-5 searches total. Batch multiple companies into one query when possible.

### Phase 2.5: Company Culture & Work Environment Research

See `references/company-culture.md` for detailed framework.

**Assess three dimensions**:
1. **Overtime culture**: Infer from industry norms and company stage
2. **Team stability**: High turnover signals? Key person dependency?
3. **Management style**: Top-down vs collaborative

**When data is unavailable**: Mark as "需面试确认" and provide suggested interview questions (see `references/interview-scripts.md`).

**Phase 2 完成后，保存中间结果**：将公司调研结果保存到 `data/{用户标识}/job-matching/company-research.md`，包含各公司的背景信息、类型分类、文化评估、发展阶段判定等。

### Phase 3: Multi-Dimensional Analysis

Perform these analyses for each relevant job. See `references/analysis-dimensions.md` for detailed guidance.

#### 3.1 Skill Timeliness Assessment

**Important**: Only analyze skills that are **explicitly mentioned in the resume** or can be **reasonably inferred from work experience**. Do NOT include a generic skill template.

**How to derive skills from resume**:
1. **Explicit mentions**: Skills directly named in job descriptions or project sections
2. **Inferred from projects**: If the resume says "built an automated system using Python and Docker", then Python and Docker are skills even if not listed separately
3. **Inferred from roles**: If the role was "Technical Developer at a tech company", then development and workflow skills can be inferred
4. **Inferred from tools**: If a specific platform/tool is mentioned as used, then integration with that platform is a skill

Rate each derived skill: **Active** / **Stagnant** / **Outdated**. See analysis-dimensions.md §1.

#### 3.2 Company-Job Match

Score independently:
- **Company match** (0-100%): Industry alignment, stability, growth, company type fit
- **Job match** (0-100%): Skill overlap, experience level fit, role relevance

#### 3.2b Technical Architecture Fit Assessment

**Important**: Architecture patterns must be **derived from the candidate's actual project experience**, not from a generic template.

**How to derive architecture experience from resume**:
1. Read each project description for architecture clues: "built a modular system" → microservices/modular; "async task processing" → event-driven; "container deployment" → cloud-native; "plugin development" → plugin architecture
2. Map each project to one or more architecture patterns
3. Only include patterns that have concrete evidence from the resume

**Assessment format**:

| Architecture Pattern | Evidence from Resume | Market Demand | Fit Rating |
|---------------------|---------------------|---------------|------------|
| [Pattern name] | [Specific project/experience quote] | High/Medium/Low/Declining | High/Medium/Low |

Sort by market demand descending.

#### 3.3 Dual SWOT Analysis (Personal vs Company)

**Critical**: Always separate personal and company analysis. See analysis-dimensions.md §3.

For each job, produce four quadrants:
1. **Personal strengths vs job requirements**: What the candidate brings
2. **Personal weaknesses vs job requirements**: Gaps to address
3. **Company strengths vs candidate needs**: What the company offers
4. **Company risks vs candidate's bottom line**: Environmental risks

**Never attribute company risk to personal weakness.** (e.g., "公司规模小" is a company risk, NOT a personal shortcoming)

#### 3.4 Company Type Fit Assessment

Based on resume signals, assess which company types best match the candidate's career stage. See `references/company-types.md`.

Present as: "根据你的简历特征，以下公司类型可能更适合你的发展阶段：..."

#### 3.5 Current Employer Handling

If job is from candidate's current employer:
- Mark as **reference only**, remove from rankings
- Still analyze for market benchmarking

#### 3.6 Culture Fit & Interview Guidance

For each job, provide:
- Culture risk assessment (Low/Medium/High/Pending)
- Information gaps to verify during interview
- Suggested interview questions (see `references/interview-scripts.md`)
- Community intelligence sources

**Phase 3 完成后，保存中间结果**：将多维分析结果保存到 `data/{用户标识}/job-matching/analysis.md`，包含技能时效性、公司匹配度、架构适配性、SWOT 分析、文化适配评估等所有分析内容。

### Phase 3.5: National Strategy & Policy Analysis

**Important**: Policy analysis must be **derived from the candidate's resume experience**, not a fixed policy list. Start from what the candidate has done, then map to relevant national strategies.

**How to derive policy relevance from resume**:
1. **Map resume industries to policy domains**: If the candidate worked in AI/tech → analyze AI policy; if in digital content → analyze cultural digitalization policy; if in specific regulated industries → analyze those industry policies
2. **Map resume skills to policy opportunities**: If the candidate has AI/ML skills → analyze AI+行动 policy; if has data platform experience → analyze data要素 market policy
3. **Map target companies to policy alignment**: If target companies are in policy-favored sectors → analyze those specific policies
4. **Only include policy domains that have a concrete connection** to the candidate's experience, target companies, or target roles
5. **Search for real-time policy data** using WebSearch (1-3 searches). Do NOT use static/old policy data from reference files. See `skills/policy-analysis.md` §2 for the search workflow.

**If the candidate's experience spans many policy domains** (e.g., 10+ years across multiple industries), include all relevant ones.
**If the candidate's experience is focused** (e.g., mostly in one industry), only include the 3-5 most relevant policy domains.

Analyze the macro policy environment and apply it to **each specific company and job**. See `skills/policy-analysis.md` for detailed framework.

**Core principle**: Policy analysis must go beyond macro-level overview. For every company and job in the report, provide concrete policy alignment assessment.

**Company-level assessment** (for each target company):
1. **Policy alignment rating** (S/A/B/C/D): Is the company's core business in a policy-favored sector?
2. **Development prospect**: Combined policy + industry analysis — is the company in a growth, stable, or declining trajectory?
3. **Policy risk exposure**: Does the company face regulatory tightening risks?
4. **Growth potential**: Could policy changes create new growth opportunities for this company?

**Job-level assessment** (for each target job):
1. **Skill policy premium**: Does policy create salary premium for this job's required skills?
2. **Demand trend**: Is policy driving demand growth for this role?
3. **Substitution risk**: Could policy-driven tech changes (e.g., AI) threaten this role?
4. **3-5 year demand forecast**: How will policy affect this job's market demand over time?

**Key policy domains to check** (relevant to tech/digital industries):
- 数字中国建设 (Digital China)
- AI产业政策 (AI regulation and development)
- 文化数字化战略 (Cultural digitalization, virtual humans)
- 元宇宙/虚拟现实产业 (Metaverse/VR/AR)
- 数字内容产业政策 (Digital content industry policy)
- 新质生产力 (New quality productive forces)

**Search budget**: 1-2 searches for current policy trends. Focus on the most recent policy announcements affecting the candidate's target industries.

**Output in report**:
- Policy environment overview table (macro) — must include: 阶段(上升/平稳/下降/转型), 契合度星标(★★★★★), 基建状态, 入局建议
- **Per-company policy alignment + development prospect** (concrete, specific to each company)
- **Per-job policy fit + 3-5 year demand forecast** (concrete, specific to each role)
- Career direction policy-resilience assessment

See `skills/policy-analysis.md` §4.5 for the full stage assessment and entry analysis framework.

**Phase 3.5 完成后，保存中间结果**：将政策分析结果保存到 `data/{用户标识}/job-matching/policy-analysis.md`，包含宏观政策环境概述、各公司政策契合度评估、各岗位政策适配分析和 3-5 年需求预测。

### Phase 4: Report Generation

Generate a DOCX report using `docx-js` (Node.js). Follow `references/docx-guide.md` for formatting.

**Report structure** (three major sections):

**封面**: Candidate name + data sources + date. No version labels, no feature tags.
- Data sources: List all sources used (e.g., BOSS直聘, 企业官网, 天眼查/企查查, 行业媒体, 政策文件等)
- Date: Placed after data sources, represents the actual update time of this document

**页眉**: "简历与岗位匹配分析报告 - {用户名}" on every page.

---

### Part A: 简历分析 (Resume Analysis)

> **数据来源：本部分内容直接引用 `resume-analyzer` 的审阅报告（Phase 0.5 读取），不重新分析。**
> 审阅报告路径：`data/{用户标识}/resume-reviews/review-{最新日期}.md`

A1. 简历完整性评估 — 引用审阅报告中的经历完整度统计（🟢🟡🔴），补充建议按优先级排列（🔴高/🟡中/🟢低）
A2. 技能时效性分析 — 引用审阅报告中的技能时效性评级（Active/Stagnant/Outdated），不自行推断
A3. 技术架构适配性分析 — 引用审阅报告中的架构适配评估，按市场需求降序排列
A4. 简历漏洞分析 — 引用审阅报告中的四维审视结果（特别是 HR 视角红灯信号），不自行分析
A5. 公司类型适配分析 — 引用审阅报告中的公司类型适配排序，不自行推断

### Part A.5: 岗位预筛说明 (Job Pre-Filtering)

A5.1. 筛选漏斗 — table showing total crawled → filtered at each stage → final count
A5.2. 预筛岗位汇总 — complete table of ALL pre-filtered relevant jobs (15-25 jobs), sorted by relevance score, with target city indicator (★)
A5.3. 深度分析说明 — explanation of which jobs were selected for deep analysis and why

### Part B: 公司分析 (Company Analysis)

B1. 国家战略与政策环境概述 — **derived from resume experience**, only include policy domains relevant to candidate's background
B2. 公司背景与动态 — per company: type, scale, policy alignment (with ★), development prospect (with ★), culture risk
    - Core business-policy mapping table sorted by 契合度 descending
    - Development prospect items with star ratings

### Part C: 岗位分析 (Job Analysis)

C1. 逐岗位双向SWOT分析 — personal vs company, four quadrants per job
C2. 三维度应对策略 — interview strategy + resume adjustment + career direction per job
C3. 综合匹配度排名 — weighted score table with policy-resilience

### Part D: 规划建议 (Planning & Recommendations)

D1. 职业方向政策韧性评估 — which career paths are most future-proof
D2. 文化适配与面试探寻 — per company culture fit and interview guidance
D3. 关键发现与行动建议 — key findings + action items

**Appendix: 参考来源**
All web sources cited in the report (company websites, policy documents, industry reports, etc.) listed in GB/T 7714-2015 format with URLs.

**AI生成声明**
The report must include a disclaimer: "本报告由AI辅助生成，仅供参考，不构成投资、求职或职业决策建议。报告中的分析和建议基于公开数据和网络信息，可能存在信息滞后或不准确的情况。请结合自身实际情况独立判断。"

## Anonymization Rules

**Skill reference files** (in `references/`): Use generic placeholders for all examples (e.g., "某外资公司", "某科技创业公司"). This ensures skills are reusable for any candidate.

**Generated reports** (`.docx` output): Use **real company names and real job titles** from the actual data. The report is for the specific candidate and should contain concrete, actionable information. Only anonymize the candidate's personal contact info (email, phone, WeChat ID) — name and work history should appear as-is since the candidate needs to verify accuracy.

**Output**: Save to `data/{用户标识}/resume_job_match_report.docx`

## Important Rules

- Always detect the user's language and produce all output in that language
- Salary expectations should be inferred from experience level and industry benchmarks
- Never fabricate company data; clearly mark when information is unavailable
- Use `infoBox` pattern (colored callout tables) for key insights and warnings
- After generation, run `sanitize.py` from the docx skill if available
- Search keywords should be derived from resume analysis, not assumed

---

## Phase 5: 求职策略与执行计划（报告生成后）

**报告生成后，不要让用户拿着报告就走。** 需要帮用户制定可执行的求职计划。

### Step 5.1 弹性时间规划

**根据用户情况制定求职时间表：**

| 用户状态 | 建议求职周期 | 每周投递量 | 每周面试目标 |
|---------|------------|-----------|------------|
| 全职求职（已离职） | 4-8 周 | 10-15 家 | 2-3 次 |
| 全职求职（在职） | 8-16 周 | 3-5 家 | 1-2 次 |
| 在校校招 | 2-4 周（秋招/春招窗口期） | 5-10 家 | 1-2 次 |
| 佛系观望 | 不设硬性目标 | 按兴趣投 | 不设目标 |

**时间规划模板：**

```markdown
# 求职行动计划：{用户标识}

## 基本信息
- 求职状态：[全职/在职/在校/观望]
- 目标岗位：[X]
- 意向城市：[X]
- 计划周期：[X 周]（[开始日期] — [结束日期]）

## 每周节奏
| 周 | 投递目标 | 面试目标 | 重点任务 |
|----|---------|---------|---------|
| 第 1 周 | [X] 家 | [X] 次 | [具体任务] |
| 第 2 周 | [X] 家 | [X] 次 | [具体任务] |
| ... | | | |

## 里程碑
- [ ] 第 2 周末：完成第一轮投递，收到 [X] 个回复
- [ ] 第 4 周末：完成 [X] 次面试，获得 [X] 个二面/offer
- [ ] 第 X 周末：[最终目标]
```

**弹性调整：**
- 如果第 2 周回复率低于 20% → 暂停投递，回顾简历和投递策略
- 如果连续 3 周没有面试机会 → 触发简历重新审视
- 如果收到面试但都挂在同一环节 → 触发针对性面试准备

**与用户确认：**
> "这是我根据你的情况做的求职时间规划。你看一下：
> - 整体周期 [X] 周，你觉得合理吗？
> - 每周 [X] 家的投递量会不会太多/太少？
> - 有没有哪段时间你有其他安排（考试/出差/假期）需要调整？
>
> 这个计划是弹性的，我们可以随时根据实际情况调整。"

### Step 5.2 进度打卡机制

**每次用户回来对话时，主动询问求职进度：**

> "距离上次聊天已经 [X] 天了，求职进展怎么样？
> 1. 投递了 [X] 家公司，收到 [X] 个回复？
> 2. 有没有面试？结果怎么样？
> 3. 有没有遇到什么困难或问题？"

**打卡记录：** 将每次打卡数据记录到 `data/{用户标识}/session-logs/job-search-progress.md`

**打卡数据用途：**
- 追踪投递回复率（低于 20% 需要调整策略）
- 追踪面试通过率（同一环节反复失败需要针对性准备）
- 追踪求职周期（超期需要心理支持）
- 为后续对话提供上下文

### Step 5.3 心理健康资源

**求职是一个高压过程，需要关注用户的心理状态。**

**识别信号：**

| 信号 | 可能的问题 | 回应方式 |
|------|----------|---------|
| 用户说"投了很多都没有回复" | 挫败感 | "这是正常的，大部分岗位的回复率只有 10-20%。不是你的问题。" |
| 用户说"面试又挂了" | 自我怀疑 | "面试失败不代表你不行，可能是匹配度的问题。我们来看看具体是哪个环节。" |
| 用户说"我是不是太差了" | 自信崩塌 | "你之前的模拟评价是 ⭐⭐⭐⭐，说明你的能力是够的。可能是策略需要调整。" |
| 用户说"不想投了" | 倦怠/焦虑 | "累了就休息几天，求职不是百米冲刺，是马拉松。休息好了再继续。" |
| 用户长时间不回来 | 可能放弃 | 如果超过 2 周没有对话，在下次对话时温和询问 |

**心理支持资源推荐：**

> "求职过程中如果感到压力很大，这些资源可能有帮助：
>
> **自助资源：**
> - 📖 《被讨厌的勇气》— 帮你理解"被拒绝"不代表"你不好"
> - 🎬 B 站搜索"求职焦虑 如何调节" — 很多过来人分享经验
> - 📝 写"成就日记"：每天记录 3 件做得好的小事（不管多小都算）
>
> **专业支持：**
> - 如果焦虑严重影响睡眠和日常生活，建议寻求专业心理咨询
> - 大部分高校都有免费的心理咨询中心
> - 线上心理咨询平台：简单心理、壹心理等
>
> 记住：求职的节奏是你自己定的，没有人规定你必须多快找到工作。"

**存档：** 将求职计划保存到 `data/{用户标识}/job-matching/job-search-plan.md`，打卡数据保存到 `data/{用户标识}/session-logs/job-search-progress.md`
