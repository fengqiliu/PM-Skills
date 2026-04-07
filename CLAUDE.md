# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

PM-Skills is a skill and agent manager for Claude Code, providing reusable prompts and specialized agents for product managers and software architects. The repository contains skills (prompt templates with metadata), agents (autonomous assistants), and plugins (complete Claude Code extensions).

## Directory Structure

```
skills/                          # PM Skills (prompt templates)
├── pm-alpha-*/                 # Alpha: Daily workflow (5 skills)
├── pm-beta-*/                  # Beta: Specialized skills (5 skills)
├── pm-gamma-*/                 # Gamma: Medical/Healthcare (3 skills)
├── pm-delta-*/                 # Delta: Collaboration/Growth (4 skills)
├── weixin-writer/              # WeChat article writing (with references/)
└── wechat-mp-writer-skill/     # Legacy WeChat skill

agents/                         # Claude Code agents
├── product-manager.md          # PM assistant (requirements, PRD, user research)
├── code-reviewer.md            # Code architecture reviewer
├── pacs-expert.md              # PACS/RIS medical imaging expert
└── competitive-analyst.md      # Competitive analysis expert

plugins/                        # Claude Code plugins
└── architect-plugin/           # Software architect toolkit
    ├── .claude-plugin/         # Plugin manifest (plugin.json)
    ├── skills/                 # 5 architecture skills
    │   ├── architecture-design/
    │   ├── code-review/
    │   ├── technical-decision/
    │   ├── architecture-assessment/
    │   └── api-design/
    └── agents/
        └── software-architect.md
```

## Skill Format

Skills follow this structure:
- `SKILL.md`: YAML frontmatter (`name`, `description`) + skill content
- `_meta.json`: `{"ownerId", "slug", "version", "publishedAt"}`
- `references/`: Optional reference materials (e.g., `weixin-writer/references/`)

## Agent Format

Agents are markdown files with YAML frontmatter:
- `description`: Agent role and expertise
- `capabilities`: List of what the agent can do

## Architecture Insights

**PM Skills tracks** (alpha/beta/gamma/delta) represent maturity/progression levels, not just categories:
- Alpha = daily foundational workflows
- Beta = specialized capabilities  
- Gamma = domain-specific (healthcare/PACS)
- Delta = soft skills and career growth

**Plugin structure**: Architect-plugin uses `.claude-plugin/plugin.json` as the plugin manifest, distinguishing it from standalone skills/agents.

## Key Content

- **PACS_RIS_PM_Daily_Workflow.md**: Comprehensive workflow for PACS/RIS product managers in medical informatics — daily schedules, requirement lifecycle, Sprint management, NMPA compliance, HL7/DICOM standards, OKR/KPI metrics, and recommended tool stacks.

- **Skills**: 17 PM skills + weixin-writer (WeChat article writing with content methodology).

- **Agents**: 4 specialized agents for PM, code review, PACS expertise, and competitive analysis.

- **architect-plugin**: Complete plugin with 5 architecture skills and 1 architect agent.

## Commands

This is a documentation/knowledge repository. Skills and agents are invoked through natural language in Claude Code:

```
# Agents (use @ prefix or describe need)
@product-manager, @code-reviewer, @pacs-expert, @competitive-analyst

# Skills (slash commands)
/architecture-design, /code-review, /technical-decision
/architecture-assessment, /api-design
/pm-alpha-requirement-analysis, /pm-beta-competitive-analysis, etc.
```
