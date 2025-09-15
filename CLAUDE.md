# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the `claude-code-showcase` repository containing specialized agents for security and AWS AgentCore analysis, along with a comprehensive development workflow for feature-based development.

## 🚨 MANDATORY WORKFLOW PROTOCOL
**When working on features and consulting sub-agents this protocol is mandatory. Make sure to always update the overview.md which each step and don't skip steps mentioned in this guide!**

After every step make sure to mark the step as done and continue with the next. Don't skip steps.

- [ ] Feature branch created
- [ ] Session directory created
- [ ] Issue requirements documented
- [ ] Initial overview created baseed on `.claude/sessions/example/overview.md`
- [ ] Presented all experts to user and decided which experts to consult in what order
- [ ] Expert consultations
   - [ ] Expert consultation 1
   - [ ] Transcribe expert analysis 1 to overview.md
   - [ ] Expert consultation 2
   - [ ] Transcribe expert analysis 2 to overview.md
   - ...
- [ ] Create comprehensive implementation plan in overview.md based on expert consultation
- [ ] Make sure that user is happy with the plan presented in overview.md
- [ ] Implementation of overview.md with user
- [ ] Reviewing with internal agent against issue.md
- [ ] Update sub-agents based on changes

### Further Information

#### Session Management

It can happen that you are entering an already worked on session, based on the session folder. Check the branch and files to make sure in which step we are and make sure the information is accurate. Sessions are documented in `.claude/sessions/`.

#### Branch Management Protocol

**Before Starting Any Feature Work**:
1. Check current branch: `git branch --show-current`
2. If on `main` branch:
   - **STOP IMMEDIATELY** - Do not proceed with feature work
   - Instruct user to create feature branch first
   - Verify no uncommitted changes: `git status`
   - Create and switch to feature branch: `git checkout -b feature/CLAUDE-XXX-descriptive-name`
3. If on feature branch:
   - Verify branch name follows convention
   - Proceed with session setup

#### Task Initialization / Session Management

1. **Main Agent** starts working on ticket/issue
2. **MANDATORY BRANCH CHECK**: If currently on main branch, STOP and instruct user to create feature branch first
   - Feature branch naming: `feature/CLAUDE-XXX-descriptive-name`
   - Example: `feature/CLAUDE-001-bedrock-lambda-scaffold`
   - **DO NOT PROCEED** until proper feature branch is created and checked out
   - Check if a similar branch already exists, if yes swatch to this one
4. Check if there was already a session in place. if yes simply check the accuracy of already made progress
3. **If it doesn't exist yet, create session directory**: `.claude/sessions/[BRANCH_NAME]/`
   - Exmaple: `.claude/sessions/feature-CLAUDE-001-bedrock-lambda-scaffold`
4. **Create/update initial requirements in issue.md file**: 
   - Ticket details and acceptance criteria
   - Technical scope and constraints
   - Initial architectural thoughts
5. **Create/update overview file**: using `.claude/sessions/example/overview.md` as template edit the real `.claude/sessions/[BRANCH_NAME]/overview.md
6. **Maintain session in overview.md file**: Update overview.md THROUGHOUT the development process with key findings and decisions from experts

#### Expert Consultation

1. **ALWAYS ask user which experts are needed** - ALWAYS give them a list of ALL available experts and provide your recommendation - but inquire what they think we need. Also ask them for the order in which to call them
3. **ALWAYS INTEGRATE EXPERT FINDINGS IMMEDIATELY INTO overview.md** - after calling an agent and before calling the next agent ALWAYS read and integrate key findings from each agent's analysis file into the session overview.md so the next agent can use the information from the previous agent
4. **Invoke agents sequentially** using the simplified protocol
5. **Proceed to next phase** if the user is happy with the overview.md

#### Implementation

1. **Review consolidated findings** in overview.md with user
2. **Confirm implementation approach** based on expert recommendations
3. **Integrate changes** collaboratively with user guidance
4. **Update session documentation** with implementation decisions and outcomes
5. **Validate implementation** meets requirements and expert recommendations

#### Code Review and Validation

1. **Internal Code Review**: Use internal /review command to compare made changes to the `.claude/sessions/[BRANCH_NAME]/issue.md`
2. **Present findings to user**: Communicate implementation results and any deviations from original plan
3. **User validation**: Confirm implementation meets acceptance criteria before proceeding
4. **Update overview.md**: Document review outcomes and user feedback
5. **Proceed only after user approval**

#### Session Completion and Evolution

1. **Main Agent reviews**: Complete session outcomes in overview.md
2. **Document learnings**: Capture patterns and insights for future sessions
3. **Archive session**: Maintain session files for future reference
4. **Agent evolution**: Note any gaps or improvements needed in agent capabilities. Changes in code, architecture and infrastructure should be present in the `.claude/agents/` experts. With the help of the user update their knowledge.

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

### File Structure

```
.claude/
├── agents/
│   ├── security-expert.md
│   └── agentcore-expert.md
├── sessions/
│   └── [BRANCH_NAME]/
│       ├── issue.md
│       ├── overview.md
│       ├── security-analysis.md (if security expert consulted)
│       └── agentcore-analysis.md (if agentcore expert consulted)
└── knowledge/
    └── agentcore.pdf
```

## Critical Requirements

- **NEVER proceed with feature work on main branch**
- **ALWAYS create proper feature branches**
- **ALWAYS use session management to communicate with subagents**
- **ALWAYS consult appropriate experts**
- **ALWAYS document findings in session files**
- **ALWAYS consult user at important points**
- **Agents must write ALL findings to analysis files**
- **Agent responses must be file paths only**

## Development Commands

Commands will be established as the project grows:
- Build commands (TBD)
- Testing commands (TBD)
- Linting and formatting (TBD)