# Module 5: Student Guide

**Deploy Your Agent and Sync Your Brain**
Follow along here. Every command, every brief, ready to copy.

By the end you will have a server that runs your agent when your laptop is closed, holding
the same notes and the same skills.

**Local commands are macOS. The server is Ubuntu.**

---

## Before the session

### Check your agent still works

```bash
cd ~/SecondBrain
hermes
```

If that fails, the [Module 3 student guide](../module-03-media-models-and-your-own-domain/student-guide.md)
gets you back to a working agent.

### Accounts

| Account | Why | Cost |
|---|---|---|
| [GitHub](https://github.com) | Holds the vault repository | 0 USD, private repos are free |
| [DigitalOcean](https://www.digitalocean.com) | The server | **About 6 USD a month.** Recurring |

DigitalOcean needs a payment method at signup. New accounts have carried a credit in the
past. Check the signup page for the current offer.

### Bring

- An SSH key on your laptop. Check with `ls ~/.ssh/*.pub`. If nothing comes back, run
  `ssh-keygen -t ed25519`.
- 20 minutes after the session to destroy the Droplet if you do not want to keep it.

---

## Step 1: Measure your vault

```bash
du -sh ~/SecondBrain
find ~/SecondBrain -name "*.md" -not -path "*/node_modules/*" | wc -l
find ~/SecondBrain -name "*.md" -not -path "*/node_modules/*" -exec du -ck {} + | tail -1
find ~/SecondBrain -type d -name node_modules | wc -l
```

Write down the total size and the markdown size. Post the two numbers in chat.

---

## Step 2: Make it a repository

Paste this into your agent:

```
Turn my vault at ~/SecondBrain into a git repository. Do it in this order and stop if any
step fails.

1. Confirm there is no .git directory here yet. If there is, stop and tell me.
2. Write .gitignore FIRST, before any git init and before any commit. Ignore:
   - node_modules/, .venv/, __pycache__/, *.pyc
   - *.mp4, *.mov, *.mp3, *.opus, *.wav, *.jpg, *.jpeg, *.png, *.gif, *.pdf
   - .env and .env.*
   - .obsidian/workspace.json, .obsidian/workspace-mobile.json, .obsidian/cache
   - .DS_Store
3. Run git init.
4. Run git add -A, then git status, and show me the file count and total size that would
   be committed. Do not commit yet.
5. If that number is above 50 MB, stop and tell me what is large. Do not commit.
6. Report any nested .git directories you found inside the vault and leave them alone.

Then wait for me before committing.
```

Read the number from step 4. If it is under 50 MB, tell the agent to commit.

> **The trap.** Git tracks a file from its first commit. Commit the media, then add the
> ignore rule, and the media stays in the repository forever. Getting the order wrong cannot
> be undone without rewriting history.

Create a **private** repository on GitHub and push to it.

---

## Step 3: Move your skills into the repo

```
Move my skills into the vault repository so they travel with it.

1. List what is in ~/.hermes/skills/ and tell me which ones I wrote and which ones shipped
   with Hermes. Only the ones I wrote are moving.
2. Create ~/SecondBrain/.hermes/skills/ and move my own skills there. Copy, verify, then
   remove the original. Do not delete anything until you have confirmed the copy.
3. Run: hermes skills trust
4. Start a fresh session inside ~/SecondBrain and run /skills. Show me the output.
   My moved skills should appear and be tagged as project skills.
5. Commit the result with the message "skills: move to project skills".

If any skill fails to appear in step 4, stop and tell me which one and what the error was.
```

Step 4 is the proof. If a skill does not appear, it did not work.

> **The trap.** Project skills only load for sessions started inside the trusted repository.
> A cron job with a working directory somewhere else loads none of them.

---

## Step 4: Create the Droplet

In the DigitalOcean control panel, **Create > Droplets**:

| Setting | Choose |
|---|---|
| Image | Marketplace, **Docker on Ubuntu** |
| Size | Basic, Regular, **1 GB / 1 vCPU / 25 GB** |
| Authentication | **SSH Key.** Add yours |
| Backups | Weekly |
| Hostname | Anything |

Read the monthly price off the create page before you click.

Then connect:

```bash
ssh root@<your-droplet-ip>
```

---

## Step 5: Harden it

A fresh Droplet is reachable by anyone who finds its IP: root account, password login
allowed, no firewall, nothing watching failed logins. Each block below closes one of those
doors. Run them in this order, on the Droplet.

```bash
adduser --disabled-password --gecos "" hermes
usermod -aG sudo hermes
rsync --archive --chown=hermes:hermes ~/.ssh /home/hermes/
```

`--disabled-password` creates `hermes` with no password at all, so a key is the only way
in. `usermod -aG sudo` gives it the same admin rights root has, through `sudo`, one command
at a time instead of a permanent root shell. The `rsync` line copies `~/.ssh`, meaning the
key that let you in as `root`, into the new user's home and hands ownership to `hermes`.
Your login method does not change: the same key that unlocked `root@` now unlocks `hermes@`.

In the DigitalOcean panel, **Networking > Firewalls**, create a firewall:

- Inbound SSH TCP 22, sources: your IP address only
- No other inbound rules
- Apply it to the Droplet

This firewall runs at the network level, outside the Droplet. It blocks everything except
SSH from your own address before traffic reaches the machine at all, which means it still
protects you even if something you do later inside the server goes wrong.

**Open a second terminal and confirm this works before continuing:**

```bash
ssh hermes@<your-droplet-ip>
```

> **The trap.** Do not close your root session until the line above works. If you lock
> yourself out, the only way back is the DigitalOcean web console.

Now, as `hermes`:

```bash
sudo sed -i 's/^#*PermitRootLogin.*/PermitRootLogin no/' /etc/ssh/sshd_config
sudo sed -i 's/^#*PasswordAuthentication.*/PasswordAuthentication no/' /etc/ssh/sshd_config
sudo systemctl restart ssh
```

Two edits to the SSH daemon's config, then a restart to apply them. `PermitRootLogin no`
removes `root` as a login target entirely, so a leaked root key or a guessed root password
gets nowhere. `PasswordAuthentication no` removes passwords as a login method for every
account, `hermes` included: only a private key that matches an entry in
`~/.ssh/authorized_keys` gets in from here on. The `sed` pattern matches the line whether it
starts commented out or already set to something else, which is why it is safe to run more
than once.

```bash
sudo ufw allow OpenSSH
sudo ufw --force enable
```

`ufw` is a second firewall, running on the Droplet itself rather than at the network level.
Its default is deny everything, so `allow OpenSSH` opens port 22 before `enable` turns that
default on, otherwise you would cut your own SSH session the moment it activates.
`--force` skips the confirmation prompt, which would otherwise hang here waiting for a
keypress. Two firewalls doing the same job is not redundant: the Cloud Firewall still
protects you if `ufw` is ever misconfigured or disabled, and `ufw` still protects you if the
Cloud Firewall rule is ever removed.

```bash
sudo apt update
sudo apt install -y fail2ban unattended-upgrades
sudo systemctl enable --now fail2ban
printf 'APT::Periodic::Update-Package-Lists "1";\nAPT::Periodic::Unattended-Upgrade "1";\n' \
  | sudo tee /etc/apt/apt.conf.d/20auto-upgrades
```

`fail2ban` watches the auth log and temporarily bans an IP after repeated failed login
attempts, so a script working through a list of keys against your one open port gets locked
out after a handful of tries. `unattended-upgrades` installs security patches on its own;
installing the package is not enough to turn it on, so the `printf` line writes the config
file that does. `systemctl enable --now` starts `fail2ban` immediately and keeps it running
after a reboot. Nobody is watching this box day to day, so both of these have to act without
you.

---

## Step 6: Run Hermes

```bash
mkdir -p ~/.hermes
docker run -it --rm -v ~/.hermes:/opt/data nousresearch/hermes-agent setup
```

Give it your model provider key. It writes to `~/.hermes/.env` and persists.

```bash
sudo mkdir -p /opt/brain

docker run -d --name hermes --restart unless-stopped \
  -v ~/.hermes:/opt/data \
  -v /opt/brain:/opt/brain \
  -e HERMES_WRITE_SAFE_ROOT=/opt/data:/opt/brain \
  nousresearch/hermes-agent gateway run
```

> **The trap.** The official image restricts writes to `/opt/data`. Without the
> `HERMES_WRITE_SAFE_ROOT` line, every attempt to write a note in `/opt/brain` fails.

Set three things in `~/.hermes/config.yaml`:

```yaml
approvals:
  mode: manual
tool_loop_guardrails:
  hard_stop_enabled: true
  hard_stop_after:
    exact_failure: 5
    idempotent_no_progress: 5
checkpoints:
  enabled: true
```

Restart it: `docker restart hermes`

---

## Step 7: Clone the brain

Create a GitHub **fine grained personal access token**: Settings > Developer settings >
Personal access tokens > Fine-grained tokens. Scope it to your vault repository only, with
**Contents: Read and write**. Set an expiry.

```bash
sudo chown -R $(docker exec hermes id -u):$(docker exec hermes id -g) /opt/brain
git clone https://github.com/<you>/<your-brain-repo>.git /opt/brain
docker exec -it hermes hermes skills trust /opt/brain
```

Git will ask for a username and password. The password is the token.

Then open a session and paste this:

```bash
docker exec -it hermes hermes
```

```
You are running on my server now. Confirm the setup before we go further.

1. Show me the output of: ls /opt/brain
2. Run /skills and confirm my project skills are loaded and tagged as project skills.
3. Write a file at /opt/brain/Research/server-hello.md containing the current date, the
   hostname, and one sentence saying which machine you are.
4. Read it back and show me the contents.

If step 3 fails with a write error, show me the exact error text. Do not retry it and do
not work around it.
```

---

## Step 8: Close the loop

```
Set up the sync loop on this server.

1. Write /opt/brain/scripts/push-brain.sh. It must:
   - cd to /opt/brain
   - git add -A
   - exit 0 quietly if there is nothing staged
   - commit with the message "agent: " plus an ISO 8601 timestamp
   - git pull --rebase --autostash
   - git push
   - print the short commit hash on success
   - use set -euo pipefail and be safe to run twice in a row
2. Make it executable and run it once. Show me the output.
3. Install a system crontab entry that runs it every 15 minutes and appends both stdout
   and stderr to /var/log/push-brain.log.
4. Show me the crontab afterwards.

Use the system crontab, not Hermes cron. This script has no model in it and does not need
one.
```

On your laptop, install the Obsidian **Git** community plugin and enable pull on vault open.

Then prove it: ask the server agent to write a note, open Obsidian, and read it.

---

## If something breaks

| Symptom | Cause and fix |
|---|---|
| `outside HERMES_WRITE_SAFE_ROOT` | The `-e HERMES_WRITE_SAFE_ROOT=/opt/data:/opt/brain` flag is missing. Stop the container, remove it, run it again with the flag |
| `Permission denied` writing to `/opt/brain` | The container user does not own it. Rerun the `chown` line in Step 7 |
| Locked out of the Droplet | Use the DigitalOcean web console. Do not paste long commands into it, browser consoles corrupt `:` and `@` characters |
| Project skills do not appear in `/skills` | The session did not start inside `/opt/brain`, or `hermes skills trust` was not run |
| `git push` asks for a password and rejects it | GitHub does not accept account passwords. The password field takes the fine grained token |
| The staged commit is hundreds of MB | An ignore rule is missing. Run `git rm -r --cached .` and fix `.gitignore` before committing |
| `docker: command not found` | The Droplet was not created from the Docker Marketplace image |

---

## Homework

1. Destroy and rebuild the Droplet from scratch, timed. Over 20 minutes means your notes
   are not good enough yet.
2. Have the server agent research something overnight and write it to `Research/`. Read it
   on the laptop the next morning without touching the server.
3. Edit the same note on both machines, then resolve the conflict. Write down what you did.
4. Run `docker stats` and report whether 1 GB was the right size.
5. Set a billing alert on your DigitalOcean account.
6. Add a paragraph to `HERMES.md` naming which machine owns which folders, and commit it.

---

## What this cost

| Item | Amount |
|---|---|
| Droplet, 1 GB | About 6 USD a month, recurring until you destroy it |
| Weekly backups | About 1.20 USD a month |
| GitHub private repository | 0 USD |
| Model usage on the server | Whatever it does, on the key you gave it |

To stop the bill: **Destroy** in the Droplet menu. Deleting the container is not enough.
