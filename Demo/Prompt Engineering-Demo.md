# Advanced Prompt Engineering: From Basic to Brilliant

## What is Advanced Prompt Engineering?

Think of advanced prompt engineering as **upgrading from giving basic instructions to having strategic conversations** with AI. Instead of saying "write an email," you're saying "write an email as a senior consultant would, considering these three examples of successful client communications, walking through your reasoning step by step."

---

## 1. Zero-Shot vs Few-Shot Prompting

### Zero-Shot Prompting

**What it is:** Asking the AI to perform a task without any examples - relying purely on its training.

**Azure AI Foundry Example:**

```
Classify this customer feedback as positive, negative, or neutral:
"The product arrived late but the quality exceeded my expectations."
```

### Few-Shot Prompting

**What it is:** Providing 2-5 examples to teach the AI the exact pattern you want.

**Azure AI Foundry Example:**

```
Classify customer feedback as positive, negative, or neutral. Here are examples:

Example 1: "Fast delivery and great quality!" → Positive
Example 2: "Product broke after one day" → Negative
Example 3: "It's okay, nothing special" → Neutral

Now classify: "The product arrived late but the quality exceeded my expectations."
```

**Why Few-Shot is Powerful:** The AI learns your exact classification style and criteria from the examples.

---

## 2. Chain-of-Thought Prompting

### Basic Approach

**What it is:** Teaching the AI to "show its work" by thinking through problems step-by-step.

**Azure AI Foundry Example - Without Chain-of-Thought:**

```
A company has 120 employees. If 30% work remotely and 25% of remote workers are managers, how many remote managers are there?
```

**Azure AI Foundry Example - With Chain-of-Thought:**

```
A company has 120 employees. If 30% work remotely and 25% of remote workers are managers, how many remote managers are there?

Think through this step by step:
1. First, calculate the number of remote workers
2. Then, calculate how many of those are managers
3. Show your calculation for each step
```

### Advanced Chain-of-Thought for Business Analysis

**Azure AI Foundry Example:**

```
Analyze this business scenario and recommend next steps. Think through this systematically:

Scenario: Our SaaS startup has 1,000 users, 15% conversion rate, $50 monthly subscription, but 40% churn rate.

Please:
1. Identify the core problems
2. Prioritize which issue to solve first
3. Suggest 3 specific action items
4. Explain your reasoning for each step
```

---

## 3. Role-Based Prompting for Agents

### Basic Role Assignment

**Azure AI Foundry Example:**

```
You are a senior financial analyst with 10 years of experience at Big 4 consulting firms.
Analyze this financial data and provide recommendations in the professional tone and format you would use for a client presentation.

[Include your financial data here]
```

### Advanced Multi-Role Agent Design

**Azure AI Foundry Example - Due Diligence Agent:**

```
You are a senior due diligence specialist at KPMG with expertise in:
- Financial analysis and risk assessment
- Regulatory compliance evaluation
- Market analysis and competitive positioning

Your task: Review the attached company documents and provide a comprehensive due diligence summary.

Format your response as:
1. Executive Summary (2-3 key findings)
2. Financial Health Assessment
3. Risk Factors (High/Medium/Low categories)
4. Market Position Analysis
5. Recommendations (Go/No-Go with reasoning)

Use the analytical rigor and professional language expected in a partner-level review.
```

### Role-Based Agent with Constraints

**Azure AI Foundry Example - Contract Review Agent:**

```
You are a legal technology specialist who reviews contracts for compliance and risk.

Your specific expertise:
- Data privacy regulations (GDPR, CCPA)
- Software licensing terms
- Service level agreements
- Liability and indemnification clauses

Rules for your analysis:
- Flag any high-risk terms in RED
- Suggest alternative language for problematic clauses
- Rate overall contract risk as LOW/MEDIUM/HIGH
- Always cite specific clause numbers in your analysis

Review the following contract section: [paste contract text]
```

---

## 4. Measuring Prompt Effectiveness

### Testing Framework for Azure AI Foundry

**Example Test Suite - Customer Service Agent:**

**Test Prompt 1 - Basic:**

```
Handle this customer complaint: "I'm furious! Your software crashed during my presentation!"
```

**Test Prompt 2 - Advanced with Role & Chain-of-Thought:**

```
You are a senior customer success manager known for turning angry customers into advocates.

Handle this customer complaint using your proven 4-step de-escalation process:
1. Acknowledge their frustration specifically
2. Take ownership without making excuses
3. Propose immediate and long-term solutions
4. Ensure they feel heard and valued

Customer complaint: "I'm furious! Your software crashed during my presentation!"

Think through each step before responding, then provide your customer-facing response.
```

### Effectiveness Metrics to Test in Azure AI Foundry:

1. **Accuracy:** Does it solve the problem correctly?
2. **Consistency:** Same quality across multiple similar inputs?
3. **Tone Appropriateness:** Matches the professional context?
4. **Completeness:** Addresses all aspects of the request?
5. **Actionability:** Provides clear next steps?

---

## Hands-On Exercise for Your Class

### Exercise: Build a Progressive Prompt Series in Azure AI Foundry

**Step 1 - Start Basic:**

```
Write a project status update email.
```

**Step 2 - Add Few-Shot Learning:**

```
Write a project status update email. Here are examples of effective updates:

Example 1: "Project Alpha - Week 3 Update: Completed user research phase (100% done), starting design phase next week. Risk: Designer availability. Mitigation: Backup designer identified."

Example 2: "Project Beta - Sprint Review: Delivered 8/10 planned features. Delayed: Payment integration (vendor API issues). Next: Focus on core checkout flow."

Now write an update for: Marketing automation project, week 5, 80% complete, budget on track, team morale high.
```

**Step 3 - Add Chain-of-Thought:**

```
You're writing a project status update email. Think through this systematically:

1. What are the key stakeholder concerns for this project type?
2. What specific metrics should you highlight?
3. How should you frame any challenges to maintain confidence?
4. What actions do you want stakeholders to take?

Then write the email for: Marketing automation project, week 5, 80% complete, budget on track, team morale high.
```

**Step 4 - Add Role-Based Enhancement:**

```
You are a senior project manager at a consulting firm, known for clear communication and proactive risk management.

Write a project status update using your systematic approach:
1. Analyze what stakeholders need to know
2. Identify the most important message
3. Structure information for executive consumption
4. Include appropriate next steps

Project details: Marketing automation project, week 5, 80% complete, budget on track, team morale high.

Use your professional email format with clear subject line, executive summary, and action items.
```

---

## Pro Tips for Azure AI Foundry Testing

1. **Test with the Same Input:** Use identical scenarios to compare prompt versions
2. **Use the Temperature Setting:** Lower (0.3) for consistency, higher (0.7) for creativity
3. **Save Successful Prompts:** Build your own prompt library for different business scenarios
4. **Iterate Systematically:** Change one element at a time to see what improves results

## Common Business Use Cases to Practice

1. **Document Analysis:** "You are a compliance officer reviewing policies..."
2. **Data Interpretation:** "You are a business analyst explaining trends..."
3. **Client Communication:** "You are a senior consultant drafting recommendations..."
4. **Risk Assessment:** "You are a risk management expert evaluating..."

The key is progression: start simple, add examples, include reasoning, then layer in professional expertise!
