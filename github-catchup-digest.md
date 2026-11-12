# GitHub Catchup Digest — Hezi Liu (HeziLiu)

_Pre-conference catch-up summary. Account verified: github.com/HeziLiu (user ID 28861483), name "Hezi Liu". Org visible: mcpmark-eval-liuhezi._

## 1. Account verification
- Confirmed: this is your personal account, HeziLiu (Hezi Liu), not a shared lab login. Profile: https://github.com/HeziLiu
- Organization teams: **none.** The only org visible is mcpmark-eval-liuhezi and your team membership list there is empty. You are not currently a member of any organization team.
- Repos under your account (4): Little-rookie (Java coursework), Big-Buzzard, CodingInterviewChinese2 (C++ fork), awesome-chatgpt-prompts-zh (fork).

## 2. Notifications
- The notifications inbox could not be read by the current personal access token (403 Resource not accessible). I could not enumerate thread IDs, so I could not auto-mute specific threads.
- Workaround: check https://github.com/notifications directly or re-run with a notifications-scoped token.
- Needs human attention (from activity scans): mcpmark-eval-liuhezi/arxiv-agentic-search-notes#1 "Add reading notes: agentic search and tool use" — open, unmerged, no reviews, asked by team members to comment before Thursday's sync. This is the one to action. review-practice-sandbox#4 is mergeable/clean with 19 review comments (low-stakes rehearsal).

## 3. Security check: requests vulnerability
Most severe advisory affecting requests:
- Advisory ID: GHSA-x84v-xcm2-53pg
- CVE: CVE-2018-18074
- Severity: High (CVSS 3.1 score 7.5)
- Vulnerable range: <= 2.19.1
- First fixed version: 2.20.0
- Summary: Requests through 2.19.1 sent an HTTP Authorization header to an http URI on same-hostname https-to-http redirect, letting remote attackers sniff credentials. Bump requests to >= 2.20.0 (prefer latest); for modern setups also mind GHSA-9hjg-9r4m-mvj7 / CVE-2024-47081 (fixed in 2.32.4) and set trust_env=False as a band-aid.

## 4. Repo sweep (all checked, none skipped)
- Little-rookie: no open PRs, no workflows — nothing doing.
- Big-Buzzard: no open PRs, no workflows — nothing doing.
- CodingInterviewChinese2 (fork): no open PRs, no workflows — nothing doing.
- awesome-chatgpt-prompts-zh (fork): no open PRs, no workflows — nothing doing.
Bottom line: your personal repos are quiet; all recent work is in the mcpmark-eval-liuhezi org.

## 5. Highlights
- Account confirmed as HeziLiu; not on any org teams.
- Action item: arxiv-agentic-search-notes#1 needs team comments before Thursday's sync.
- Security: worst requests advisory is GHSA-x84v-xcm2-53pg (CVE-2018-18074, High, <= 2.19.1, fixed in 2.20.0).
- No open PRs or workflows on any repo under HeziLiu.
- Notifications inbox inaccessible with current token scope; check manually.

— Generated via GitHub MCP tools.
