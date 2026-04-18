# Career Skills Toolkit

A collection of tools designed to assist with resume analysis and job matching workflows.

## Regional Availability

This toolkit is currently validated for use in China. Features may reference:
- Chinese job platforms (BOSS直聘, 拉勾, 猎聘)
- Chinese regulatory policies and economic strategies
- Local company classification systems (SOE, MNC, Private, etc.)
- Chinese-language document formats

Full functionality may not be guaranteed outside of China.

## Available Tools

| Tool | Description |
|------|-------------|
| [Resume Analyzer](#resume-analyzer) | Analyze resumes to extract structured insights for job search preparation |
| [Resume-Job Matcher](#resume-job-matcher) | Match resumes against job listings and generate matching reports |

---

### Resume Analyzer

Extract structured insights from resumes to optimize job search strategy.

**Features:**
- Resume completeness assessment with prioritized improvement suggestions
- Career trajectory analysis (companies, tenure, industry switches)
- Skill inventory categorization (languages, frameworks, tools, soft skills)
- Strengths and weaknesses evaluation
- Resume vulnerability analysis from HR perspective
- Development direction recommendations
- Search keyword generation (primary, extended, aspirational)
- Company type fit assessment

**Output**: `resume_analysis_report.docx`

---

### Resume-Job Matcher

Analyze resume-job compatibility and generate comprehensive matching reports.

**Features:**
- Skill timeliness assessment (Active/Stagnant/Outdated)
- Company background research (scale, funding, business focus)
- Company culture and work environment assessment
- Technical architecture fit analysis
- Dual SWOT analysis (Personal vs Company)
- Policy alignment analysis
- Comprehensive job matching ranking
- Professional DOCX report generation

**Prerequisite**: Run `resume-analyzer` first for optimal results.

**Output**: `resume_job_match_report.docx`

---

## Project Structure

```
skills/
├── [tool-name]/          # Individual tool directory
│   ├── references/       # Supplementary documentation and frameworks
│   │   └── *.md          # Domain-specific guidelines and templates
│   └── SKILL.md          # Tool definition and workflow specification
└── [tool-name].skill     # Tool registration file
```

### Structure Details

- **Tool Directories**: Each tool has its own directory containing:
  - `SKILL.md`: Main tool definition with workflow and configuration
  - `references/`: Optional supplementary documentation

- **Tool Registration**: `.skill` files register tools with the system

## Usage

### General Workflow

1. **Prepare Input Data**
   - Place relevant input files in the appropriate `data/` directory
   - Follow the specific tool's input requirements

2. **Execute Tools**
   - Tools can be run individually or in sequence
   - Some tools depend on outputs from other tools (see prerequisites)

3. **Review Output**
   - Outputs are saved to the workspace directory
   - Reports are generated in DOCX format where applicable

### Resume Analysis Workflow

1. **Run `resume-analyzer`** to extract:
   - Search keywords for job filtering
   - Personal SWOT baseline
   - Development direction recommendations
   - Company type fit assessment

2. **Run `resume-job-matcher`** to:
   - Match resume against job listings
   - Perform company research and analysis
   - Generate comprehensive matching report

**Input Requirements**:
- Resume files: Place in `data/resume/` (supports `.md`, `.docx`, `.pdf`)
- Job listings: Place in `data/dailies/` (CSV format, e.g., `jobs_YYYYMMDD.csv`)

## Features

- Multi-format support: Works with `.md`, `.docx`, and `.pdf` files
- Policy-aware analysis: Integrates national strategy analysis for forward-looking guidance
- HR perspective: Includes vulnerability analysis from recruiter viewpoint
- Actionable insights: Provides concrete recommendations with priority ratings
- Professional reports: Generates well-formatted DOCX reports

## Adding New Tools

To add a new tool to this repository:

1. Create a new directory under `skills/` with your tool name
2. Create a `SKILL.md` file with your tool definition
3. Add any reference documentation in a `references/` subdirectory
4. Create a `.skill` registration file

Refer to existing tools for formatting and structure examples.

## License

MIT License

## Contributing

Contributions are welcome. Please feel free to submit issues for bug reports or feature requests, or create pull requests for new tools or improvements.

---

Built to assist with career planning and job search workflows.