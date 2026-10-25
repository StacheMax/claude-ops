# claude-ops

Lets Claude create GitHub repos from its cloud sessions, without the token ever passing through chat.

Claude pushes a request file to `requests/pending/`. The **Create repo** workflow (`.github/workflows/create-repo.yml`) creates the repo using the `REPO_CREATOR_TOKEN` secret and moves the request to `requests/done/` with the result. You can browse `requests/done/` to see every repo that was requested and what happened.

The workflow only creates repos. It has no code path that deletes or changes existing ones.

## If it stops working

The token expires on the date you chose when you made it. When it does, requests will finish with `status: "error"`. Regenerate it at **github.com/settings/personal-access-tokens** (same settings: all repositories, Administration read and write), then update the secret under this repo's **Settings → Secrets and variables → Actions**.
