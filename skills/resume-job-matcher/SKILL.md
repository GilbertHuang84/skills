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

## Prerequisites

Run `resume-analyzer` skill first if not already done. Its output provides:
- Search keywords for job filtering
- Personal SWOT baseline
- Development direction recommendations
- Company type fit assessment

## Workflow

### Phase 1: Data Collection

1. **Read resume** from `data/resume/` (supports `.md`, `.docx`, `.pdf`)
2. **Check for job data** in `data/dailies/` (CSV format, typically `jobs_YYYYMMDD.csv`)

#### Path A: Job data file exists

3a. **Read job data** from the CSV file
4a. **Filter relevant jobs**: Exclude jobs unrelated to the candidate's field

Use search keywords from `resume-analyzer` output if available.

#### Path B: No job data file provided

If no CSV/JSON job data file is found in `data/dailies/`, automatically search for jobs:

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

6b. **Save collected data** to `data/dailies/jobs_auto_{YYYYMMDD}.csv` for reproducibility

7b. **Continue with Phase 2** using the auto-collected job data

**Note**: Auto-collected data may be less complete than CSV exports. Mark any missing fields as "待确认" in the report.

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

### Phase 3.5: National Strategy & Policy Analysis

**Important**: Policy analysis must be **derived from the candidate's resume experience**, not a fixed policy list. Start from what the candidate has done, then map to relevant national strategies.

**How to derive policy relevance from resume**:
1. **Map resume industries to policy domains**: If the candidate worked in AI/tech → analyze AI policy; if in digital content → analyze cultural digitalization policy; if in specific regulated industries → analyze those industry policies
2. **Map resume skills to policy opportunities**: If the candidate has AI/ML skills → analyze AI+行动 policy; if has data platform experience → analyze data要素 market policy
3. **Map target companies to policy alignment**: If target companies are in policy-favored sectors → analyze those specific policies
4. **Only include policy domains that have a concrete connection** to the candidate's experience, target companies, or target roles
5. **Search for real-time policy data** using WebSearch (1-3 searches). Do NOT use static/old policy data from reference files. See `references/policy-analysis.md` §2 for the search workflow.

**If the candidate's experience spans many policy domains** (e.g., 10+ years across multiple industries), include all relevant ones.
**If the candidate's experience is focused** (e.g., mostly in one industry), only include the 3-5 most relevant policy domains.

Analyze the macro policy environment and apply it to **each specific company and job**. See `references/policy-analysis.md` for detailed framework.

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

See `references/policy-analysis.md` §4.5 for the full stage assessment and entry analysis framework.

### Phase 4: Report Generation

Generate a DOCX report using `docx-js` (Node.js). Follow `references/docx-guide.md` for formatting.

**Report structure** (three major sections):

**封面**: Candidate name + data sources + date. No version labels, no feature tags.
- Data sources: List all sources used (e.g., BOSS直聘, 企业官网, 天眼查/企查查, 行业媒体, 政策文件等)
- Date: Placed after data sources, represents the actual update time of this document

**页眉**: "简历与岗位匹配分析报告 - {用户名}" on every page.

---

### Part A: 简历分析 (Resume Analysis)

A1. 简历完整性评估 — supplement suggestions with priority (🔴高/🟡中/🟢低), no status column
A2. 技能时效性分析 — **derived from resume only**, Active/Stagnant/Outdated rating per skill
A3. 技术架构适配性分析 — **derived from resume project experience**, sorted by market demand descending
A4. 简历漏洞分析 — HR perspective credibility check, logic conflicts, trustworthiness concerns (NEW)
A5. 公司类型适配分析 — sorted by fit score descending

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

**Output**: Save to workspace as `resume_job_match_report.docx`

## Important Rules

- Always detect the user's language and produce all output in that language
- Salary expectations should be inferred from experience level and industry benchmarks
- Never fabricate company data; clearly mark when information is unavailable
- Use `infoBox` pattern (colored callout tables) for key insights and warnings
- After generation, run `sanitize.py` from the docx skill if available
- Search keywords should be derived from resume analysis, not assumed
