# claude-context
Agents and commands from [HumanLayer](https://www.humanlayer.dev)'s Claude Code Workflow, personalized for my development environment. See [I Mastered the Claude Code Workflow](https://medium.com/@ashleyha/i-mastered-the-claude-code-workflow-145d25e502cf) by Ashley Ha. Also see [my blog post](https://gcgrafix.com/blog/010226-chordkeeper-dev) about my AI workflow so far.

### [My] Setup
1. Copy the `claude/agents` and `claude/commands` directories to [my] user's global `.claude` directory.
2. Copy the `thoughts` directory tree to the project's root directory.

### My Typical Workflow
1. Grab a ticket from my project management tool (Plane.so).
2. Create a new branch for the ticket.
3. In my ADE/IDE's agent chat, run `/research_codebase TICKET-ID` plus context and prompt details.
   a. Review the research doc it creates, iterate. No coding happens in this step.
4. Clear context (e.g. start a new chat), then run `/create_plan` plus the research doc it created in the previous step.
   a. Review the plan, answer any questions the agent has. Again, no coding happens in this step.
5. Clear context, then run `/implement_plan` plus the plan doc it created in the previous step.
6. Review code changes, test dev, iterate with agent as needed.
7. Commit, push, open PR, merge.
8. Depending on project, deployment happens automatically (e.g. CI/CD with GitHub Actions + Docker).

Subsequent tasks in the same project can reference prior research and plans for well-defined context, reasoning, and continuity.

### Future Work
- Incorporate new skills, etc.
- Use Linear (as originally implemented) or implement Plane GH issue integration (requires Pro) to pull ticket details automatically.
