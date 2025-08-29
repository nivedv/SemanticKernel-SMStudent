# Weekend Capstone Project: AI-Powered Personal Finance Assistant

## Nived Varma - Microsoft Certified Trainer.

## Project Overview

**Duration:** 2-3 hours  
**Scenario:** Build an AI-powered personal finance assistant that helps users analyze their spending patterns, create budgets, and make investment recommendations while ensuring user approval for any financial transactions.

**Business Context:** You're developing a prototype for a fintech startup that wants to provide personalized financial advice through conversational AI. The system must be trustworthy, require user consent for financial actions, and provide structured, actionable insights.

## Learning Objectives Applied

This project integrates all the skills you've learned:

- **Prompt Engineering** (Lab 2): Create structured prompts for financial analysis and recommendations
- **Custom Plugin Development** (Lab 3): Build financial calculation and recommendation functions
- **Security Filters** (Lab 4): Implement approval mechanisms for sensitive financial operations

## Project Requirements

### Core Features to Implement

1. **Expense Analysis Engine**

   - Analyze spending patterns from sample transaction data
   - Categorize expenses and identify trends
   - Generate structured insights in JSON format

2. **Budget Recommendation System**

   - Create personalized budget recommendations based on income and spending
   - Suggest cost-saving opportunities
   - Calculate potential savings

3. **Investment Advisory Plugin**

   - Recommend investment strategies based on user profile
   - Calculate potential returns and risk assessments
   - Provide investment portfolio suggestions

4. **Security and Approval System**
   - Implement filters for financial transactions
   - Require user approval for investment recommendations
   - Block unauthorized financial operations

## Technical Implementation

### Phase 1: Setup and Data Preparation (30 minutes)

1. **Environment Setup**

   ```bash
   mkdir finance-assistant-capstone
   cd finance-assistant-capstone
   python -m venv capstone-env
   ./capstone-env/Scripts/Activate
   pip install python-dotenv semantic-kernel[azure]
   ```

2. **Create Sample Data Files**

   Create `transactions.json`:

   ```json
   {
     "transactions": [
       {
         "date": "2024-12-01",
         "amount": 85.5,
         "category": "Groceries",
         "description": "Whole Foods"
       },
       {
         "date": "2024-12-02",
         "amount": 1200.0,
         "category": "Rent",
         "description": "Monthly Rent"
       },
       {
         "date": "2024-12-03",
         "amount": 45.75,
         "category": "Dining",
         "description": "Restaurant dinner"
       },
       {
         "date": "2024-12-04",
         "amount": 120.0,
         "category": "Utilities",
         "description": "Electric bill"
       },
       {
         "date": "2024-12-05",
         "amount": 35.2,
         "category": "Transportation",
         "description": "Gas station"
       },
       {
         "date": "2024-12-06",
         "amount": 89.99,
         "category": "Entertainment",
         "description": "Streaming services"
       },
       {
         "date": "2024-12-07",
         "amount": 250.0,
         "category": "Shopping",
         "description": "Clothing store"
       },
       {
         "date": "2024-12-08",
         "amount": 75.4,
         "category": "Groceries",
         "description": "Supermarket"
       },
       {
         "date": "2024-12-09",
         "amount": 180.0,
         "category": "Healthcare",
         "description": "Doctor visit"
       },
       {
         "date": "2024-12-10",
         "amount": 95.5,
         "category": "Dining",
         "description": "Weekend brunch"
       }
     ],
     "monthly_income": 4500.0
   }
   ```

   Create `user_profile.json`:

   ```json
   {
     "age": 28,
     "risk_tolerance": "moderate",
     "financial_goals": ["emergency_fund", "retirement", "house_down_payment"],
     "current_savings": 15000.0,
     "monthly_income": 4500.0,
     "financial_experience": "beginner"
   }
   ```

### Phase 2: Core Plugin Development (60 minutes)

Create `finance_plugin.py`:

```python
import json
from semantic_kernel.functions import kernel_function
from typing import List, Dict, Any

class FinanceAnalysisPlugin:
    def __init__(self):
        with open('transactions.json', 'r') as f:
            self.financial_data = json.load(f)
        with open('user_profile.json', 'r') as f:
            self.user_profile = json.load(f)

    @kernel_function(description="Analyzes spending patterns and returns categorized expense breakdown in JSON format")
    def analyze_expenses(self, time_period: str = "monthly") -> str:
        """Analyze user's spending patterns by category"""
        # TODO: Implement expense categorization and analysis
        # Calculate total spending by category
        # Identify top spending categories
        # Calculate percentage of income spent
        # Return structured JSON response
        pass

    @kernel_function(description="Creates personalized budget recommendations based on income and spending patterns")
    def create_budget_plan(self, savings_goal: str = "20") -> str:
        """Generate budget recommendations with spending limits"""
        # TODO: Implement budget creation logic
        # Calculate recommended spending limits by category
        # Suggest areas for cost reduction
        # Include emergency fund recommendations
        # Return structured JSON with budget allocations
        pass

    @kernel_function(description="Provides investment recommendations based on user profile and risk tolerance")
    def recommend_investments(self, investment_amount: str) -> str:
        """Generate investment recommendations based on user profile"""
        # TODO: Implement investment recommendation logic
        # Consider user's risk tolerance and goals
        # Calculate potential returns for different strategies
        # Provide diversified portfolio suggestions
        # Return structured JSON with investment options
        pass

    @kernel_function(description="SENSITIVE: Executes investment transactions - requires user approval")
    def execute_investment(self, investment_type: str, amount: str) -> str:
        """Execute an investment transaction (requires approval filter)"""
        # TODO: Implement investment execution logic
        # This function should be protected by approval filter
        # Log the transaction
        # Return confirmation message
        pass
```

### Phase 3: Prompt Templates Implementation (45 minutes)

Create `finance_prompts.py`:

```python
from semantic_kernel.prompt_template import PromptTemplateConfig, KernelPromptTemplate
from semantic_kernel.prompt_template.handlebars import HandlebarsPromptTemplate

class FinancePrompts:
    @staticmethod
    def get_expense_analysis_template():
        """Semantic Kernel template for expense analysis"""
        return KernelPromptTemplate(
            prompt_template_config=PromptTemplateConfig(
                template="""
You are a professional financial advisor. Analyze the provided expense data and create insights.

Expense Data: {{$expense_data}}
Monthly Income: {{$monthly_income}}

Provide analysis in this JSON format:
{
  "spending_summary": {
    "total_expenses": 0,
    "top_categories": [],
    "percentage_of_income": 0
  },
  "insights": {
    "highest_spending_category": "",
    "potential_savings_areas": [],
    "spending_patterns": ""
  },
  "recommendations": []
}
""",
                name="expense_analysis_prompt",
                template_format="semantic-kernel"
            )
        )

    @staticmethod
    def get_investment_advice_template():
        """Handlebars template for investment recommendations"""
        return HandlebarsPromptTemplate(
            prompt_template_config=PromptTemplateConfig(
                template="""
<message role="system">
You are a certified financial planner. Provide investment recommendations based on the user's profile.
</message>
<message role="user">
User Profile: {{userProfile}}
Available Investment Amount: ${{investmentAmount}}
</message>
<message role="assistant">
{
  "investment_recommendations": {
    "recommended_allocation": {},
    "expected_annual_return": "",
    "risk_level": "{{riskTolerance}}",
    "investment_options": []
  },
  "timeline_recommendations": {
    "short_term": "",
    "medium_term": "",
    "long_term": ""
  }
}
</message>
""",
                name="investment_advice_prompt",
                template_format="handlebars"
            )
        )
```

### Phase 4: Security Filter Implementation (30 minutes)

Create `finance_filters.py`:

```python
from semantic_kernel.functions import FunctionInvocationContext
from typing import Callable, Awaitable

async def financial_approval_filter(context: FunctionInvocationContext,
                                  next: Callable[[FunctionInvocationContext], Awaitable[None]]) -> None:
    """Filter that requires approval for sensitive financial operations"""

    sensitive_functions = ["execute_investment", "transfer_funds", "make_payment"]

    if context.function.name in sensitive_functions:
        # TODO: Implement approval logic
        # Check if function requires user approval
        # Display operation details to user
        # Request user confirmation
        # Block execution if not approved
        pass

    await next(context)

def has_user_permission(plugin_name: str, function_name: str) -> bool:
    """Check if user has given permission for the operation"""
    # TODO: Implement permission checking
    # Display clear information about the operation
    # Request user input (Y/N)
    # Return boolean based on user response
    pass
```

### Phase 5: Main Application Integration (30 minutes)

Create `main.py`:

```python
import asyncio
from semantic_kernel import Kernel
from semantic_kernel.connectors.ai.azure.chat_completion import AzureChatCompletion
from semantic_kernel.prompt_template.input_variable import InputVariable
from semantic_kernel.contents.chat_history import ChatHistory
from finance_plugin import FinanceAnalysisPlugin
from finance_prompts import FinancePrompts
from finance_filters import financial_approval_filter

async def main():
    # TODO: Initialize kernel with Azure OpenAI
    # TODO: Add finance plugin to kernel
    # TODO: Register security filter
    # TODO: Create chat history
    # TODO: Implement interactive conversation loop

    print("🏦 Welcome to your AI Financial Assistant!")
    print("I can help you analyze expenses, create budgets, and provide investment advice.")
    print("Type 'exit' to quit.\n")

    while True:
        user_input = input("You: ")
        if user_input.lower() == 'exit':
            break

        # TODO: Process user input and generate response
        # TODO: Handle different types of financial queries
        # TODO: Apply appropriate templates and functions

    print("Thank you for using the AI Financial Assistant!")

if __name__ == "__main__":
    asyncio.run(main())
```

## Implementation Guidelines

### Required Functionality

1. **Expense Analysis**

   - Load and parse transaction data
   - Calculate spending by category
   - Identify trends and insights
   - Generate structured JSON responses

2. **Budget Creation**

   - Analyze income vs expenses
   - Recommend spending limits
   - Suggest savings strategies
   - Create actionable budget plans

3. **Investment Recommendations**

   - Consider user's risk profile
   - Suggest diversified portfolios
   - Calculate potential returns
   - Provide educational insights

4. **Security Controls**
   - Filter sensitive operations
   - Request user approval
   - Log financial decisions
   - Protect against unauthorized actions

### Testing Scenarios

Test your implementation with these scenarios:

1. **Expense Analysis Query:**

   - "Analyze my spending patterns from last month"
   - "What are my top expense categories?"

2. **Budget Planning Query:**

   - "Help me create a monthly budget"
   - "How can I save more money?"

3. **Investment Advisory Query:**

   - "I have $5000 to invest, what do you recommend?"
   - "Should I invest in stocks or bonds?"

4. **Security Testing:**
   - Try to execute an investment and approve/deny it
   - Verify that sensitive operations require permission

## Deliverables

Create a project folder containing:

1. **Core Files:**

   - `main.py` - Main application entry point
   - `finance_plugin.py` - Financial analysis functions
   - `finance_prompts.py` - Prompt templates
   - `finance_filters.py` - Security filters
   - `.env` - Configuration file

2. **Data Files:**

   - `transactions.json` - Sample transaction data
   - `user_profile.json` - User financial profile

3. **Documentation:**
   - `README.md` - Setup and usage instructions
   - `TESTING.md` - Testing scenarios and expected outputs

## Bonus Challenges (Optional)

If you complete the core project early, try these enhancements:

1. **Enhanced Data Analysis:**

   - Add monthly comparison features
   - Implement spending trend predictions
   - Create visual spending reports (text-based)

2. **Advanced Security:**

   - Implement transaction limits
   - Add multi-factor approval for large investments
   - Create audit logging

3. **Personalization:**
   - Add user preference learning
   - Implement dynamic risk assessment
   - Create personalized financial tips

## Success Criteria

Your project is successful if it:

✅ **Integrates all three lab concepts** (prompts, plugins, filters)  
✅ **Provides structured financial insights** using JSON responses  
✅ **Implements security controls** for sensitive operations  
✅ **Handles interactive conversations** with appropriate context  
✅ **Uses different template types** (Semantic Kernel and Handlebars)  
✅ **Demonstrates practical financial use cases** relevant to real users

## Submission Guidelines

1. **Code Quality:** Clean, commented code following Python best practices
2. **Functionality:** All core features working as specified
3. **Documentation:** Clear README with setup and usage instructions
4. **Testing:** Evidence of testing with the provided scenarios
5. **Security:** Proper implementation of approval filters

Good luck building your AI Financial Assistant! This project will demonstrate your mastery of Semantic Kernel fundamentals while creating something genuinely useful for personal finance management.

## Support Resources

- Review the three lab exercises for reference implementations
- Use the provided data files as starting points
- Focus on functionality first, then optimize
- Test incrementally as you build each component

Remember: The goal is to apply what you've learned in a new context, not to build a production-ready financial system. Focus on demonstrating the core Semantic Kernel concepts through a compelling use case!
