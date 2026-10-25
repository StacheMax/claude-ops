# claude-ops — instructions for Claude

This repo lets Claude do account-level GitHub tasks that its own tools can't, by pushing request files that a GitHub Action carries out. Currently it does one thing: **create repos**.

## Creating a repo

1. `git pull --rebase` first.
2. Add a file `requests/pending/<repo-name>.json`:

   ```json
   {
     "name": "repo-name",
     "description": "One line about the repo",
     "private": true,
     "auto_init": false
   }
   ```

   - `name` is required: letters, digits, `.`, `_`, `-`; up to 100 characters.
   - `private` defaults to `true`. Only make a repo public if the owner asked for that.
   - `auto_init: true` adds a starter README commit; leave it `false` to get an empty repo.
3. Commit (`ops: request repo <name>`) and push to `main`.
4. Wait about 30–60 seconds, then `git pull`. The request moves to `requests/done/<repo-name>.json` with a `status`:
   - `created` — includes the new repo's `url`
   - `exists` — a repo with that name was already there; nothing was changed
   - `error` — includes the `error` message
5. To work in the new repo, attach it to the session (it is visible to the Claude GitHub app automatically).

Only create repos the owner asked for, or that a task they asked for clearly needs. The workflow cannot delete or modify repos, and that should stay true.

## Setup (already done by the owner)

- Secret `REPO_CREATOR_TOKEN`: a fine-grained personal access token with **Administration: read and write** on all of the owner's repositories. If requests start failing with an authentication or permission error, the token has probably expired — tell the owner to regenerate it and update the secret.
