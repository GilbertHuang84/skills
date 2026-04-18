# Analysis Dimensions Reference

Detailed guidance for each analysis dimension in the matching report.

## 1. Skill Timeliness Assessment

### Core Principle

**Only analyze skills that appear in or can be inferred from the candidate's resume.** Do NOT use a generic skill template.

### How to Derive Skills from Resume

| Source | Example | Derived Skills |
|--------|---------|---------------|
| Explicit skill list | "Python, Docker, Jenkins" | Python, Docker, Jenkins |
| Project descriptions | "搭建自动化系统，使用Python和Docker" | Python, Docker, automation development |
| Role titles | "技术负责人 at 某科技公司" | Technical development, workflow management |
| Tool/platform mentions | "基于某平台API开发" | Platform integration, API development |
| Technology stack | "使用某框架进行数据交换" | 某框架, data architecture |

### Rating Criteria

| Status | Criteria | Examples |
|--------|----------|---------|
| **Active** | Used in current job within last 6 months; technology is current industry standard | Python (primary language), Docker/K8s (daily use) |
| **Stagnant** | Used 1-3 years ago; foundation remains but latest changes not tracked | 某框架/工具 (used at previous job but not current focus) |
| **Outdated** | Not used 3+ years; technology has evolved significantly; major relearning needed | C++ (last used 10+ years ago), 某旧版工具 (已被新工具替代) |

### Assessment Factors

1. **Time since last use**: The longer the gap, the more decay
2. **Technology evolution rate**: Fast-moving tech (AI tools, new frameworks) decays faster than stable tech (Python, SQL)
3. **Transferability**: Related skills slow decay (Python -> any scripting; 某平台API -> any platform API)
4. **Market demand**: High-demand skills justify recovery investment

### Common Skill Decay Patterns

| Skill | Typical Decay Timeline | Recovery Effort |
|-------|----------------------|-----------------|
| Python | Minimal (universal) | 1-2 weeks |
| 主流平台API | Slow (stable API) | 2-4 weeks |
| 某专业工具 | Medium (active development) | 1-2 months |
| 某快速迭代框架 | Fast (major updates) | 2-3 months |
| 某新兴标准 | Medium (growing adoption) | 1-2 months |
| C++ | Slow decay but fast tech change | 3-6 months |

## 2. Company Match Scoring

### Scoring Framework (0-100%)

| Factor | Weight | Indicators |
|--------|--------|------------|
| Industry alignment | 30% | Same or adjacent industry |
| Company stability | 25% | Funding, revenue, client base |
| Growth potential | 25% | Market position, expansion signals |
| Cultural fit potential | 20% | Team size, work style, location |

### Development Stage Assessment

| Stage | Signals | Risk Level |
|-------|---------|------------|
| **Early startup** | <50 people, seed funding, no revenue | High |
| **Growth** | 50-200 people, Series A-B, rapid hiring | Medium |
| **Mature** | 200+ people, profitable or late-stage funding, stable clients | Low |
| **Declining** | Layoffs, shrinking headcount, outdated tech stack | High |
| **Uncertain** | No public info, no funding, no media coverage | High |

### Red Flags

- No verifiable projects or clients
- Massive hiring without clear business driver
- JD content mismatched with job title (data scraping error)
- No funding but aggressive expansion
- Single-client dependency

## 3. Dual SWOT Analysis: Personal vs Company

**Critical**: Always separate personal strengths/weaknesses from company strengths/weaknesses. Never conflate company risk with personal shortcoming.

### 3A. Personal SWOT (Candidate-Centric)

#### Personal Strengths

- Be specific: cite concrete projects, companies, measurable outcomes
- Connect to job requirements: "Your X experience directly addresses their need for Y"
- Use evidence from resume: project names, technologies, achievements

**Template**: "[Specific experience from resume] directly maps to [job requirement], because [reason]."

#### Personal Weaknesses

- Be honest but constructive: every gap should have a mitigation strategy
- Distinguish between "hard gap" (missing skill) and "soft gap" (stagnant skill)
- Consider recovery time and whether it's worth the investment
- **Never attribute company risk to personal weakness** (e.g., "公司规模小" is NOT a personal weakness)

**Template**: "[Skill] gap exists because [reason], but [mitigation strategy]."

### 3B. Company SWOT (Company-Centric)

#### Company Strengths

Evaluate what the company offers to the candidate:

| Dimension | What to Assess |
|-----------|---------------|
| **Industry position** | Market share, client quality, brand recognition |
| **Technical environment** | Tech stack maturity, tools investment, R&D culture |
| **Growth opportunity** | Promotion path, skill development, project variety |
| **Compensation** | Base salary, benefits, equity/bonus potential |
| **Stability** | Funding, revenue, 客户项目, market demand |

#### Company Weaknesses (Risks)

Evaluate risks the candidate would face:

| Dimension | What to Assess |
|-----------|---------------|
| **Financial risk** | Underfunded, revenue dependency, burn rate |
| **Career risk** | Narrow role scope, no growth path, skill stagnation |
| **Culture risk** | Overtime culture, toxic management, high turnover |
| **Market risk** | Industry decline, client concentration, competition |
| **Operational risk** | Chaotic process, understaffed, unclear expectations |

**Important**: Company weaknesses are NOT personal shortcomings. They are environmental risks the candidate should be aware of.

### 3C. Combined Match Assessment

For each job, present a clear two-column view:

```
| 你的优势 vs 岗位需求 | 公司优势 vs 你的需求 |
| 你的短板 vs 岗位要求 | 公司风险 vs 你的底线 |
```

This makes it clear: personal fit and company quality are independent dimensions.

### 6. Three-Dimensional Response Strategy (三维度应对策略)

**Critical**: "应对策略" is NOT just interview tips. It must cover three independent dimensions for each job opportunity.

#### Dimension A: Interview Strategy (面试应对策略)

For each job, provide 2-3 concrete talking points:
1. Lead with strongest personal match
2. Acknowledge personal gaps honestly, show learning plan
3. Ask questions to verify company strengths and probe company risks

See `references/interview-scripts.md` for question templates.

#### Dimension B: Resume Adjustment Strategy (简历调整策略)

Tailor the resume for each target company/role:

| Adjustment Type | When to Apply | Example |
|----------------|---------------|---------|
| **Highlight reorder** | Move most relevant experience to top | If targeting a specific role, put most relevant projects first |
| **Skill emphasis** | Bold or expand skills matching JD keywords | If JD emphasizes a specific technology, expand that experience section |
| **Metric addition** | Add quantified outcomes for relevant projects | "系统并行任务处理能力从百级提升至万级" |
| **Project selection** | Choose 3-5 projects most relevant to target | Omit unrelated projects to keep resume focused |
| **Title alignment** | Adjust how past roles are described | Emphasize management aspects if targeting lead roles |
| **Gap mitigation** | Add brief notes for career gaps or short tenures | Explain frequent changes with positive framing |

**Template**: "针对[目标公司/岗位]，建议将[某项目/技能]前置，弱化[不相关内容]，补充[缺失的量化数据]。"

#### Dimension C: Career Direction Strategy (职业方向策略)

For each job, assess the strategic value beyond just "getting this job":

| Strategy | Signals | Recommendation |
|----------|---------|----------------|
| **长期发展 (Long-term)** | Company is stable/growing, role has clear promotion path, skills are transferable | "适合长期深耕，建议3-5年规划" |
| **跳板策略 (Stepping stone)** | Company brand value > role scope, or industry is in transition | "可作为跳板积累[某方面]经验，建议1-2年后评估" |
| **短期过渡 (Short-term)** | Role fills a gap but limited growth, or company has high risk | "适合短期过渡，同时保持外部机会关注" |
| **战略转型 (Strategic pivot)** | Role enables entry into a new industry or technology domain | "可作为向[新领域]转型的切入点" |

**Assessment factors**:
- Company growth trajectory and policy alignment
- Role scope vs career ceiling
- Skill accumulation potential (what you'll learn)
- Network and brand value of the company
- Market timing (is this industry growing or shrinking?)
- Personal life stage and priorities

**Template**: "该岗位的战略价值评估：[长期/跳板/短期/转型]。理由：[具体分析]。建议：[行动计划]。"

### Combined Strategy Output Format

For each job in the report, present all three dimensions:

```
| 面试策略 | [2-3 talking points] |
| 简历调整 | [specific adjustments for this target] |
| 职业方向 | [long-term / stepping stone / short-term / pivot + reasoning] |
```

## 4. Company Type Fit Assessment

See `references/company-types.md` for the full classification framework.

For each candidate, assess fit across company types (SOE, MNC, Private, Unicorn, Specialized SME, Outsourcing, Agency) based on resume signals:

| Resume Signal | Inferred Preference | Best Fit Types |
|--------------|-------------------|----------------|
| Long tenure (3+ years) at one company | Values stability | SOE, MNC, Specialized SME |
| Frequent job changes (<2 years each) | Seeks growth/change | Startup, Private |
| Management experience | Ready for larger scope | MNC, Unicorn |
| Deep technical specialization | Values expertise depth | Specialized SME, MNC |
| Early career (<5 years) | Building foundation | Outsourcing (acceptable), Private |
| Multiple industry switches | Explores broadly | Private, Startup |

Present in report as: "根据你的简历特征，以下公司类型可能更适合你的发展阶段：..."

## 5. Current Employer Handling

### Rules

1. If job is from candidate's current employer, mark as **reference only**
2. Remove from recommendation rankings
3. Still analyze the match (useful for understanding market value)
4. Add a note: candidate may be preparing to leave; returning is not the goal

### Why This Matters

- Candidates rarely want to return to a company they're leaving
- Including it in rankings wastes the candidate's attention
- But the salary and requirements data is still valuable for market benchmarking
