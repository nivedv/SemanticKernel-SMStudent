# Advanced Prompt Engineering Practice Labs

## For Data Engineers, Business Analysts, and Data Analysts

---

## DATA ENGINEERS - Practice Labs

### Lab DE-1: Data Pipeline Troubleshooting Agent

**Scenario:** Your ETL pipeline is failing intermittently, and you need an AI agent to help diagnose issues from log files.

#### Basic Prompt (Test this first in Azure AI Foundry):

```
Analyze this ETL log and tell me what went wrong:
[LOG] 2024-08-28 14:32:15 - INFO: Starting data extraction from source database
[LOG] 2024-08-28 14:32:45 - ERROR: Connection timeout to database server 192.168.1.100
[LOG] 2024-08-28 14:33:00 - RETRY: Attempting reconnection (attempt 1/3)
[LOG] 2024-08-28 14:33:30 - ERROR: Connection timeout to database server 192.168.1.100
[LOG] 2024-08-28 14:34:00 - RETRY: Attempting reconnection (attempt 2/3)
[LOG] 2024-08-28 14:34:30 - ERROR: Connection timeout to database server 192.168.1.100
[LOG] 2024-08-28 14:35:00 - FATAL: Maximum retry attempts reached. Pipeline terminated.
```

#### Advanced Prompt (Compare the difference):

```
You are a senior data engineer with 8 years of experience troubleshooting production ETL pipelines. You're known for quickly identifying root causes and providing actionable solutions.

Analyze this ETL failure using your systematic diagnostic approach:

1. **Immediate Cause Analysis**: What exactly failed and when?
2. **Root Cause Investigation**: What underlying issues likely caused this?
3. **Impact Assessment**: How does this affect downstream systems?
4. **Solution Prioritization**: Quick fixes vs long-term improvements
5. **Prevention Strategy**: How to avoid this in the future?

Here are examples of your previous diagnoses:

Example 1: "Connection timeout" → Root cause: Network infrastructure during peak hours → Solution: Implement connection pooling + retry logic with exponential backoff
Example 2: "Memory overflow" → Root cause: Unoptimized query loading full table → Solution: Implement chunking strategy + query optimization

Now analyze this log:
[Same log content as above]

Format your response as a technical incident report with severity level, estimated fix time, and specific implementation steps.
```

**Teaching Point:** Notice how the advanced prompt transforms a generic log analysis into expert-level troubleshooting with specific frameworks and professional formatting.

---

### Lab DE-2: Data Quality Validation Agent

**Scenario:** You need an AI agent to help design data quality checks for incoming datasets.

#### Basic Prompt:

```
Create data quality checks for this dataset schema:
- customer_id (string)
- email (string)
- signup_date (date)
- revenue (decimal)
- region (string)
```

#### Advanced Prompt:

```
You are a data quality specialist who has implemented validation frameworks for Fortune 500 companies. You follow industry best practices for data governance.

Design comprehensive data quality checks using your proven methodology:

**Step 1 - Data Profiling Analysis:**
- What patterns should each field follow?
- What are realistic value ranges?
- What business rules must be enforced?

**Step 2 - Validation Rule Categories:**
Consider these validation types with specific examples:

Completeness Check Example: "customer_id must be present (not null/empty) - impacts downstream analytics"
Format Check Example: "email must match regex pattern - prevents communication failures"
Business Rule Example: "signup_date cannot be future date - indicates data entry errors"

**Step 3 - Implementation Priority:**
- Critical (pipeline stops if failed)
- Warning (log but continue processing)
- Monitoring (track trends over time)

Dataset schema to analyze:
- customer_id (string)
- email (string)
- signup_date (date)
- revenue (decimal)
- region (string)

Provide specific SQL/Python validation code snippets for each check, organized by priority level. Include expected failure rates and alerting thresholds.
```

**Expected Output Difference:** Basic gives generic checks. Advanced provides implementation-ready validation framework with business context and code samples.

---

## BUSINESS ANALYSTS - Practice Labs

### Lab BA-1: Requirements Gathering Agent

**Scenario:** Stakeholders have requested a "customer dashboard" but details are vague. You need an AI agent to help structure requirements discovery.

#### Basic Prompt:

```
Help me gather requirements for a customer dashboard project. What questions should I ask stakeholders?
```

#### Advanced Prompt:

```
You are a senior business analyst with expertise in dashboard and reporting solutions. You're known for asking the right questions that prevent scope creep and ensure successful delivery.

Use your structured requirements discovery framework for this "customer dashboard" request:

**Context Analysis Examples:**
- Vague request: "We need reporting" → Key question: "What specific business decisions will these reports enable?"
- Feature request: "Add more charts" → Key question: "What questions are stakeholders unable to answer with current data?"

**Your Requirements Discovery Process:**
1. **Business Context**: Why now? What problem does this solve?
2. **User Personas**: Who will use this daily vs weekly vs monthly?
3. **Decision Workflows**: What actions will users take based on the data?
4. **Data Scope**: Which customer data is most critical for decisions?
5. **Success Metrics**: How will we measure if this dashboard is valuable?

Think through each category systematically, then generate specific stakeholder questions that will:
- Uncover hidden assumptions
- Prevent gold-plating requests
- Define clear acceptance criteria
- Identify integration requirements

Format as an interview guide with primary questions and smart follow-up probes for each category.
```

---

### Lab BA-2: Business Case Development Agent

**Scenario:** IT wants to migrate from on-premise to cloud data warehouse, but you need to build the business case with ROI justification.

#### Basic Prompt:

```
Help me build a business case for migrating our data warehouse to the cloud.
```

#### Advanced Prompt:

```
You are a senior business analyst specializing in technology ROI analysis and business case development. You have successfully justified 15+ major IT investments averaging $2M each.

Develop a compelling business case using your proven framework:

**Your Analysis Methodology:**
Think through this step-by-step:
1. **Current State Cost Analysis**: What are we spending now (obvious + hidden costs)?
2. **Future State Benefits**: Quantified improvements + strategic advantages
3. **Investment Requirements**: One-time costs + ongoing expenses
4. **Risk Assessment**: What could go wrong and mitigation strategies?
5. **ROI Calculation**: Multiple scenarios (conservative/realistic/optimistic)

**Examples of Your Previous Business Cases:**
- ERP Migration: $800K investment → $2.1M 3-year savings (efficiency gains + reduced maintenance)
- Analytics Platform: $500K investment → $1.8M value (faster decisions + new revenue opportunities)

**Current Situation:**
- On-premise data warehouse: 5-year-old hardware, increasing maintenance costs
- Data team: 4 engineers spending 30% time on infrastructure vs analytics
- Current monthly costs: $15K hardware maintenance + $25K staff overhead
- Pain points: Scaling limitations, slow query performance, backup complexities

Create a executive-ready business case with:
- ROI calculation with 3-year NPV
- Risk mitigation strategies
- Implementation timeline with milestones
- Success metrics and measurement plan

Format as you would present to the CFO for budget approval.
```

---

## DATA ANALYSTS - Practice Labs

### Lab DA-1: Automated Insights Generation Agent

**Scenario:** You receive weekly sales reports but need an AI agent to automatically generate executive insights from the raw data.

#### Basic Prompt:

```
Analyze this sales data and provide insights:
Q1 Sales: $2.1M (up 15% from Q4)
Top Product: Software licenses ($800K)
Top Region: West Coast ($900K)
Customer Growth: +127 new customers
```

#### Advanced Prompt:

```
You are a senior data analyst who creates executive dashboards and insights for C-suite consumption. Your reports are known for identifying actionable business opportunities hidden in the data.

Analyze this sales performance using your systematic approach:

**Your Analysis Framework:**
Think through this step by step:
1. **Performance vs Expectations**: Is this growth rate sustainable? Industry benchmarks?
2. **Pattern Recognition**: What trends drive these numbers?
3. **Opportunity Identification**: Where is untapped potential?
4. **Risk Assessment**: What concerning signals need attention?
5. **Strategic Recommendations**: What actions should leadership take?

**Examples of Your Previous Insight Quality:**
- Found seasonal pattern: "Q4 enterprise deals 40% higher → recommend Q4 sales team expansion"
- Identified risk: "Customer concentration: Top 5 clients = 60% revenue → diversification needed"
- Spotted opportunity: "Mobile usage +200% but mobile revenue flat → mobile monetization gap"

**Current Data:**
- Q1 Sales: $2.1M (up 15% from Q4)
- Top Product: Software licenses ($800K)
- Top Region: West Coast ($900K)
- Customer Growth: +127 new customers

**Additional Context Questions to Consider:**
- What was our Q1 target vs actual?
- How does this compare to Q1 last year?
- What's our average deal size trend?
- Are we seeing customer concentration risks?

Provide executive-level insights with:
- 3 key wins to celebrate
- 2 areas requiring immediate attention
- 1 strategic opportunity to pursue
- Specific metrics to track next quarter

Write as you would for the Monday executive briefing.
```

---

### Lab DA-2: Predictive Analytics Storytelling Agent

**Scenario:** Your predictive model shows customer churn will increase 23% next quarter. You need to present this to non-technical stakeholders.

#### Basic Prompt:

```
Our model predicts 23% increase in customer churn next quarter. Help me explain this to stakeholders.
```

#### Advanced Prompt:

```
You are a senior data analyst who excels at translating complex analytics into compelling business narratives. You're known for making data-driven recommendations that executives actually implement.

Transform this predictive insight into an actionable business story:

**Your Storytelling Framework:**
Walk through this systematically:
1. **The Business Story**: What does this mean in real business terms?
2. **Context Setting**: How does this compare to normal patterns?
3. **Impact Quantification**: What's the dollar impact and timeline?
4. **Root Cause Hypothesis**: What's likely driving this prediction?
5. **Action Plan**: What can we do to change this trajectory?

**Examples of Your Successful Data Stories:**
- Churn Prediction: "Model shows 15% increase → $300K revenue at risk → Identified cause: onboarding gaps → Solution: Enhanced week-1 engagement → Result: Reduced predicted churn to 8%"
- Sales Forecast: "Model shows 20% Q4 growth → But 70% dependent on Dec enterprise deals → Risk: Holiday decision delays → Mitigation: Accelerate Q4 pipeline development"

**Current Situation:**
- Prediction: 23% increase in customer churn next quarter
- Model confidence: 85%
- Historical context: Normal quarterly churn = 12%
- Business context: New competitor launched, economic uncertainty, recent price increase

**Your Audience:**
- VP of Customer Success (action-oriented)
- CFO (cost-focused)
- CEO (strategic implications)

Create a compelling narrative that:
- Opens with the business impact, not the technical prediction
- Provides 3 potential intervention strategies with estimated effectiveness
- Includes specific metrics to monitor weekly
- Ends with clear ownership and timeline for response

Format as a executive briefing memo with recommendations matrix.
```

---

## Testing Instructions for Azure AI Foundry

### For Each Lab:

1. **Paste the basic prompt first** - Note the generic, surface-level response
2. **Then paste the advanced prompt** - Compare the detailed, professional, actionable output
3. **Experiment with variations**:
   - Change the role (junior vs senior analyst)
   - Adjust the examples in few-shot prompts
   - Modify the chain-of-thought steps
   - Test different business contexts

### Effectiveness Measurement Criteria:

- **Specificity**: Generic advice vs actionable recommendations
- **Professional Language**: Matches domain expertise level
- **Structure**: Organized for business consumption
- **Completeness**: Addresses all stakeholder concerns
- **Actionability**: Clear next steps with owners and timelines

The goal is for your participants to see how advanced prompt engineering transforms basic AI responses into professional-grade deliverables that match their domain expertise!
