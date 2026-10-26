# Local Setup

This vault is separate from any existing vault. No personal-vault paths or configuration are needed.

## Install and open

1. Clone or extract the project into a new folder named `prompt-to-knowledge` wherever you keep standalone projects. Do not place it inside an existing vault.
2. In Obsidian, choose **Open folder as vault** and select that folder.
3. Enable **Settings → General → Command line interface** and complete the registration prompt. Keep Obsidian running. The official documentation consulted for this setup specifies the Obsidian 1.12 installer and recommends installer 1.12.7 or later; check the current documentation for your installation.
4. Open the same folder as a local project in Codex, with permission to use the local terminal and this folder. A chat environment needs local execution access to run a local CLI.
5. Confirm the vault name explicitly. If it differs from the examples, update the three AGENTS files and the command examples before running any mutations.

```sh
obsidian help
obsidian vault="prompt-to-knowledge" vault info=name
obsidian vault="prompt-to-knowledge" search query="path:review knapsack"
obsidian vault="prompt-to-knowledge" read path="review/dynamic-programming/knapsack-and-subset-sum.md"
```

## How the connection works

```text
Prompt → local agent → AGENTS.md guidance → Obsidian CLI → notes
```

This vault supplies instructions and Markdown. Obsidian provides the CLI, and the agent environment supplies terminal execution. No custom bridge server is included. Files live locally, but text the agent reads can be sent to its model provider as part of the conversation, so local storage alone does not mean offline inference.

The root `AGENTS.md` provides vault-wide instructions. The review and exam-prep files specialize tasks in their folders. The root file explicitly tells the agent to read those files when routing a task, including tasks launched from the root.

## Read and append

A read-only first prompt:

> Read the root instructions and explain why Dijkstra's algorithm fails on negative edges, using only the review notes. Do not edit files.

A deliberate edit prompt:

> Read the root and review instructions. Append a short, original Prim's-algorithm trace to the existing MST note, avoiding repetition of its existing explanation. Report the changed file.

The underlying CLI supports commands like:

```sh
obsidian vault="prompt-to-knowledge" append path="review/greedy/minimum-spanning-trees.md" content="An explicitly requested example goes here."
```

These mutation commands are illustrative, not installation steps. Search and read before appending. Check `obsidian help append` or other command-specific help when the installed syntax differs.

Inside `content` values, the CLI turns a backslash followed by `n` into a newline, and a backslash followed by `t` into a tab. It has no escape for a literal backslash-n, so write such text with Obsidian or an editor instead. In an agent script, pass arguments as an array rather than interpolating untrusted source text into a shell command, then read the note back to confirm the result.

## Optional Git setup

Before staging anything, create a `.gitignore` that excludes OS metadata, Obsidian workspace and session files, credentials, and any locally retained PDFs you do not intend to distribute. A conservative starting set is:

```gitignore
.DS_Store
.obsidian/workspace.json
.env
.env.*
!.env.example
**/materials/
*.pdf
```

You can later share selected, reviewed Obsidian settings. A `.gitignore` does not untrack an already tracked file or remove it from history. Inspect the staged files and the diff before committing or publishing.

## References

- [Obsidian CLI official documentation](https://obsidian.md/help/cli), consulted 2026-10-20.
- [OpenAI: Custom instructions with AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md), consulted 2026-10-20.

The source PDFs are optional for reading the notes, but required for checking their original wording or diagrams. See [[setup/source-materials|source materials]].
