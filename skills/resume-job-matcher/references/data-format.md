# Job Data Format Reference

Expected formats for resume and job data files.

## Resume Location

Default path: `data/resume/`

Supported formats:
- `.md` (Markdown) — preferred, easy to parse
- `.docx` — use pandoc or docx skill to extract
- `.pdf` — use pdftotext or pdf skill to extract

## Job Data Format (CSV)

Default path: `data/dailies/jobs_YYYYMMDD.csv`

### Expected Columns

```csv
job_title,company,location,experience,education,decoded_salary,job_desc,contact_name,contact_title,url,source,crawl_time
```

| Column | Description |
|--------|-------------|
| job_title | Job title |
| company | Company name |
| location | Work location |
| experience | Required experience |
| education | Required education |
| decoded_salary | Salary range |
| job_desc | Full job description |
| contact_name | Recruiter name |
| contact_title | Recruiter title |
| url | Job listing URL |
| source | Job platform |
| crawl_time | Data fetch timestamp |

### Common False-Positive Job Title Patterns

Job titles containing abbreviations or acronyms that may match search keywords but belong to different fields entirely. The agent should detect and filter these based on job description content.

**General detection approach**: Read the `job_desc` column. If the description mentions technologies, tools, or responsibilities from a completely different industry than the candidate's target, it's a false positive — regardless of whether the title matches.

**Examples** (illustrative — adapt to the actual search keywords used):

| Job Title Pattern | Actual Field | Detection Keywords |
|------------------|-------------|-------------------|
| Titles containing target abbreviation + unrelated context | Different industry | Keywords from unrelated industries in job_desc |
| Abbreviation matches but role is completely different | Unrelated function | Job description technologies don't match target skill set |

### Filtering Strategy

1. First pass: Remove rows where `job_desc` contains keywords from unrelated industries
2. Second pass: Keep only rows matching candidate's target industry and skill set
3. Third pass: Verify company names against search results

## Job Data Format (JSON)

Alternative path: `data/job_data/job_data_YYYYMMDD_HHMMSS.json`

Structure varies by source. Extract `job_title`, `company`, `location`, `salary`, `job_desc` fields.
