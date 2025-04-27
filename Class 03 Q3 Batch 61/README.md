#  PIAIC Batch 61 – Quarter 3, Class 03 – Karachi  
### **Instructor:** Wajahat Hussain



## 🚀 Exploring DACA Practical Setup with OpenAI Agents SDK

In today’s session, we practically explored how to set up an **AI agent** using **OpenAI Agents SDK** and **Google Colab**.  
We installed essential packages, configured the environment, and created a simple agent to understand the end-to-end flow.



## 🧠 Key Concepts Covered

- OpenAI Agents SDK
- `nest_asyncio` for Jupyter Notebook async compatibility
- API Key Handling using `userdata`
- Basic agent creation
- Function tool decoration
- Running agents asynchronously



## 🛠️ Step-by-Step Practical Walkthrough


### 1. **Installing OpenAI Agents SDK**

```python
!pip install openai-agents
```

**Explanation:**  
We use **`!pip install`** inside Colab to install external Python libraries.  
Here, `openai-agents` is installed, which allows creating AI agents with special functions, communication patterns, and handoffs easily.


### 2. **Handling Asynchronous Code in Jupyter (Nest Asyncio)**

```python
import nest_asyncio
nest_asyncio.apply()
```

**Explanation:**  
- Jupyter/Colab doesn't allow traditional `asyncio` event loops directly.  
- `nest_asyncio` **patches** the running event loop, so we can safely run asynchronous (`async/await`) code inside notebooks without errors.



### 3. **Setting Up API Key and Client Connection**

```python
import os
from openai import AsyncOpenAI
from google.colab import userdata

API_KEY = os.getenv("GEMINI_API_KEY") or userdata.get("GEMINI_API_KEY")
BASE_URL = "https://generativelanguage.googleapis.com/v1beta/openai/"

client = AsyncOpenAI(base_url=BASE_URL, api_key=API_KEY)
```

**Explanation:**  
- `userdata.get()` retrieves secret keys safely in Colab without exposing them publicly.
- `AsyncOpenAI` client helps interact with the LLM (Large Language Model) **asynchronously**.
- We are setting up the environment to call Google’s Gemini API.

> **Note:** Always keep your API keys private to avoid misuse.


### 4. **Disabling SDK Tracing (Optional)**

```python
from agents import set_tracing_disabled

set_tracing_disabled(disabled=True)
```

**Explanation:**  
- **Tracing** captures detailed logs of what an agent does.
- Here, we disable it to keep the logs clean and focus only on functionality.


### 5. **Creating a Frontend Tool Function**

```python
from agents import function_tool

@function_tool
def frontend_tool(input):
    return f"Frontend code based on input: {input}"
```

**Explanation:**  
- `@function_tool` **decorator** turns a Python function into a callable tool that an agent can use.
- **This function** simulates generating frontend code based on input. (Example: You give "Navbar" → it returns "Frontend code for Navbar".)



### 6. **Setting Up the Agent**

```python
from agents import Agent, OpenAIChatCompletionsModel

agent = Agent(
    name="frontend_expert",
    instructions="You are a frontend expert. Create frontend code as per user input.",
    model=OpenAIChatCompletionsModel(model="gemini-2.0-flash", client=client),
    tools=[frontend_tool]
)
```

**Explanation:**  
- **Agent:** An independent entity following instructions.
- **Name:** `"frontend_expert"` → identifies the agent.
- **Instructions:** What the agent should focus on (creating frontend code).
- **Model:** Gemini 2.0 is the LLM backing the agent.
- **Tools:** Functions (like `frontend_tool`) the agent can use to complete tasks.



### 7. **Running the Agent**

```python
response = await agent.run(input="Create a responsive navbar.")
print(response)
```

**Explanation:**  
- `await agent.run()` calls the agent with a task.  
- Agent processes the input using LLM + available tools and returns an answer.



## 📈 End-to-End Flow of This Practical

1. Install OpenAI Agents SDK.
2. Set up async compatibility using `nest_asyncio`.
3. Safely load API keys.
4. Create agent tools using `@function_tool`.
5. Define agent with instructions, LLM model, and tools.
6. Run the agent with an input prompt and view the output.



## 📚 Important Theoretical Concepts

| Term | Explanation |
|:----|:------------|
| `nest_asyncio` | Allows using `asyncio` inside Jupyter notebooks safely. |
| API Key | Secret key used to access LLM APIs securely. |
| `function_tool` | Decorator that exposes Python functions as tools for agents. |
| Agent | An AI program that performs tasks independently using LLM + tools. |
| Asynchronous Programming | Allows writing non-blocking code with `async/await`. |
| OpenAIChatCompletionsModel | A wrapper class to define which LLM model the agent should use. |


## 🎯 Conclusion

Today's class provided an **essential hands-on foundation** for building and running an **AI Agent** using **OpenAI Agents SDK** with **Google Colab**.  
We learned:
- How to install and configure the development environment
- How to create agent tools
- How to define, run, and get outputs from an agent

Mastering these basics will allow you to create **more advanced agents** in upcoming classes, leading toward **multi-agent AI systems** and **enterprise-grade deployment**.
