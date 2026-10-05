# Take-Home Assessment: Pausable Agent Runner on AWS

Thanks for your interest in joining us as a Founding Engineer. This is a **greenfield** exercise: there is no starter code and no prescribed architecture. You decide the design, the AWS services and the tech stack.

You are **expected and encouraged** to build this with Claude Code, Codex or similar agentic tools. We evaluate the result *and* how well you work with these tools.

- **Time budget:** about 4 to 6 hours of focused work. You have up to 7 days to submit.
- **Languages:** TypeScript and/or Python.
- **Cost:** keep it within the AWS free tier or a few dollars. A fully local setup (e.g. LocalStack) is acceptable if you explain why.

---

## The problem

Build an **agent runner pipeline on AWS**: a system that executes a multi-step job, where at least one step is an "agent" (an LLM call or a stub behind an LLM-shaped interface) that can decide it needs a human.

The runner must support these operations:

| Operation | Expected behaviour |
| --- | --- |
| **Start** | Start a run of a pipeline with some input. Returns a run ID. |
| **Request human input** | A step can pause the run and ask a human a question. The pending request must be discoverable. |
| **Submit human decision** | A human answers; the run continues from where it paused, using the answer. |
| **Stop** | An operator can stop a run at any point, including while it is waiting for a human. State is preserved. |
| **Resume** | A stopped or failed run continues without redoing completed steps and without repeating side effects. |
| **Status** | Show a run's current step, state, pending requests and history. |

What the pipeline actually *does* is up to you. Keep the business logic trivial (e.g. three or four steps that transform some JSON, with one agent step that sometimes asks for approval). The interesting part is the runner, not the workload.

### What we care about

- A **clear state model** for runs and steps, and where that state lives.
- **Durability**: a run can wait for a human for hours or days without burning compute.
- **Idempotency**: retries, duplicate human answers and resumes must not cause double side effects.
- A **clean seam** between orchestration and agent logic, so a real LLM can be plugged in.
- **Infrastructure as code**, deployable in one or two commands, with a teardown.
- **Tests** for the parts that matter most.
- A simple way to drive it: CLI, small HTTP API or scripts. No UI required.

### Out of scope

Authentication, a frontend, multi-tenancy and production-grade observability. Mention how you'd approach them, but don't build them.

---

## Working with Claude: required

We want to see how you set up and steer an agentic coding workflow, following [Anthropic's Claude Code best practices](https://www.anthropic.com/engineering/claude-code-best-practices).

Your repository must include:

1. **`CLAUDE.md`** (and/or `AGENTS.md` for Codex) – project context, conventions, commands and guardrails for the agent. Keep it current as the project evolves.
2. **At least one custom skill** in `.claude/skills/<skill-name>/SKILL.md` – reusable know-how for this project (for example: how to add a new pipeline step, how to write an idempotent handler, how to deploy and test).
3. **At least two custom slash commands** in `.claude/commands/` – repeatable workflows you actually used (for example: `/new-step`, `/deploy`, `/test-run`, `/review`).
4. **Documentation** of all of the above in [`docs/AI_USAGE.md`](docs/AI_USAGE.md): what each skill and command does, why you created it, and an example of using it.

Quality beats quantity. One well-designed skill that you clearly used is worth more than five you never ran.

---

## What to submit

A link to a **private GitHub repository** (share access with us), containing:

1. Your code and IaC.
2. **[`docs/DESIGN.md`](docs/DESIGN.md)** – architecture, state model, how stop / human-in-the-loop / resume work, trade-offs, alternatives considered, what you'd do next.
3. **[`docs/AI_USAGE.md`](docs/AI_USAGE.md)** – your Claude/Codex workflow, skills, slash commands, and where the tools got it wrong.
4. **`CLAUDE.md`**, **`.claude/skills/`** and **`.claude/commands/`** as described above.
5. A short **demo**: a 2 to 5 minute recording or a scripted walkthrough showing start → pause for human input → stop → resume → submit decision → completed.
6. Your **git history**, kept intact. We read it.

## How we evaluate

| Area | What we look for |
| --- | --- |
| Correctness | Stop, human-in-the-loop and resume work, including edge cases (stop while waiting, answer submitted twice, resume after a failure). |
| Design | Sensible, well-argued AWS choices; clear state model; idempotency; clean separation of concerns. |
| Pragmatism | Scope fits the time budget. Simple where it can be, careful where it must be. |
| Code quality | Readable code, meaningful tests, deployable IaC. |
| Communication | Crisp docs with explicit trade-offs and assumptions. |
| Agentic workflow | Thoughtful CLAUDE.md, useful skills and slash commands, honest account of where AI helped and where you corrected it. |

In the follow-up interview you'll walk us through your solution and extend it live with your AI tools of choice.

## Questions

If something is unclear, make a reasonable assumption, write it down in `DESIGN.md`, and move on. You can also email us; we reply within a working day.

Good luck, and have fun with it.
