# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the `claude-code-showcase` repository containing specialized agents for security and AWS AgentCore analysis, along with a comprehensive development workflow for feature-based development.

## 🚨 MANDATORY WORKFLOW PROTOCOL
**When working on features and consulting sub-agents this protocol is mandatory. Make sure to always update the overview.md which each step and don't skip steps mentioned in this guide!**

### Branch Management Protocol

**Before Starting Any Feature Work**:
1. Check current branch: `git branch --show-current`
2. If on `main` branch:
   - **STOP IMMEDIATELY** - Do not proceed with feature work
   - Instruct user to create/switch to feature branch first
   - Verify no uncommitted changes: `git status`
   - Look up available branches and switch or if necessary create and switch to feature branch: `git checkout -b feature/CLAUDE-XXX-descriptive-name`
3. If on feature branch:
   - Verify branch name follows convention
   - Proceed with session setup
4. Present all possible experts and let the user choose which ones to consult
5. Copy .claude/sessions/example/overview.md AS IS into the feature branch: `cp .claude/sessions/example/overview.md .claude/sessions/feature/CLAUDE-XXX-descriptive-name/overview.md`
6. Act according to the steps documented in the overview.md Don't skip steps and always mark a step as done when finished.

### Further Information

#### Session Management

It can happen that you are entering an already worked on session, based on the session folder. Check the branch and files to make sure in which step we are and make sure the information is accurate. Sessions are documented in `.claude/sessions/`.

## Agent Communication Protocol

### SIMPLIFIED AGENT INVOCATION

**Input to Agent**: Provide ONLY the session context file path `.claude/sessions/[BRANCH_NAME]/overview.md` - NO additional context, explanations, or instructions

**Agent Workflow**: Agent reads session data, analyzes project structure, writes ALL findings to analysis file without making changes to the code.

**Output from Agent**: Receive ONLY the findings file path (no summaries, explanations, or additional text)

**Main Agent Action**: Read the comprehensive findings file and integrate into session overview

## Available Experts

**MANDATORY**: The main agent MUST use the appropriate sub-agents for specific tasks. Each sub-agent has specialized knowledge and capabilities for their domain.

### 1. Security Expert (@security-expert)
- **File**: `.claude/agents/security-expert.md`
- **Use for**: Security vulnerability analysis, threat assessment, defensive security practices
- **Specializes in**: AWS security, encryption, IAM, compliance frameworks, security best practices
- **When to use**: Security analysis, vulnerability assessment, compliance requirements, security architecture
- **Output file**: `.claude/sessions/[BRANCH_NAME]/security-analysis.md`
- **Available tools**: Read, Edit, Write, Grep, TodoWrite
- **Knowledge base**: Comprehensive AWS security practices, encryption standards, threat detection

### 2. AgentCore Expert (@agentcore-expert)
- **File**: `.claude/agents/agentcore-expert.md`
- **Use for**: AWS AgentCore service analysis, agent orchestration, multi-agent workflows
- **Specializes in**: AgentCore Runtime, Identity, Memory, Code Interpreter, Browser, Gateway, Observability
- **When to use**: AgentCore implementations, agent deployment, workflow orchestration, agent system architecture
- **Output file**: `.claude/sessions/[BRANCH_NAME]/agentcore-analysis.md`
- **Available tools**: Read, Edit, Write, Grep, TodoWrite
- **Knowledge base**: Complete AgentCore service documentation, deployment patterns, best practices

## Critical Requirements

- **NEVER proceed with feature work on main branch**
- **ALWAYS create proper feature branches**
- **ALWAYS use session management to communicate with subagents**
- **ALWAYS consult appropriate experts**
- **ALWAYS document findings in session files**
- **ALWAYS consult user at important points**
- **Agents must write ALL findings to analysis files**
- **Agent responses must be file paths only**