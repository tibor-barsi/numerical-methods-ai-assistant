# Numerične metode — AI Assistant (Mistral Le Chat)

Custom AI assistant for the **Numerične metode (Numerical Methods)** course at the Faculty of Mechanical Engineering, University of Ljubljana, developed by [LADISK](http://ladisk.si/).

The assistant runs on [Mistral Le Chat](https://le.chat) using a custom system prompt and is intended as a study aid for students — not a replacement for lectures, textbooks, or office hours.

---

## What it does

- Answers questions about the course curriculum (14 lectures, Python to numerical ODEs)
- Guides students through topics using the same terminology, notation, and code conventions as the course
- Refuses to solve homework or exam problems — guides students to the answer instead
- Can discuss numerical methods and Python topics outside the curriculum, with a clear disclaimer
- Declines unrelated topics entirely

## Two modes

**Free help** — student asks a specific question and gets targeted help.

**Guided curriculum** — the assistant walks the student step by step through all 14 lectures in order, checks understanding at each stage, and adapts to the student's pace.

## Progress tracking

In guided mode, the assistant can generate a **progress file** (`.md`) at the end of each session. The file records completed lectures, current position, what the student understood well, and what needs revisiting. Uploading the file at the start of the next session lets the assistant resume exactly where the student left off.

## Setup

1. Open [le.chat](https://le.chat) and create a new Agent
2. Paste the contents of [`system_prompt.md`](./system_prompt.md) into the system prompt field
3. Choose a model (Mistral Large recommended, Mistral Small for lighter usage)
4. Save and share the agent link with students

## Course resources

- Online textbook: [jankoslavic.github.io/pynm](https://jankoslavic.github.io/pynm)
- Course repository: [github.com/jankoslavic/pynm](https://github.com/jankoslavic/pynm)
