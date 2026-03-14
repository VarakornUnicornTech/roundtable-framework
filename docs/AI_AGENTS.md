# AI Agent Contributors

RoundTable Framework is built for AI-assisted development, and we welcome contributions from AI agents alongside human contributors. This document defines the rules, expectations, and guidelines for AI participation.

## Why AI Contributions Are Welcome

RoundTable Framework is an AI governance tool — built with Claude Code, managed by AI teams, and directed by a human Commander. AI contributions are a natural extension of this philosophy. We believe that AI agents can improve the framework they themselves operate within, creating a virtuous cycle of improvement.

## Rules for AI-Generated Pull Requests

### 1. Disclosure Is Mandatory

Every PR that contains AI-generated content **must** declare:
- Which AI tool was used (Claude Code, Claude API, GitHub Copilot, etc.)
- What percentage of the PR is AI-generated vs. human-written
- This is declared in the **AI Disclosure** section of the PR template

### 2. Human Sponsorship Required

Every AI-generated PR **must** have a human sponsor who:
- Is identified in the PR description
- Has reviewed the AI-generated content for accuracy
- Takes full responsibility for the contribution
- Can respond to review feedback and make corrections
- Has a GitHub account and is reachable for follow-up

### 3. Same Review Standards Apply

AI-generated contributions go through the **exact same review process** as human contributions:
- CODEOWNERS-based review assignment
- All automated CI checks must pass
- Reviewer checklist applies in full
- Security review for `.claude/` changes
- No exceptions or fast-track for AI-generated PRs

### 4. No Automated Mass PRs

AI agents **may not** submit large numbers of PRs without prior maintainer approval:
- Maximum 3 open PRs from any single AI agent/operator at a time
- Batch submissions (e.g., "fix all typos in docs/") must be pre-approved by a maintainer
- Automated PR bots must be registered with the maintainer team before operating

### 5. Quality Over Quantity

AI agents should prioritize:
- Meaningful, well-scoped changes
- Accurate content (AI may hallucinate — reviewers will check)
- Complete PR descriptions with context and reasoning
- Proper testing documentation

## AI Agent Testing

AI agents can be used for **automated testing** of the framework:

### Framework Initialization Testing
- Verify that `.claude/CLAUDE.md` loads correctly in Claude Code
- Verify that team roster/agent files are recognized
- Verify that skills respond to slash commands
- Verify that hooks enforce file protection

### Policy Compliance Testing
- Check that policies are internally consistent
- Verify that section numbers match filenames
- Validate that all cross-references resolve correctly
- Ensure no legacy naming contamination

### Regression Testing
- After framework changes, verify that existing features still work
- Test across different operating systems (Windows, macOS, Linux)
- Test with different Claude Code versions

### Test Reporting
All AI-generated test results must:
- Be reproducible by a human
- Include the exact Claude Code version and configuration
- Specify the operating system and environment
- Not be fabricated or estimated — actual test runs only

## Known AI Integrations

| AI Tool | Role in RoundTable | Status |
|---------|-------------------|--------|
| **Claude Code** | Primary development environment | Core integration |
| **Claude Agent SDK** | Subagent execution for multi-team operations | Supported |
| **Claude API** | Potential for automated testing and validation | Community contribution |
| **GitHub Copilot** | Code suggestions during contribution | Accepted (with disclosure) |
| **Other AI tools** | Case-by-case | Must disclose, same rules apply |

## What AI Agents Should NOT Do

- Submit PRs without human review/sponsorship
- Modify security-critical files without explicit authorization
- Create issues or discussions pretending to be human
- Overwhelm the project with automated contributions
- Submit test results that weren't actually executed
- Copy content from other projects without proper attribution

## Future Vision

As AI agents become more capable, we anticipate:
- AI agents participating in code reviews (advisory, not binding)
- Automated framework compliance checking via AI
- AI-assisted translation for bilingual documentation
- Collaborative human-AI pull requests as a standard workflow

We'll update this document as these capabilities mature. Community input on AI integration is welcome in [GitHub Discussions](https://github.com/VarakornUnicornTech/UniOpsQC/discussions).

---

*See [CONTRIBUTING.md](../CONTRIBUTING.md) for the full contribution workflow and [REVIEW_PROCESS.md](REVIEW_PROCESS.md) for review standards.*
