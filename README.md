# Ground Truth — Coordinate Agents with Git

You have probably tried an agent framework. They often work in demos but fail when you add multiple agents to a real project. You end up managing custom message queues and state instead of building.

This is a different approach. It uses Git—a protocol that scales to millions of users—as the coordination layer for your agents. 🛠️

## Why Git?
Every new agent system rebuilds the same coordination primitives: locking, history, identity, and review. Git already solves these. This project uses Git's native operations—commits, pull requests, and reviews—as the sole communication channel between agents. There is no custom state to manage.

## Quick Start
This is designed for you to fork and own. There is no upstream lock-in.

1.  **Fork this repository.**
2.  Clone your fork and deploy it to Cloudflare Workers (zero dependencies, MIT licensed).
    ```bash
    git clone https://github.com/your-account/ground-truth
    cd ground-truth
    npx wrangler deploy
    ```
3.  Configure your agents in `config.json`. Each uses a real GitHub account.
    ```json
    {
      "agents": [
        {
          "name": "engineer",
          "token": "github_pat_...",
          "personality": "Writes pragmatic, tested code."
        }
      ]
    }
    ```
4.  Start a coordination cycle.
    ```bash
    curl -X POST https://your-worker.workers.dev/cycle \
      -H "Authorization: Bearer YOUR_SECRET"
    ```

## How It Works
This is not a runtime. It is a worker that triggers your agents, which then act directly on your repository via the GitHub API.

*   **No Custom State:** The worker is stateless. All coordination state—task assignments, discussions, approvals—lives in the Git graph (commits, PRs, reviews). If the worker disappears, you lose no data.
*   **Debuggable with Familiar Tools:** Every agent action is a commit, comment, or PR status. You debug interactions using `git log` and GitHub's UI, not a proprietary dashboard.
*   **Agents Act Like Teammates:** Agents are assigned issues, branch off `main`, open draft PRs, request reviews, and merge changes. The workflow mirrors a human team.

## When to Use This
✅ You use GitHub for development.
✅ You need multiple agents to work asynchronously on code or documents.
✅ You want a transparent, persistent audit trail without maintaining new infrastructure.

❌ This coordinates work over hours or days, not seconds. A single cycle (issue→PR→merge) typically takes 2-5 minutes due to API latency and agent reasoning time.
❌ You must manage GitHub tokens and rate limits.

## Limitations
The system is bound by Git's operations. It cannot guarantee real-time responsiveness. Agent performance is dependent on the underlying model (e.g., GPT-4) and subject to its reasoning errors, which will be visible in the commit history.

---

<div style="text-align:center;padding:16px;color:#64748b;font-size:.8rem"><a href="https://the-fleet.casey-digennaro.workers.dev" style="color:#64748b">The Fleet</a> &middot; <a href="https://cocapn.ai" style="color:#64748b">Cocapn</a></div>