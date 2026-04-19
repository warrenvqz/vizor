# Vizor — Team Capacity

A single-file, zero-dependency resource planner. Allocate design and copy
tasks to your team by week, see who's overbooked, and keep a light pipeline
of upcoming projects.

## Features

- **Planner** — people × weeks grid; drag task chips between cells, click a
  chip to edit days/role, auto-totaled per person with amber at 80% and red
  over 100%.
- **Projects** — all projects grouped by status (Active, Upcoming, Done).
  Create, rename, recolor, or delete projects. Each project card shows its
  full task list with click-to-edit and inline "+ Add task".
- **Board** — time-bucketed view (Later / This Week / Today / Done) based
  on task due dates.
- **Team management** — add / edit / remove people inline; removing a
  person unassigns their staffed tasks safely.
- **Capacity heatmap** — right-rail summary across every week.
- **Google Sheets import** *(optional)* — seed the tool from an existing
  resourcing sheet.

## Running locally

No build step. Just open `index.html` in a browser.

```bash
# or serve it locally so you can reload cleanly
python3 -m http.server 8000
# → http://localhost:8000
```

## Tech

Plain HTML + CSS + JS. No frameworks, no bundler. All state lives in memory
for the session — reloading the page resets to the seed data. If you want
persistence, wiring the `PROJECTS`, `TASKS`, and `PEOPLE` globals to
`localStorage` is a dozen lines.

## Data model

Everything revolves around three arrays:

- `PEOPLE` — teammates (`id`, `name`, `role`, `color`, `initials`)
- `PROJECTS` — projects (`id`, `name`, `color`, `status`, `start`, `end`)
- `TASKS` — the single source of truth
  (`id`, `projectId`, `name`, `role`, `effortDays`, `assigneeId?`,
  `weekLabel?`, `due?`, `done?`)

A task is "staffed" once it has both `assigneeId` and `weekLabel`.
Unstaffed tasks appear in the right-rail Upcoming Needs panel, draggable
onto any person-week cell.
