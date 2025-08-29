# Run Prompts with Semantic Kernel

## Nived Varma / Microsoft Certified Trainer

## Exercise Overview

In this exercise, you'll use Semantic Kernel to create an AI assistant that can provide career advice using prompt templates. You'll learn how to build and run different types of prompt templates including Semantic Kernel templates and Handlebars templates.

**Duration:** Approximately 15 minutes to complete

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
   cd mslearn-ai-semantic-kernel/Labfiles/02-run-prompts/Python
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

## Creating and Running Prompt Templates

### Step 1: Set up the Kernel and Chat Components

1. Edit the main program file:

   ```bash
   code prompts.py
   ```

2. Add the kernel setup code:

   ```python
   # Create a kernel with Azure OpenAI chat completion
   kernel = Kernel()
   chat_completion = AzureChatCompletion(
       api_key=api_key,
       endpoint=endpoint,
       deployment_name=deployment_name
   )
   kernel.add_service(chat_completion)
   ```

3. Add chat history initialization:

   ```python
   # Create the chat history
   chat_history = ChatHistory()
   ```

4. Add the reply handling function:
   ```python
   # Get the reply from the chat completion service
   reply = await chat_completion.get_chat_message_content(
       chat_history=chat_history,
       kernel=kernel,
       settings=AzureChatPromptExecutionSettings()
   )
   print("Assistant:", reply)
   chat_history.add_assistant_message(str(reply))
   ```

### Step 2: Create a Semantic Kernel Prompt Template

This template will provide career role recommendations in JSON format.

```python
# Create a semantic kernel prompt template
sk_prompt_template = KernelPromptTemplate(
    prompt_template_config=PromptTemplateConfig(
        template="""
You are a helpful career advisor. Based on the users's skills and interest, suggest up to 5 suitable roles.
Return the output as JSON in the following format:
"Role Recommendations":
{
"recommendedRoles": [],
"industries": [],
"estimatedSalaryRange": ""
}
My skills are: {{$skills}}. My interests are: {{$interests}}. What are some roles that would be suitable for me?
""",
        name="recommend_roles_prompt",
        template_format="semantic-kernel",
    )
)
```

### Step 3: Render and Execute the Semantic Kernel Prompt

```python
# Render the Semantic Kernel prompt with arguments
sk_rendered_prompt = await sk_prompt_template.render(
    kernel,
    KernelArguments(
        skills="Software Engineering, C#, Python, Drawing, Guitar, Dance",
        interests="Education, Psychology, Programming, Helping Others"
    )
)

# Add the Semantic Kernel prompt to the chat history and get the reply
chat_history.add_user_message(sk_rendered_prompt)
await get_reply()
```

### Step 4: Create a Handlebars Template for Skill Gap Analysis

This template will analyze skill gaps and recommend courses.

```python
# Create a handlebars template
hb_prompt_template = HandlebarsPromptTemplate(
    prompt_template_config=PromptTemplateConfig(
        template="""
<message role="system">
Instructions: You are a career advisor. Analyze the skill gap between
the user's current skills and the requirements of the target role.
</message>
<message role="user">Target Role: {{targetRole}}</message>
<message role="user">Current Skills: {{currentSkills}}</message>
<message role="assistant">
"Skill Gap Analysis":
{
"missingSkills": [],
"coursesToTake": [],
"certificationSuggestions": []
}
</message>
""",
        name="missing_skills_prompt",
        template_format="handlebars",
    )
)
```

### Step 5: Render and Execute the Handlebars Prompt

```python
# Render the Handlebars prompt with arguments
hb_rendered_prompt = await hb_prompt_template.render(
    kernel,
    KernelArguments(
        targetRole="Game Developer",
        currentSkills="Software Engineering, C#, Python, Drawing, Guitar, Dance"
    )
)

# Add the Handlebars prompt to the chat history and get the reply
chat_history.add_user_message(hb_rendered_prompt)
await get_reply()
```

### Step 6: Add Interactive User Input

```python
# Get a follow-up prompt from the user
print("Assistant: How can I help you?")
user_input = input("User: ")

# Add the user input to the chat history and get the reply
chat_history.add_user_message(user_input)
await get_reply()
```

### Step 7: Test the Application

1. Save your changes (Ctrl+S)

2. Run the application:

   ```bash
   python prompts.py
   ```

3. Expected output for the first prompt (role recommendations):

   ```json
   Assistant: "Role Recommendations":
   {
   "recommendedRoles": [
       "Educational Software Developer",
       "Psychology-Based Game Designer",
       "Learning Management System Specialist",
       "Technology Trainer/Instructor",
       "Creative Programmer for Arts-Based Applications"
   ],
   "industries": [
       "Education Technology",
       "Game Development and Psychology",
       "Software Development",
       "Corporate Training",
       "Creative Arts Technology"
   ],
   "estimatedSalaryRange": "$55,000 - $120,000 per year (depending on experience and role)"
   }
   ```

4. The application will then show skill gap analysis and prompt for user input:

   ```
   Assistant: How can I help you?
   User: How long will it take to gain the required skills?
   ```

5. Expected follow-up response:
   ```json
   "Skill Acquisition Estimate":
   {
   "estimatedTime": {
       "Unity/Unreal Engine proficiency": "3-6 months (focused learning and project-based practice)",
       "Game mechanics and physics programming": "2-4 months (depending on your familiarity with algorithms and physics concepts)",
       "3D modeling/animation skills": "4-6 months (if learning beginner-to-intermediate-level modeling tools like Blender or Maya)",
       "Level design and storytelling techniques": "2-3 months (with regular game project work and creative design exercises)",
       "Version control systems like Git": "1 month (trial-and-error practice with collaborative coding workflows)"
   },
   "totalEstimatedTime": "9-18 months (if pursued part-time with a consistent learning schedule)"
   }
   ```

## Key Concepts

### Semantic Kernel vs Handlebars Templates

- **Semantic Kernel Templates:** Use `{{$variableName}}` syntax for variable substitution
- **Handlebars Templates:** Use `{{variableName}}` syntax and support more complex logic and formatting
- **Template Formats:** Different template engines provide various capabilities for prompt construction

### Template Configuration

- **PromptTemplateConfig:** Defines the template structure, format, and metadata
- **KernelArguments:** Provides variable values to be substituted in templates
- **Template Rendering:** Converts templates with variables into executable prompts

### Chat History Management

- **ChatHistory:** Maintains conversation context across multiple interactions
- **Message Types:** System, user, and assistant messages each serve different purposes
- **Context Preservation:** Enables coherent multi-turn conversations

## Best Practices

1. **Template Design:** Structure prompts clearly with specific output format requirements
2. **Variable Naming:** Use descriptive variable names that match your data structure
3. **Error Handling:** Always handle potential rendering and execution errors
4. **Context Management:** Maintain appropriate chat history for conversation flow

## Cleanup

When you've finished exploring, remember to delete the Azure resources you created to avoid unnecessary costs:

1. Navigate to the [Azure portal](https://portal.azure.com)
2. Find the resource group containing your resources
3. Delete the resource group and all associated resources

## Conclusion

You have successfully created prompt templates using both Semantic Kernel and Handlebars formats, learned how to render templates with dynamic arguments, and built an interactive AI assistant that maintains conversation context. This foundation prepares you for more advanced prompt engineering and multi-agent scenarios.
