---
date-generated: 2026-10-20
date-updated: 2026-10-20
---

# Local Setup

This package is separate from any existing vault. No personal-vault paths or configuration are needed.

## Install and open

1. Extract the archive into a new folder named `cs537-os-starter` wherever you keep standalone projects. Do not extract it inside an existing vault.
2. In Obsidian, choose **Open folder as vault** and select the extracted folder.
3. Enable **Settings → General → Command line interface** and complete the registration prompt. Keep Obsidian running. The official documentation consulted for this starter specifies the Obsidian 1.12 installer and recommends installer 1.12.7 or later; consult the current documentation for your installation.
4. Open this same folder as a local project in Codex, with permission to use the local terminal and this folder. A chat environment needs local execution access to invoke a local CLI.
5. Confirm the vault name explicitly. If it differs from the examples, update the three AGENTS files and command examples before running mutations.

```sh
obsidian help
obsidian vault="cs537-os-starter" vault info=name
obsidian vault="cs537-os-starter" search query="path:review paging"
obsidian vault="cs537-os-starter" read path="review/virtualization/address-translation-and-page-tables.md"
```

## How the connection works

```text
Prompt → local agent → AGENTS.md guidance → Obsidian CLI → notes
```

This starter supplies instructions and Markdown. Obsidian provides the CLI; the agent environment supplies terminal execution. No custom bridge server is included. Files live locally; text the agent reads can be sent to its model provider as part of the conversation, so local storage alone does not mean offline inference.

The root `AGENTS.md` provides vault-wide instructions. The review and exam-prep files specialize tasks in their folders. The root explicitly asks the agent to read those files when routing a task, including tasks launched from the root.

## Read and append

A read-only first prompt:

> Read the root instructions and explain the difference between a TLB miss and a page fault using only the review notes. Do not edit files.

A deliberate edit prompt:

> Read the root and review instructions. Append a short, original example of a TLB hit to the existing TLB note, avoid repeating its existing explanation, and update date-updated. Report the changed file.

The underlying CLI supports commands like:

```sh
obsidian vault="cs537-os-starter" append path="review/virtualization/tlb-and-page-replacement.md" content="\n## Additional example\n\nAn explicitly requested example goes here."
obsidian vault="cs537-os-starter" property:set path="review/virtualization/tlb-and-page-replacement.md" name="date-updated" value="2026-10-20" type=date
```

The mutation commands are illustrative, not installation steps. Use the actual current maintenance date. Search and read before appending. Consult `obsidian help append` or other command-specific help when installed syntax differs. In an agent script, use argument arrays rather than interpolating untrusted source text into a shell command.

## Optional Git setup

Start a fresh repository for this folder. Before staging anything, create a `.gitignore` that excludes OS metadata, Obsidian workspace/session files, credentials, and any locally retained PDFs you do not intend to distribute. A conservative initial pattern set is:

```gitignore
.DS_Store
.obsidian/
.env
.env.*
!.env.example
**/materials/
*.pdf
```

You can later share selected, reviewed Obsidian settings. `.gitignore` does not untrack an already tracked file or remove it from history. Inspect staged files and the diff before creating a commit or publishing. Use screenshots from this starter for demonstrations.

## References

- [Obsidian CLI official documentation](https://obsidian.md/help/cli), consulted 2026-10-20.
- [OpenAI: Custom instructions with AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md), consulted 2026-10-20.

The source PDFs are optional for reading the study notes, but required when verifying their original wording or diagrams. See [[setup/source-materials|source materials]].
