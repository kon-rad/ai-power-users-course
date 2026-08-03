# Module 1 — Live Stream Script

**Day 1 · Module 1 of 15 · ~45 minutes · YouTube Live**

Host-facing script. Spoken lines are in plain text. `[Actions]` in brackets are
what to do on screen. Commands to type are in code blocks. Adapt wording freely —
the timings are the guardrails.

Pin the student guide link in chat before you go live.

---

## 0:00–0:05 · Welcome & course intro

Welcome to AI Power Users, from the Global Builders Club. This is Day 1, Module 1
of 15, and I'm really glad you're here.

Here's how this works. Five days, Monday to Friday. Three hours a day, and each
day is three 45-minute modules with a 15-minute break in between. Every session
is live, and every session is hands-on. You don't just watch me — you build
alongside me, and you finish each module with something real on your own machine.

This is a peer-to-peer course. You'll teach each other, you'll do exercises and
short assignments, and you'll grade each other's work against a shared rubric. So
a few values set the tone for the whole week: win and help win, criticise in
private and praise in public, create more than you consume, done over perfect,
and just... participate. Show up and try things.

Today's mission: by the end of these 45 minutes, you'll have your own **private
AI second brain** — a personal AI agent that lives inside your own notes, runs on
a private model, and works even when you close the laptop lid and open it on a
plane. No single company in the middle.

[Show the student guide on screen. Remind: "If you haven't installed anything yet,
that's okay — start now, downloads run in the background while we talk."]

## 0:05–0:08 · The big picture

Let me show you what we're building before we build it.

[Show the diagram from the student guide.]

Five pieces. **Handy** lets you talk instead of type, and it runs fully offline.
**Obsidian** is where you see and organise your notes — it's just Markdown files
in a folder. **Hermes** is the AI agent; it lives inside that folder and can read
and write your notes and run tasks. **Morpheus** is the private brain — the large
language model that powers the agent, running on a decentralized network instead
of one company's servers. And **cmux** is the terminal we launch our agents in.

The magic is how they connect: your notes are a folder, the agent runs *in* that
folder, and the model answers privately. That's the whole idea. Let's build it.

## 0:08–0:14 · Install the tools

We'll kick off all the installs first, then explain each while it downloads.

[Obsidian] First, Obsidian. Go to obsidian.md, download, install. It's free for
personal use. This is your window into your second brain — it turns a folder of
text files into a searchable, linkable knowledge base with a graph view.

[Handy] Next, Handy — your offline speech-to-text.

macOS:
```bash
brew install --cask handy
```
Windows:
```powershell
winget install cjpais.Handy
```
Or download from handy.computer. Handy is free and open source. You press a
shortcut, talk, release, and it types what you said into whatever app you're in —
and your voice never leaves your machine.

[VS Code] Also grab Visual Studio Code from code.visualstudio.com — free and open
source. Obsidian is how we *read* our notes; VS Code is how we *inspect the
machinery* — the actual skill files, the config, and any code the agent writes.
Two windows into the same folder.

[cmux] On macOS, grab cmux from github.com/manaflow-ai/cmux — it's a terminal
built for running AI agents, free and open source. On Windows or Linux, your
normal terminal works fine; I'll point you to alternatives in the guide.

[Hermes] Finally, install the agent. In your terminal:
```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```
(Windows PowerShell version is in the guide.) This sets up everything Hermes
needs. While that runs — Hermes is an open-source agent from Nous Research. Think
of it as a tireless assistant that can actually *do* things in your folder, not
just chat.

[Let installs run. "Leave those cooking. Let's go get the brain."]

## 0:14–0:20 · Private LLM: Morpheus

Every agent needs a model behind it. We're using Morpheus — a decentralized,
private network of LLMs. Instead of your prompts going to one big company,
they're served across an open network, and the private providers run in secure
enclaves.

[Open app.mor.org] Go to app.mor.org and create an account, then log in.

[Create API key] Now create an API key. Copy it somewhere safe — treat it like a
password, we'll talk a lot more about key safety later in the week. There's a free
way to get started, and you can test the available models right here in the app —
note the name of a model you like, we'll need it in a minute.

The key thing: Morpheus gives you an OpenAI-compatible endpoint. That means any
tool that speaks the common AI "language" can use it — including Hermes. The
address is `https://api.mor.org/api/v1`. Keep that handy.

## 0:20–0:28 · Build the second brain

Now the brain itself. Your second brain is just a folder — but we'll organise it
with a method called **PARA**: Projects, Areas, Resources, Archives.

[Create the folder + subfolders — show terminal or Finder.]
```bash
mkdir -p ~/SecondBrain/{1-Projects,2-Areas,3-Resources,4-Archives}
```
Projects are things with a deadline and an outcome. Areas are ongoing
responsibilities. Resources are topics and references. Archives are the stuff
you're done with. That's it — four buckets, and everything you capture goes in
one of them.

[Open Obsidian → "Open folder as vault" → choose ~/SecondBrain.] Now open Obsidian
and point it at that folder. Boom — your folder is now a visual, linked knowledge
base. Create one note in `1-Projects` for something you're actually working on;
we'll use it in a second.

## 0:28–0:36 · Run & connect the agent

Here's where it comes alive. We run the agent *inside* the brain folder, so it can
see and edit your notes.

[Open cmux / terminal, cd into the folder.]
```bash
cd ~/SecondBrain
```
Now connect Hermes to Morpheus. We tell it the private endpoint and your key:
```bash
export OPENAI_BASE_URL="https://api.mor.org/api/v1"
export OPENAI_API_KEY="paste-your-morpheus-key-here"
```
Then launch:
```bash
hermes
```
[When Hermes starts, set the model to the Morpheus model you noted.]
```
/model openai:PASTE-MORPHEUS-MODEL-NAME
```
Let's say hello:
```
Read the note in 1-Projects and tell me what it's about in one sentence.
```
[Agent reads the file and answers.] That right there is the whole thing working:
your private model, reading your private notes, from your own folder.

## 0:36–0:43 · Your first skill: the daily standup

Last piece, and it's the fun one. We're going to teach the agent a reusable
**skill** — a daily standup, where it reviews your projects, goals, and progress
with you, like a stand-up meeting with your AI.

Hermes skills live in `~/.hermes/skills/`, and each becomes a slash command.

[Create the skill file — show the student guide's SKILL.md content.]
```bash
mkdir -p ~/.hermes/skills/daily-standup
```
[Paste the SKILL.md from the guide into `~/.hermes/skills/daily-standup/SKILL.md`.]

The skill tells the agent: look at my Projects, ask me three questions — what did
I do, what's next, what's blocking me — and save a dated standup note in my Areas
folder.

[Back in Hermes:]
```
/daily-standup
```
[Agent asks the questions. Here's the Handy moment:] Instead of typing my answers,
I'll just talk. [Press the Handy shortcut, speak an answer, release — it types.]

[Agent writes the note. Open it in Obsidian.] And there's today's standup, saved
in your second brain, written by talking to a private AI. You now have a routine
you can run every morning for the rest of the course — and the rest of your life.

## 0:43–0:45 · Recap & what's next

Look at what you just did: local speech-to-text, a PARA second brain in Obsidian,
VS Code to inspect the machinery, a private agent running on a decentralized
model, and your first custom skill. That's a real private-AI workstation, and
it's yours.

Module 2, after the break, is pure hands-on — we'll live in this setup, run the
agent on real tasks, and fix whatever didn't connect. Module 3 is the peer lab,
where you'll explain this in your own words and take a short peer-graded quiz.

Homework's in the guide: run your standup twice, capture five real notes, and do
the quiz. Bring your open-ended answers to the peer lab.

Grab your 15-minute break, stretch, hydrate — see you in Module 2.

[End stream segment.]
