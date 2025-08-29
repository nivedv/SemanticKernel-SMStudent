# Apply Filters on Functions

## Nived Varma - MCT

## Exercise Overview

In this exercise, you'll consume a previous chat conversation between the user and the assistant to generate a new response. You'll learn how to apply a trust filter on function results to ensure user approval before executing sensitive operations like booking flights.

**Duration:** Approximately 10 minutes to complete

## Setup Instructions

### Step 1: Set up Azure AI Resources

1. Open the [Azure AI Foundry portal](https://ai.azure.com) in a web browser and sign in using your Azure credentials
2. Close any tips or quick start panes that appear on first sign-in
3. Navigate to the home page using the Azure AI Foundry logo at the top left
4. In the navigation pane on the left, select **Overview** to see the main page for your project

> **Note:** If an "Insufficient permissions" error is displayed, use the Fix me button to resolve it.

5. Under the **Libraries** section of the overview page, select **Azure OpenAI**
6. Keep note of your endpoint and API key - you'll need these for the next task

### Step 2: Set up Development Environment

1. Open a new browser tab and navigate to the [Azure portal](https://portal.azure.com)
2. Sign in with your Azure credentials if prompted
3. Use the `[>_]` button to create a new Cloud Shell with PowerShell environment
4. In the cloud shell toolbar Settings menu, select **Go to Classic version**
5. Clone the repository:

   ```bash
   rm -r mslearn-ai-semantic-kernel -f
   git clone https://github.com/MicrosoftLearning/mslearn-ai-semantic-kernel mslearn-ai-semantic-kernel
   ```

6. Navigate to the Python folder:
   ```bash
   cd mslearn-ai-semantic-kernel/Labfiles/04-apply-function-filters/Python
   ```

### Step 3: Install Dependencies

```bash
python -m venv labenv
./labenv/bin/Activate.ps1
pip install python-dotenv semantic-kernel[azure]
```

### Step 4: Configure API Settings

1. Edit the configuration file:

   ```bash
   code .env
   ```

2. Replace the placeholders with your actual values:

   - `your_project_endpoint` - Your Azure OpenAI endpoint
   - `your_project_api_key` - Your Azure OpenAI API key
   - `your_deployment_name` - Your gpt-4o model deployment name

3. Save the file (Ctrl+S) and quit the editor (Ctrl+Q)

## Creating Function Filters

Function filters in Semantic Kernel allow you to intercept and control function execution. In this exercise, you'll create a permission filter that requests user approval before allowing the assistant to perform sensitive operations.

### Step 1: Create the Permission Filter

1. Edit the filter file:

   ```bash
   code filters.py
   ```

2. Create the function filter class:

   ```python
   # Create the function filter class
   async def permission_filter(context: FunctionInvocationContext,
   next: Callable[[FunctionInvocationContext], Awaitable[None]]) -> None:
   ```

3. Implement the function invocation method:
   ```python
   # Implement the function invocation method
   if not has_user_permission(context.function.plugin_name, context.function.name):
       context.result = "The operation was not approved by the user"
       return
   await next(context)
   ```

### Step 2: Register the Filter with the Kernel

Add the permission filter to your kernel:

```python
# Add the permission filter to the kernel
kernel.add_filter('function_invocation', permission_filter)
```

### Step 3: Test the Filter Implementation

1. Save your changes (Ctrl+S)

2. Run the application:

   ```bash
   python filters.py
   ```

3. Test the filter by trying to book a flight and denying approval:

   **Expected Interaction:**

   ```
   User: Find me a flight to Tokyo on January 19
   Assistant: I found a flight to Tokyo on the 19th of January. The flight is with Air Japan and the price is $1200.

   User: Can you book this flight for me?
   System Message: The agent requires an approval to complete this operation. Do you approve (Y/N)
   User: N
   Assistant: I'm sorry, but I am unable to book the flight for you.
   ```

## Understanding Function Filters

### What are Function Filters?

Function filters are middleware components that intercept function calls before they're executed. They provide a way to:

- **Control Access:** Ensure certain operations require user approval
- **Validate Input:** Check parameters before function execution
- **Log Activity:** Track function usage and performance
- **Handle Errors:** Provide custom error handling and recovery

### Key Components

1. **FunctionInvocationContext:** Contains information about the function being called
2. **Permission Checking:** Validates whether the user has authorized the operation
3. **Filter Chain:** Allows multiple filters to be applied in sequence
4. **Context Result:** Provides a way to modify or block function execution

### Filter Flow

1. **Function Call Initiated:** AI agent attempts to call a function
2. **Filter Intercepts:** Permission filter checks for user approval
3. **Permission Request:** System prompts user for approval if needed
4. **Decision Point:**
   - If approved: Function continues execution
   - If denied: Function execution is blocked with custom message

### Security Benefits

- **User Control:** Ensures sensitive operations require explicit user consent
- **Audit Trail:** Provides logging of approved/denied operations
- **Risk Mitigation:** Prevents unauthorized actions by AI agents
- **Trust Building:** Increases user confidence in AI system behavior

## Best Practices

1. **Clear Messaging:** Provide informative messages when operations are blocked
2. **Granular Control:** Apply filters only to sensitive functions that require approval
3. **User Experience:** Make approval requests clear and specific about the action
4. **Error Handling:** Gracefully handle both approved and denied scenarios
5. **Logging:** Track filter decisions for auditing and improvement

## Advanced Filter Scenarios

Function filters can be extended for various use cases:

- **Rate Limiting:** Control frequency of function calls
- **Content Filtering:** Validate input parameters for appropriateness
- **Cost Management:** Monitor and limit expensive operations
- **Compliance:** Ensure operations meet regulatory requirements
- **Performance Monitoring:** Track function execution times and success rates

## Cleanup

When you've finished exploring, remember to delete the Azure resources you created to avoid unnecessary costs:

1. Navigate to the [Azure portal](https://portal.azure.com)
2. Find the resource group containing your resources
3. Delete the resource group and all associated resources

## Conclusion

You have successfully implemented a function filter that provides user control over AI agent actions. This security layer ensures that sensitive operations like booking flights require explicit user approval, building trust and maintaining control in AI-powered applications. Function filters are essential components for production AI systems where user safety and control are paramount.
