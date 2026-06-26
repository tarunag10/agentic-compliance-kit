# Safety Model

`agentic-compliance-kit` is designed for compliance workflows where source
records may be privileged, confidential, personal, regulated, or business
critical. The default operating posture is read-only until a maintainer or user
explicitly approves a write action.

## Read-Only Defaults

Agents and tools should inspect legal documents, compliance records, tickets,
policies, and evidence stores without changing them by default. A workflow
should treat file edits, uploads, database writes, ticket updates, comments,
messages, and pull requests as separate write actions that require approval.

Read-only mode should still preserve useful context for review: source paths,
record identifiers, timestamps, tool names, and generated outputs. It should not
silently rewrite source material, remove evidence, or replace user decisions.

## Human Review Requirements

Require explicit approval before any workflow:

- edits or deletes source documents;
- uploads generated files to a third-party system;
- sends messages, emails, issue comments, or pull request comments;
- opens, updates, merges, or closes issues and pull requests;
- changes labels, statuses, assignments, or compliance records;
- escalates privileges or uses credentials beyond the original read scope.

Approvals should be specific to the action, target, and output. Broad approval
for one step should not be reused for unrelated systems or later writes.

## Audit Logs

Compliance agents should produce audit logs that are useful to maintainers,
reviewers, and users. Logs should include:

- tool calls and the reason each tool was used;
- source record identifiers and relevant paths;
- generated outputs and where they were stored;
- approval decisions, approver identity when available, and timestamps;
- write actions attempted, completed, skipped, or rejected;
- errors, retries, and any fallback behavior.

Audit logs should avoid storing unnecessary secrets or full sensitive payloads.
Prefer stable identifiers, hashes, excerpts, and links to controlled systems
when that provides enough evidence for later review.

## Sensitive Data Boundaries

Treat the following as sensitive unless a project-specific policy says
otherwise:

- privileged or confidential legal materials;
- personal data, health data, financial data, employment data, and government
  identifiers;
- security credentials, tokens, session cookies, private keys, and webhook
  secrets;
- internal investigations, regulatory filings, risk assessments, and audit
  evidence;
- proprietary contracts, policies, playbooks, and customer records.

Agents should minimize data access, redact secrets from logs, and avoid sending
protected material to tools or services that are outside the approved workflow.
When the correct handling is unclear, stop at a draft recommendation and ask for
human review before taking action.

## Maintainer Expectations

Maintainers should document which tools are read-only, which tools can write,
and which permissions each integration requires. New examples should show safe
defaults first and make approval boundaries visible in code, configuration, and
documentation.

Changes that add write capability should include tests or examples that prove
the write is gated behind explicit approval.
