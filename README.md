# Osmedeus Skills for AI Agents

Give any AI coding agent deep knowledge of the [Osmedeus](https://github.com/j3ssie/osmedeus) security automation engine — writing YAML workflows, running CLI commands, and configuring advanced features.

Works with Claude Code, Cursor, Amp, and more.

## Installation

First, clone the repo:

```bash
git clone https://github.com/osmedeus/osmedeus-skills.git
cd osmedeus-skills
```

---

### Claude Code

#### Option A: Personal (available in all your projects)

```bash
mkdir -p ~/.claude/skills
ln -s "$(pwd)/osmedeus-expert" ~/.claude/skills/osmedeus-expert
```

#### Option B: Copy directly

```bash
mkdir -p ~/.claude/skills
cp -r osmedeus-expert ~/.claude/skills/osmedeus-expert
```

#### Option C: Project-level (available only in a specific project)

```bash
cd /path/to/your/project
mkdir -p .claude/skills
ln -s /path/to/osmedeus-skills/osmedeus-expert .claude/skills/osmedeus-expert
```

#### Verify

Open Claude Code and type `/osmedeus-expert` — it should appear in the skill list.

---

### Cursor

Cursor uses [rules files](https://docs.cursor.com/context/rules) to provide custom context to the AI.

#### Option A: Project-level (recommended)

```bash
cd /path/to/your/project
mkdir -p .cursor/rules
cp osmedeus-expert/SKILL.md .cursor/rules/osmedeus-expert.mdc
```

#### Option B: Global

```bash
mkdir -p ~/.cursor/rules
cp osmedeus-expert/SKILL.md ~/.cursor/rules/osmedeus-expert.mdc
```

To include the full reference files, append them:

```bash
cat osmedeus-expert/references/*.md >> .cursor/rules/osmedeus-expert.mdc
```
---

### Other Agents

For any AI agent that supports custom instructions or system prompts, you can use the content from `osmedeus-expert/SKILL.md` (core knowledge) and `osmedeus-expert/references/*.md` (detailed references) directly. Copy or paste them into your agent's configuration.

To combine everything into a single file:

```bash
cat osmedeus-expert/SKILL.md osmedeus-expert/references/*.md > osmedeus-combined.md
```

## Usage

### Invoke directly

```
/osmedeus-expert
```

### Or just ask naturally

Claude will auto-trigger the skill when your question is about Osmedeus. Here are example prompts:

#### Writing Workflows

```
Write me an Osmedeus module that runs subfinder and amass in parallel,
merges the results, then resolves live hosts with dnsx.
```

```
Create a flow that chains subdomain enumeration, port scanning,
and vulnerability scanning with nuclei. Skip vuln scanning if
no open ports are found.
```

```
How do I write a foreach step that scans each subdomain with nmap
using 10 concurrent threads?
```

```
Add decision routing to my workflow — if scan_depth is "quick",
skip the nuclei scan and go straight to reporting.
```

```
Write a module that uses the agent step type to analyze scan results
with an LLM and generate a prioritized vulnerability report.
```

#### Running Osmedeus

```
How do I run a module against a list of 500 targets with concurrency of 10?
```

```
What's the command to run a flow with a 2-hour timeout and custom parameters?
```

```
How do I set up osmedeus as a distributed scanner with a master and workers?
```

```
Show me how to chunk a large target list and run only the first chunk.
```

#### Configuration & Advanced

```
How do I set up a cron trigger that runs my workflow every night at 2 AM?
```

```
Create a workflow that extends a base module but overrides the thread count
and adds an extra vulnerability scanning step.
```

```
How do I configure a Docker runner for a specific step in my workflow?
```

```
What template variables are available? How do I use platform detection
to run different commands on Linux vs macOS?
```

#### Debugging

```
My foreach step isn't working — the variable is empty. What am I doing wrong?
```

```
How do I validate my workflow YAML before running it?
```

```
What does on_error: continue do and when should I use it?
```

## What's Inside

```
osmedeus-expert/
├── SKILL.md                          # Main skill (commands, workflow basics, patterns)
└── references/
    ├── cli-flags.md                  # All CLI flags for every command
    ├── step-types.md                 # Complete field reference for all 8 step types
    ├── template-variables.md         # Variables, generators, utility functions
    ├── workflow-advanced.md          # Triggers, inheritance, runners, decisions
    └── examples.md                   # 7 full annotated workflow examples
```

The skill uses progressive disclosure — SKILL.md loads first with essential knowledge, and reference files are pulled in only when deeper detail is needed on a specific topic.

## Updating

```bash
cd osmedeus-skills
git pull
```

If you used a symlink, the update is instant. If you copied, re-copy the files.

## License

Osmedeus is made with ♥ by [@j3ssie](https://twitter.com/j3ssie) and it is released under the MIT license.
