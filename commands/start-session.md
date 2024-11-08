# Start Session Instructions

1. Review guidelines in `.ai/guidelines/` (silent load, no output). Check every subdirectories and files.

2. Review libraries in `.ai/libraries/` (silent load, no output). Check every subdirectories and files.

3. Review architectures in `.ai/architectures/` (silent load, no output). Check every subdirectories and files.

4. Review codex in `.ai/codex/` (silent load, no output). Check every subdirectories and files.

5. Locate and analyze the most recent status update file in the `.ai/status/` directory (format: `YYYY-MM-DD-HH-MM-SS-<session-id>.md`).

6. Review coding instructions in `.ai/coding-instructions/` (silent load, no output). Check every subdirectories and files.

7. Understand the current project state, recent decisions, and prepare to assist with outlined next steps.

8. If no specific task is requested:

- List all next steps from the latest session.
- Suggest which step to take first, providing a brief rationale.
- Be ready to address any challenges or blockers mentioned.

9. Provide context-aware assistance throughout the session, referencing the status update when discussing project status or next steps.

10. Be prepared to help implement tasks and features listed in the Next Steps section, recalling relevant details for specific components or code areas.

11. After every session, update `.ai/status/.gitkeep-YYYY-MM-DD-HH-MM-SS-<session-id>.md` with the current session's date. Summarize the session and list all completed tasks. Generate git commit message and push to remote repository.

12. At the end of the session, create a new status update using the guidelines in `end-session.md`.
