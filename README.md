# Automated Content Creator Crew

A Python-based automation for creating, writing, and optimizing SEO-friendly articles using agents powered by OpenAI and CrewAI.

---

## 🚀 Overview

This project uses a crew of specialized agents (Planner, Writer, Editor) to automate the creation of high-quality, SEO-optimized articles. Agents are powered by OpenAI's language models and managed via CrewAI.

---

## 🛠️ Features

- **Content Planning:** Generates detailed, SEO-focused outlines.
- **Article Writing:** Drafts engaging, fact-checked articles based on outlines.
- **Content Editing:** Refines and optimizes articles for clarity, accuracy, and SEO compliance.

---

## 📦 Requirements

- Python 3.8+
- `crewai`
- `langchain_openai`
- OpenAI API key

---

## 🔐 API Key Setup

Set your OpenAI API key as an environment variable for security:

#### On Linux/macOS:
```bash
export OPENAI_API_KEY="your_openai_api_key_here"
```

#### On Windows (Command Prompt):
```cmd
set OPENAI_API_KEY="your_openai_api_key_here"
```

---

## ⚙️ How It Works

1. **Initialize Agents:** Content Planner, Article Writer, Content Editor.
2. **Define Tasks:** Plan outline, write article, edit & optimize.
3. **Execute Workflow:** Agents work sequentially to produce a polished article.

---

## 📝 Usage

```bash
python main_single_file.py
```

You will be prompted to enter:
- **Article Topic**
- **Requirements/Editorial Guidelines**

The tool will generate a complete article following the defined workflow.

---

## 🧑‍💻 Code Preview

```python
# Example agent setup
planner = Agent(role='Content Planner', ...)
writer = Agent(role='Article Writer', ...)
editor = Agent(role='Content Editor', ...)
# Example task execution
crew = Crew(agents=[planner, writer, editor], tasks=[...], process=Process.sequential)
result = crew.kickoff()
```

---

## 📄 Main File Structure

- **Agent definitions:** Planner, Writer, Editor
- **Task definitions:** Outline planning, article writing, editing/optimization
- **Execution logic:** User input, agent/task initialization, workflow kickoff

---

## 🤝 Contributing

Feel free to submit issues or pull requests for improvements!

---


## 👤 Author

- **mehra01**  
  [GitHub Profile](https://github.com/mehra01)

---

## 💡 Inspiration

Built to automate the process of creating high-quality, SEO-optimized content using AI agents.
