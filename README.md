# Flightdeck

A windowed desktop workspace for Microsoft admin portals across Commercial and GCC High clouds, with an embedded AI assistant per profile. It hosts every admin portal in one window, signs you in once per profile, keeps each customer tenant fully isolated, and puts a Claude Code assistant next to every tenant that can act through Microsoft Graph, Azure Resource Manager, and Exchange PowerShell with your approval.

[![Download Flightdeck for Windows](https://img.shields.io/badge/Download-Flightdeck%20for%20Windows-0078d4?style=for-the-badge&logo=windows&logoColor=white)](https://github.com/jadenentropex/flightdeck-releases/releases/latest/download/Flightdeck-Setup.exe)

The button above always downloads the latest installer for Windows 10 and 11, 64-bit.

## Portals and profiles

- All Microsoft admin portals in one app: Azure, Entra, Intune, Defender, Exchange, Purview, Teams, Power Platform, and more, grouped by category, for Commercial and GCC High.
- One sign-in per profile: sign in once in a profile and every portal in that cloud opens without signing in again. The session persists across restarts.
- Named, isolated profiles per customer tenant: cookies, cache, and sign-in are siloed so tenants never touch each other.
- Multiple instances of the same portal in per-instance tabs, with quick-switch chips.
- Tenant-specific portals resolve themselves. The SharePoint admin center host comes from the tenant's initial onmicrosoft domain, looked up from the directory and cached, because it is usually nothing like the vanity domain.
- A credential vault, encrypted with your Windows account key (DPAPI) and kept separate from the browser session, with autofill into Microsoft sign-in pages only.
- An isolated per-project web browser for looking things up without leaving the app.

## The AI assistant

Every profile has its own embedded Claude Code assistant that acts as the signed-in administrator for that tenant.

- Tools bound to your session: Microsoft Graph (read and write), Azure Resource Manager (read and write), live portal automation (screenshot, snapshot, navigate, click, type), and Exchange Online PowerShell and in-session Graph PowerShell for settings that live only there.
- Three modes, and the assistant is told which one is in force: read-only, approve (every change raises a card), and auto-run. In auto-run it can still stop and ask: an explicit approval request is gated in every mode, so when the assistant asks, it genuinely waits for your answer.
- Attach anything to a message: paste a screenshot straight into the composer, or attach a file from anywhere on disk. Attachments are copied into the profile workspace, so they stay readable, reusable, and inside the security boundary.
- Pop the assistant out into its own window when a quarter of the screen is too narrow for a long answer, and dock it back when you are done.
- Messages you send while it is working are queued in order and fire as soon as the turn finishes; nothing is dropped.
- Conversation history per profile: start a fresh conversation without losing the old one, and reopen any past conversation from the archive, resuming it where the session allows.
- Session context meter and one-click Compact to summarize and shrink a long conversation, plus per-message timestamps and a live thinking indicator.
- Model and effort controls: choose the Claude model (Default, Opus, Sonnet, Fable, Haiku) and the reasoning effort (Default through Max).
- Grounded in Microsoft Learn: the assistant uses the official Microsoft Learn documentation service for authoritative, current guidance.
- Multiple Claude accounts: keep separate, isolated Claude sign-ins and switch between them.

## Documents

- The assistant builds client-ready Word documents: a cover page, a running header with the classification marking, page numbers, and a table of contents.
- Real Word structure rather than hand-drawn text: tables with repeating header rows, bullet and numbered lists, fact panels, callouts for findings and risks, code blocks, and captioned screenshots.
- Compliance reports carry portal screenshots as evidence, captioned with what they show and when they were captured.

## Tenant state ledger

Each tenant workspace keeps `tenant-state.md`, a high-level record of what exists, what it is for, and how it is protected.

- Fixed sections (identity, groups, licensing, Azure estate, network, workloads and data flows, security posture, logging, compliance, backup, constraints, change history) so tenants are comparable, seeded automatically and never overwriting what is already there.
- Every playbook is wired to it: documentation playbooks read it before gathering and verify live before citing, deployments update it after the change, and reviews enrich it from what they discover.
- Structure and posture only. It records that a group grants privileged access to a system, not that one person joined it.

## File explorer and context

- A VS Code-style file explorer docked on the left, rooted at each profile's workspace plus any project folders you attach.
- Attached folders become context the assistant can read and edit, so it can work directly in a Terraform or Bicep repo you point it at.
- Files open in their real application: a .docx opens in Word, a .pdf in your PDF reader.

## Mailboxes workspace

- A dedicated, cloud-neutral Mailboxes workspace, reached from the banner and walled off from the tenant admin agents.
- Connect your own Microsoft 365 mailboxes across organizations for read-only search and reading.
- Every mailbox stays connected across restarts (refresh tokens are stored encrypted), and regulated mailboxes can be bound to a specific Claude account for data residency.
- The read-only mail tools are also available inside every tenant agent, residency-gated, so you can ask about your own connected mail from anywhere without it touching the tenant.

## Playbook Center

A cloud-neutral workspace, reached from the banner, for a versioned library of best-practice deployment and operations playbooks that the assistant can run against the current tenant.

- Categorized, searchable library (Infra, Identity, Security, Governance, Endpoint, Monitoring, AI, Collaboration, Documentation). Selecting a playbook fills an editable directive you review before sending; the full guide is attached to the run.
- Every deployment playbook ships a validated Terraform composition beside its guide: Azure Verified Modules where they exist, pinned to exact versions, with native resources where no module does. Each one takes a cloud setting that switches provider endpoints and default regions between Commercial and GCC High, so the same code serves both.
- Suites run an ordered program of playbooks end to end (CMMC, IL5, NIST 800-53), completing and verifying each member before the next.
- Always-on doctrine: every deployment follows Azure Landing Zone architecture and applies the Well-Architected Framework five pillars per workload, and integrates with existing infrastructure-as-code and live resources instead of greenfielding over an existing estate.
- A real, user-visible repo at `Documents\Flightdeck\Playbooks`, seeded with built-in defaults that never overwrite your edits. Files a shipped playbook gains later are delivered into folders you already have.
- A purpose-built author agent, grounded in Microsoft Learn, that creates, updates, and refreshes playbooks. It authors files only and has no tenant tools.

## Data residency and security

- Per-tenant siloing: each profile has its own isolated session partition, so tenants stay separated.
- The assistant uses only Flightdeck's own tools; it does not inherit your personal claude.ai cloud connectors, so a GCC High tenant's data never routes through a commercial connector.
- Tokens are only ever sent to the resolved Microsoft endpoint; the assistant refuses to send a token to any other host.
- File access is confined to the profile's roots. Opening, reading, and attaching all enforce it, and attachments are copied inside rather than read in place.
- Interactive and device-code sign-ins are refused inside the shell: they cannot complete there, and every credential path goes through the app's own broker.
- Profiles, tenants, mailbox connections, playbooks, and window state are stored on your PC (under `%APPDATA%\Flightdeck` and `Documents\Flightdeck`).
- Saved passwords and connection tokens are encrypted with your Windows account key (DPAPI).
- Assistant changes are recorded in the profile's audit log.

## Requirements

- Windows 10 or Windows 11, 64-bit.
- For the AI assistant only: a Claude subscription. The assistant runs on the bundled Claude CLI using your own Claude sign-in; sign in once from inside the app. The portals and the vault work without it.
- To run a playbook's Terraform composition: Terraform 1.9 or newer on your PATH. Everything else works without it.
- Appropriate Microsoft admin roles in the tenants you manage. Flightdeck acts as the signed-in administrator; it never elevates your access.

## Install

1. Click the Download button above, or go to the releases page: https://github.com/jadenentropex/flightdeck-releases/releases . The direct link is https://github.com/jadenentropex/flightdeck-releases/releases/latest/download/Flightdeck-Setup.exe and always serves the newest build.
2. Run it. It installs per-user (no admin rights needed) and adds Start menu and desktop shortcuts.
3. Launch Flightdeck, create a profile for a tenant, and open a portal.

The installer is self-contained: it bundles the app, its runtime, and the Claude CLI. Windows SmartScreen may warn that the publisher is unrecognized because the build is not code-signed; choose More info then Run anyway if you trust this source.

## Updates

Installed builds check the public releases repo on launch and download new versions in the background, then install on the next quit. No action is needed.

## Build from source

Requires Node.js 18+ and the repository checked out.

```
npm install
npm start                 # run from source
npm test                  # portal catalog, playbooks, chat markdown, documents, ledger, queue, attachments, approval gate
npm run dist              # build the installer into dist/
npm run release           # build and publish to the releases repo (maintainers; needs GH_TOKEN)
```

`scripts/tf-validate.ps1` runs terraform fmt, init, and validate across every playbook composition.

## License

MIT. See LICENSE.
