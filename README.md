# Ground Truth — Git as Agent Coordination Protocol

You’ve tried agent frameworks, chat protocols, message buses, and task queues. They work in demos but fail with more than a few agents in real codebases.

Ground Truth isn’t another runtime. It’s a way to coordinate agents using the protocol that already works at scale: Git.

---

## What This Is

A minimal, open-source agent runtime built on Git and GitHub. Each agent is a GitHub user with a Personal Access Token. Coordination happens through commits, pull requests, and comments—not custom chat rooms or RPC.

You define agent personalities, assign them tokens, and let them work through Git operations. The rest is standard GitHub: permissions, audit logs, review workflows, and forks.

---

## How It Works

1. **Agents as GitHub users**  
   Each agent has its own GitHub account and token. The token defines its identity and access.

2. **Coordination through Git**  
   Agents communicate by making commits, opening pull requests, and commenting. Presence is visibility in the repository graph.

3. **Copilot for cost splitting**  
   If an agent has a Copilot subscription, it can offload boilerplate code generation while retaining strategic control.

4. **No custom protocol**  
   Everything uses existing Git tooling and GitHub’s API. No new message formats or state synchronization layers.

---

## Quick Start

Deploy to Cloudflare Workers:

```bash
git clone https://github.com/your-org/ground-truth
cd ground-truth
npm install
npx wrangler deploy
```

Create a `config.json` with your agents:

```json
{
  "agents": [
    {
      "name": "flux",
      "token": "github_pat_...",
      "personality": "Lead engineer, pragmatic, ships fast"
    }
  ]
}
```

Run an agent cycle:

```bash
curl -X POST https://your-worker.workers.dev/cycle \
  -H "Authorization: Bearer YOUR_SECRET"
```

Agents will check assigned repositories and act based on recent activity.

---

## When to Use This

- You already use GitHub and want agent coordination without new infrastructure.
- You need agents to work asynchronously over hours or days, not seconds.
- You want permissioning and audit logs that integrate with existing tools.

---

## Limitations

This approach assumes your agents can operate within Git’s commit–review–merge cycle. It is not suitable for real-time, sub-second coordination. Token management and GitHub rate limits are your responsibility.

---

## Why It Works

Git is a battle-tested distributed coordination system. GitHub provides identities, permissions, and review workflows. Using them directly means you get 15 years of tooling for free.

Agents don’t need another chat protocol. They need a way to propose changes, discuss them, and merge—exactly what Git already does.

---

## License

MIT

---

**Attribution**  
Superinstance & Lucineer (DiGennaro et al.)

Part of the Cocapn Fleet  
[Explore the Fleet](https://the-fleet.casey-digennaro.workers.dev) • [Learn about Cocapn](https://cocapn.ai)