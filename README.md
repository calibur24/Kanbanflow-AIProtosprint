# KanbanFlow Pro

An AI-enhanced Kanban board built as a hackathon project with role-based access, Supabase persistence, Groq Vision-powered task generation, autonomous task auditing, and playful destructive-action UX.

The current implementation is contained in a single `Index.html` file and includes authentication, board management, task management, team management, AI scope expansion, image-to-task generation, overdue-task reassignment, user history stats, realtime sync, and a custom "Sus Action" hover effect with sound. fileciteturn5file0

## Highlights

- **Role-based login**
  - Admin login
  - User login tied to team members
  - Session persistence with `localStorage`
- **Kanban board management**
  - Create boards
  - Delete boards
  - Switch between boards
- **Task management**
  - Create, edit, delete tasks
  - Drag-and-drop between **To-Do**, **In Progress**, and **Done**
  - Priority tags, due dates, assignees
- **Team management**
  - Add members
  - Set and update user passwords
  - Remove users safely
- **AI Scope Expansion**
  - Expands vague tasks into subtasks, acceptance criteria, and risks/edge cases
- **Vision-powered task generation**
  - Upload an image inside the task modal
  - Groq Vision analyzes screenshots, whiteboards, diagrams, handwritten notes, or bug images
  - Suggested tasks can auto-fill the task form
- **Autonomous Project Manager**
  - Detects overdue **In Progress** tasks
  - Reassigns to the strongest finisher based on all-time completion stats
  - Moves the task back to **To-Do**
  - Extends the deadline by 5 days
  - Appends an audit note only once
- **User History Statistics**
  - Shows **completed tasks of all time / total tasks assigned of all time**
  - Shows unfinished count
  - Visible to admin and the relevant user
- **Analytics and visualization**
  - Chart.js task progress overview
  - Summary cards for pending, in-progress, and completed tasks
- **Realtime sync**
  - Supabase-backed data updates reflected live in the UI
- **Fun UX feature**
  - "Sus Action" hover effect on destructive buttons
  - Custom pop-out clip + hover sound effect

## Tech Stack

- **Frontend:** HTML, CSS, Vanilla JavaScript
- **Database / Backend-as-a-Service:** Supabase
- **AI Text / Vision:** Groq API using `meta-llama/llama-4-scout-17b-16e-instruct`
- **Charts:** Chart.js
- **Markdown rendering:** Marked.js

## Main Features in Detail

### 1. Role-Based Access
Admins can manage the full workspace, while users only see and interact with their own assigned tasks. The UI changes depending on the logged-in role. fileciteturn5file0

### 2. AI Scope Expansion
When a task is too vague, the app can expand it into structured engineering work:
- technical subtasks
- acceptance criteria
- risks and edge cases

This is useful for hackathon demos because it turns unclear prompts into more actionable work items.

### 3. AI Image-to-Task Workflow
Inside the **New Task** modal, admins can upload an image and send it directly to Groq Vision. The model returns structured suggestions in JSON, including:
- title
- description
- priority
- suggested team
- suggested deadline

The admin can then apply a suggestion to auto-fill the task form. fileciteturn5file0

### 4. Autonomous Project Manager
The app includes an automated audit loop that periodically checks overdue tasks in **In Progress**. When triggered, it:
- selects the best reassignment target from all-time completion history
- resets the task to **To-Do**
- extends the due date by 5 days
- appends a single audit message without duplicating warnings

This gives the project a stronger "AI project manager" feel for demos.

### 5. User History Statistics
Instead of showing only current-board completion percentages, the app tracks long-run user performance:
- completed tasks of all time
- total assigned tasks of all time
- unfinished count
- completion percentage progress bar

### 6. Sus Action UX
Hovering over destructive actions like delete buttons triggers a playful “sus” effect with a pop-out visual and sound. This was added as a novelty feature for the hackathon demo.

## Project Structure

This version is currently a **single-file app**:

```text
.
├── Index.html
└── README.md
```

The `Index.html` file contains:
- markup
- styling
- client-side logic
- Supabase connection
- Groq API calls
- embedded sound asset

## Setup

### 1. Clone the repository

```bash
git clone <your-repo-url>
cd <your-repo-folder>
```

### 2. Open the app

Because this is a single-file frontend project, you can run it by opening `Index.html` in a browser or serving it locally with a simple static server.

Example with Python:

```bash
python -m http.server 5500
```

Then open:

```text
http://localhost:5500
```

## Configuration

The current file contains frontend constants for Supabase and Groq. In the current hackathon build, these are embedded directly in the HTML. fileciteturn5file0

### Supabase
The project expects:
- a Supabase URL
- a Supabase anon key
- tables for boards, tasks, and team members

### Groq Vision
The app calls the Groq chat completions endpoint and sends uploaded images as `data:` URLs using the model:

```text
meta-llama/llama-4-scout-17b-16e-instruct
```

## Suggested Database Tables

The app logic implies a structure similar to:

### `boards`
- `id`
- `name`

### `tasks`
- `id`
- `title`
- `description`
- `priority`
- `due_date`
- `assignee_id`
- `board_id`
- `status`

### `team_members`
- `id`
- `name`
- `password`

## Demo Flow

A simple hackathon demo sequence:

1. Login as **Admin**
2. Create a board
3. Add team members
4. Create a task manually
5. Create another task from an uploaded image using Groq Vision
6. Move a task to **In Progress** and make it overdue
7. Run **Agent Audit** to show automatic reassignment and deadline extension
8. Switch to a user login to show role-based visibility and personal history stats
9. Hover over a delete button to show the **Sus Action** effect

## Known Limitations

- The current build is a **single-file prototype**, so code organization is not production-grade.
- API keys are embedded in the frontend for demo purposes, which is **not secure** for production. fileciteturn5file0
- Vision analysis quality depends on image clarity and prompt quality.
- The all-time history metric is based on the tasks available in the app database.
- Browser autoplay rules may affect when hover sounds can first play.

## Production Improvements

For a real deployment, the next steps would be:
- move Supabase and Groq secrets to a backend
- split the project into separate HTML/CSS/JS files
- add proper environment variable management
- add stricter validation and error handling
- add audit logs in a dedicated database table instead of embedding them in task descriptions
- improve mobile responsiveness
- add proper user auth instead of plaintext demo passwords

## Why This Project Stands Out

This project goes beyond a normal Kanban board by combining:
- project management
- multimodal AI
- automated task oversight
- visual analytics
- role-based collaboration
- playful interaction design

It is designed to feel like a lightweight AI project manager rather than just a static task board.

## License

Choose a license for your repository, for example:

```text
MIT License
```

---

If you use this README as-is, replace the repository URL, add screenshots, and include your actual Supabase schema if you want it to look more complete on GitHub.
