# Numerične metode — AI Assistant

Custom AI assistant for the **Numerične metode (Numerical Methods)** course at the Faculty of Mechanical Engineering, University of Ljubljana, developed by [LADISK](http://ladisk.si/).

The assistant is deployed on a self-hosted [Open WebUI](https://openwebui.com/) instance using the [Mistral API](https://mistral.ai/) and is intended as a study aid for students — not a replacement for lectures, textbooks, or office hours.

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

### Open WebUI (current deployment)

1. Deploy [Open WebUI](https://openwebui.com/) via Docker on your server
2. Add your Mistral API key under **Admin Panel → Settings → Connections**
3. Create a custom model under **Workspace → Models**, paste [`system_prompt.md`](./system_prompt.md) as the system prompt and [`model-description.md`](./model-description.md) as the description
4. Restrict users to that model only under **Admin Panel → Settings → Models**
5. Disable prompt suggestions under **Admin Panel → Settings → Interface**
6. (Optional) Attach the RAG knowledge base from [`rag_knowledge/`](./rag_knowledge/) under **Admin Panel → Knowledge**

### Authentication (Microsoft Entra ID)

To restrict access to FS students and educators only, configure Microsoft OAuth in your Open WebUI environment:

```env
MICROSOFT_CLIENT_ID=your-client-id
MICROSOFT_CLIENT_SECRET=your-client-secret
MICROSOFT_CLIENT_TENANT_ID=your-tenant-id
ENABLE_OAUTH_SIGNUP=true
```

Register the app in [Entra ID](https://portal.azure.com/) with redirect URI `https://your-server/oauth/oidc/callback`, set supported account types to *"this organizational directory only"*, and add `email` as an optional ID token claim.

### Original (Mistral Le Chat)

1. Open [le.chat](https://le.chat) and create a new Agent
2. Paste the contents of [`system_prompt.md`](./system_prompt.md) into the system prompt field
3. Choose a model (Mistral Large recommended, Mistral Small for lighter usage)
4. Save and share the agent link with students

## Course resources

- Online textbook: [jankoslavic.github.io/pynm](https://jankoslavic.github.io/pynm)
- Course repository: [github.com/jankoslavic/pynm](https://github.com/jankoslavic/pynm)
