
# Personal Productivity Agent

This project implements a multi‑agent productivity system designed to collect tasks, prioritize them, generate a daily schedule, produce reminders, and evaluate the output. The system is built using Python, a modular agent architecture, and a custom Gradio interface.


# Course & Submission

Course: 5-Day AI Agents Intensive Course with Google
Capstone Writeup:
https://www.kaggle.com/competitions/agents-intensive-capstone-project/writeups/personal-productivity-agent

---

# 1. Architecture Overview

Below is the complete architecture and workflow layout of the system.
![Personal Productivity Agent Architecture]

<img width="1801" height="1618" alt="Agent_flow" src="https://github.com/user-attachments/assets/62c6d3e7-c531-41a1-8bcb-113eafa6fce4" />
<img width="1801" height="1618" alt="mermaid-diagram-2025-12-01-111708" src="https://github.com/user-attachments/assets/ff911fc4-170d-4288-a3ef-d5a01bbe5310" />


---

# 2. Detailed Flow

```
User Input (Text or JSON)
        │
        ▼
Collector Agent
 - Parses text
 - Extracts durations & priorities
 - Normalizes fields
        │
        ▼
Priority Agent
 - Scores tasks (urgency, importance, effort, deadline)
 - Assigns high/medium/low priority
 - Ranks tasks
        │
        ▼
Planner Agent
 - Generates timeline starting at 09:00 (local timezone)
 - Inserts break intervals
 - Ensures time consistency
        │
        ▼
Reminder Agent
 - Generates reminder timestamps
 - Stores reminder JSON files
        │
        ▼
Reflection Agent
 - Analyzes quality, workload, balance
 - Produces improvement tips
        │
        ▼
Orchestrator
 - Collects outputs from all agents
 - Merges schedule, reminders, evaluation
 - Returns unified response to UI
        │
        ▼
Gradio UI
 - Renders schedule
 - Displays scores and reminders
 - Shows task history
```

---

# 3. Folder Structure

```
src/
 ├── agents/
 │    ├── collector_agent.py
 │    ├── priority_agent.py
 │    ├── planner_agent.py
 │    ├── reminder_agent.py
 │    ├── reflection_agent.py
 │    └── orchestrator.py
 │
 ├── evaluation/
 │    └── plan_evaluator.py
 │
 ├── memory/
 │    ├── session_manager.py
 │    ├── memory_bank.py
 │    └── context_engineer.py
 │
 ├── observability/
 │    ├── logger.py
 │    ├── metrics.py
 │    └── tracer.py
 │
 ├── tools/
 │    ├── time_estimator.py
 │    ├── habit_analyzer.py
 │    ├── search_tool.py
 │    └── mcp_tools.py
 │
 └── utils/
      └── helpers.py

ui/
 └── gradio_app.py

README.md
```

---

# 4. Key Components

### CollectorAgent

Parses raw text and JSON inputs. Extracts priority, category, duration, tags, and deadlines.
Normalizes all tasks into a standard structure.

### PriorityAgent

Scores tasks based on:

- urgency
- importance (category importance map)
- expected effort
- deadline proximity

Outputs a numerical score + adjusted final priority.

### PlannerAgent

Builds a chronological schedule:

- Default start time: 09:00 local time
- Adds break intervals
- Computes start/end times
- Ensures sequential consistency

### ReminderAgent

Generates reminder timestamps and writes them to the `reminders/` directory.

### ReflectionAgent

Runs simple pattern analysis, workload balance checks, and improvement suggestions.

### Orchestrator

Coordinates the end‑to‑end pipeline:

- Calls each agent sequentially
- Aggregates all outputs
- Handles history logging
- Returns final structured output to UI

### Gradio UI

Provides:

- Task input box
- History view
- Reminder view
- Responsive layout with mobile‑friendly navbar
- Rich schedule visualizer with cards/sections

---

<img width="1277" height="612" alt="Main Screen" src="https://github.com/user-attachments/assets/7a2d59c9-94e5-40ff-b9a7-d676fa7fdad9" />

<img width="1100" height="2012" alt="FullScreenSchedule" src="https://github.com/user-attachments/assets/fffd98e1-3f5a-48b2-94b7-81a2b6973bd7" />

<img width="1320" height="632" alt="ReminderReflect" src="https://github.com/user-attachments/assets/9b2006c7-21e9-4793-b463-5792510435aa" />


# 5. Running the Project

Install dependencies:

```
pip install -r requirements.txt
```

Start the Gradio UI:

```
python -m ui.gradio_app
```

The interface opens at:

```
http://localhost:7860/
```

---

# 6. Configuration

Create a `.env` file:

```
GOOGLE_API_KEY=your_api_key_here
```

Your system will use this key for LLM-powered agents.

---

# 7. Scheduling Logic

- Day start time: **09:00 local timezone**
- Each task is scheduled sequentially
- Breaks added after each task
  - < 60 min → 5 min break
  - 60–120 min → 15 min break
  - > 120 min → 20 min break
    >

This ensures a realistic plan with proper work‑break cycles.

---

# 8. Notes

- Timezone handling uses `tzlocal` to avoid UTC shift issues.
- Category importance mapping is customizable in `priority_agent.py`.
- UI supports both JSON and plain-English input formats.

>>>>>>
>>>>>
>>>>
>>>
>>
