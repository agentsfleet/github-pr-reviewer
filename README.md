# GitHub Pull Request reviewer

> [!IMPORTANT]
> **This repository is sunset and archived.**
> [`agentsfleet`](https://github.com/agentsfleet/agentsfleet) now allows a user
> to author their own fleets directly, so this standalone Fleet Bundle is no
> longer maintained. Use
> [github.com/agentsfleet/agentsfleet](https://github.com/agentsfleet/agentsfleet)
> instead. The content below is kept for reference only.

`github-pr-reviewer` is an [`agentsfleet`](https://agentsfleet.net) Fleet
Bundle. It reviews GitHub pull requests and posts focused review comments.

The fleet checks changes for correctness bugs, security risks, and missing
tests. It can read pull request data and post comments. It cannot push, merge,
approve, or close a pull request.

## Bundle contents

| File | Purpose |
|---|---|
| `SKILL.md` | Defines the review goal, steps, and safety limits. |
| `TRIGGER.md` | Declares the GitHub event, tool, credential, network, and budget policy. |

Both files use `github-pr-reviewer` as the bundle name. `agentsfleet` rejects a
bundle when these names differ.

## Before you begin

You need Node.js 24 or later. You also need an `agentsfleet` account and a
GitHub repository that you can manage.

Create a GitHub token that can read the repository and post comments. Create a
random webhook secret with at least 32 characters.

## Install

1. Install the `agentsfleet` command-line interface (CLI).

   ```bash
   npm install --global @agentsfleet/cli
   ```

   ```text
   added <varies> packages in <varies>
   ```

2. Sign in through your browser.

   ```bash
   agentsfleet login
   ```

   ```text
   Login session
   session_id: <varies>
   login_url: <varies>
   browser: opened
   login complete
   ```

3. Save the GitHub token and webhook secret in your active workspace.

   Replace `<GITHUB_TOKEN>` and `<GITHUB_WEBHOOK_SECRET>` with the values you
   created above.

   ```bash
   agentsfleet secret create github --data='{"token":"<GITHUB_TOKEN>","webhook_secret":"<GITHUB_WEBHOOK_SECRET>"}'
   ```

   ```text
   ✓ Secret 'github' stored in vault.
   ```

4. Install the fleet from the platform library.

   ```bash
   agentsfleet install --library github-pr-reviewer
   ```

   ```text
   ✓ github-pr-reviewer is live.
     Fleet ID: <varies>
     Webhook URLs (register on the upstream provider):
       github: <varies>
   ```

5. Open **Repository settings → Webhooks → Add webhook** in GitHub.

   Use the GitHub URL from step 4 as the payload URL. Choose
   `application/json`, enter the secret from step 3, and select **Pull
   requests** as the event.

## Verify it works

Open a pull request in the connected repository. GitHub sends the event after
you open the pull request.

Replace `<FLEET_ID>` with the identifier from the install output.

```bash
agentsfleet logs <FLEET_ID>
```

```text
Event Stream
  <varies>  webhook:github  <varies>
```

The event stream should show a GitHub event. The pull request should contain
the fleet's review comments.

## Repository ownership

This repository owns only the `github-pr-reviewer` Fleet Bundle.

- [`agentsfleet/platform-ops`](https://github.com/agentsfleet/platform-ops)
  owns the `platform-ops` Fleet Bundle.

An installation skill teaches Claude Code, Codex CLI, Amp, or another host how
to install a fleet. It is not part of the fleet that runs inside `agentsfleet`.

## License

MIT. See [`LICENSE`](LICENSE).
