# Module 0 Student Guide: AI Power User Fundamentals for Windows 11

Follow along here. Every step, command, and link from the session.

No technical background needed. You will not write code. By the end you have one
folder you can open four different ways, and a computer that types what you say.

---

# Before the session

Download and install these three.

| App | Download from |
|---|---|
| **Handy** | https://handy.computer |
| **Obsidian** | https://obsidian.md |
| **VS Code** | https://code.visualstudio.com |

Then open Handy, go to **Models**, and download a **Whisper** model. Whisper
models support 99+ languages, including Cambodian (Khmer). Whisper Large is the
most accurate. Whisper Turbo or Small are faster on older machines.

You also need: Windows 11, 8 GB RAM, 5 GB free disk.

---

# The idea

Everything today points at one folder:

```
C:\Users\you\Documents\secondBrain
```

You create it in the terminal, browse it in File Explorer, read it in Obsidian,
and inspect it in VS Code. **Four windows, one folder.**

---

# 1. What a terminal is

- **1960s.** A terminal was a physical machine. A keyboard and a paper printer,
  wired to a shared computer in another room.
- **1978.** Glass terminals. A screen and a keyboard, no computer inside.
- **Today.** There is no machine. Your terminal window is an **emulator**,
  software pretending to be a 1970s appliance.

**Why it survived:** a click cannot be saved, repeated, or shared. A command can.
And AI agents produce text, terminals consume text, so the terminal is where you
and an AI can work on the same thing.

## Open source

The source code is published, and anyone can read, change, and share it.

| Tool | Free | Open source |
|---|---|---|
| Handy | Yes | Yes, MIT |
| VS Code | Yes | Code is MIT. Microsoft's build adds telemetry |
| Obsidian | Yes for personal use | No |

Obsidian is not open source and we use it anyway, because your notes are plain
text files in a normal folder. If Obsidian disappeared, every note still opens in
Notepad. **Ask what happens to your files if the company dies.**

---

# 2. The terminal

Press `Windows`, type `terminal`, press Enter.

```
PS C:\Users\you>
```

`PS` is PowerShell. `C:\Users\you` is where you are standing. `>` is where your
typing goes.

## Moving around

| Command | What it does |
|---|---|
| `pwd` | Where am I |
| `ls` | What is in here |
| `cd Documents` | Go in |
| `cd ..` | Go up one |
| `cd ~` | Go home |
| `cls` | Clear the screen |

**Reading a path.** `C:\Users\you\Documents\secondBrain` is nested drawers. A
backslash means "go inside". `~` is shorthand for `C:\Users\you`.

## Two keys that matter most

- **Tab** completes a name you started typing. Use it constantly. It kills typos.
- **Up arrow** brings back your last command.

**`Ctrl+C` stops whatever is running.** Nothing here can hurt your laptop.

## Build your second brain

```powershell
cd ~\Documents
mkdir secondBrain
cd secondBrain
mkdir 1-projects, 2-areas, 3-resources, 4-archives
ls
```

Then right-click the `secondBrain` folder in File Explorer and choose **Always
keep on this device**, so OneDrive keeps a real copy on your machine instead of a
cloud placeholder.

## Files

```powershell
ni note.md                          # new file. There is no "touch" on Windows
"Hello" > note.md                   # write a line into it
cat note.md                         # read it back
cp note.md copy.md                  # copy
mv copy.md renamed.md               # move, and also rename
rm renamed.md                       # delete
```

`rm` does not use the Recycle Bin. Deleted is deleted.

## Terminal tabs and panes

| Action | Keys |
|---|---|
| New tab | `Ctrl+Shift+T` |
| Next tab | `Ctrl+Tab` |
| Split the pane | `Alt+Shift+D` |
| Move between panes | `Alt` plus arrow |
| Close pane or tab | `Ctrl+Shift+W` |
| Clear screen | `Ctrl+L` |

---

# 3. Windows 11 shortcuts

## Copy, paste, cut

| Action | Keys |
|---|---|
| Copy | `Ctrl+C` |
| Cut | `Ctrl+X` |
| Paste | `Ctrl+V` |
| Select all | `Ctrl+A` |
| Undo | `Ctrl+Z` |
| Redo | `Ctrl+Y` |

**One gotcha.** In the terminal, `Ctrl+C` copies **only when text is selected**.
With nothing selected it stops the running command instead. `Ctrl+V` always
pastes.

## Windows and tabs

| Action | Keys |
|---|---|
| Switch between application windows | `Alt+Tab` |
| See all open windows | `Win+Tab` |
| Switch tabs inside an application | `Ctrl+Tab` |
| Previous tab | `Ctrl+Shift+Tab` |
| **New tab** | `Ctrl+T` in most apps, `Ctrl+Shift+T` in Windows Terminal |
| Close tab | `Ctrl+W` |
| Snap window left or right | `Win+Left`, `Win+Right` |
| Show the desktop | `Win+D` |
| Open or switch to a taskbar app | `Win+<number>` |

Learn `Alt+Tab`, `Ctrl+Tab`, `Win+Left` and `Win+Right` and you stop dragging
windows with a mouse.

---

# 4. File Explorer

Open it with `Win+E`.

## Turn these on first

**View**, then **Show**, then tick:

- **File name extensions**
- **Hidden items**

Windows hides the end of filenames by default. It shows `note` when the file is
really `note.md`. Most beginner confusion with files comes from this setting.

## Shortcuts

| Action | Keys |
|---|---|
| Open File Explorer | `Win+E` |
| New folder | `Ctrl+Shift+N` |
| **Rename a file** | `F2`, type the new name, press Enter |
| Up one level | `Alt+Up` |
| Back, forward | `Alt+Left`, `Alt+Right` |
| Address bar | `Ctrl+L` |
| Delete | `Delete`, or `Shift+Delete` to skip the Recycle Bin |
| Properties | `Alt+Enter` |

## Pin a folder to the side panel

Right-click `secondBrain` and choose **Pin to Quick access**. It now sits in the
left sidebar, one click from anywhere. Drag items in the sidebar to reorder them,
and right-click and **Unpin** to remove.

## The bridge: Explorer and terminal are the same place

**Explorer to terminal.** Right-click inside the folder and choose **Open in
Terminal**. Or press `Ctrl+L`, type `wt`, press Enter. The terminal opens already
standing in that folder.

**Terminal to Explorer.**

```powershell
start .
```

The `.` means "here". They were never two places. They are two windows onto the
same folder.

---

# 5. Handy: talk instead of type

## Model

Use a **Whisper** model. Whisper supports 99+ languages, including Cambodian
(Khmer). Pick Whisper Large for accuracy, or Whisper Turbo or Small if your
machine is slower.

Under **Advanced**, add names and terms it keeps getting wrong to **Custom
Words**, and turn on **Post-processing** to clean up the raw text.

## Shortcuts and push to talk

**Settings**, then **Shortcuts**. Click the shortcut and press the keys you want.

| Mode | How it works |
|---|---|
| **Push to talk** | Hold the key, speak, release. Recommended |
| **Toggle** | Press to start, press again to stop |

Also set your **microphone**, turn on **audio feedback** so you hear recording
start, and set **overlay position** to None if the indicator annoys you.

Try it in three places: the terminal, Obsidian, and your browser. **It types
wherever your cursor is**, in any application.

## History

The **History** tab in the settings sidebar holds every past transcription with
its timestamp, the text, and an audio player. **Copy any entry back to your
clipboard with one click.**

Under **Advanced**, then **History**: **History Limit** sets how many are kept,
0 turns it off. **Auto-delete recordings** frees disk space. **Star** an entry to
protect it.

Everything runs on your laptop. Your voice does not go to a company, and it works
with the wifi off.

---

# 6. Obsidian

## Open the folder as a vault

**Open folder as vault**, then pick `Documents\secondBrain`.

Nothing is imported or converted. **A vault is just a folder.**

## PARA

| Folder | What goes in it | The test |
|---|---|---|
| `1-projects` | Has an outcome and an end | Can I tick this off? |
| `2-areas` | Ongoing, no end date | Do I do this forever? |
| `3-resources` | Reference material | Would I want this later? |
| `4-archives` | Finished or dormant | Is this done or dead? |

**PARA sorts by how soon you must act, not by subject.** "Learn Khmer" is an
Area. "Pass the Khmer exam in March" is a Project.

## See all your file types

**Settings**, then **Files and links**, turn on **Detect all file extensions**.

By default Obsidian shows markdown and hides everything else. Turn this on and
your images, PDFs, and audio appear in the sidebar too.

## Links

Type `[[` and Obsidian autocompletes.

```markdown
[[Note Name]]                  link to a note
[[Note Name|display text]]     link with a different label
[[Note Name#Heading]]          link to a section
![[Note Name]]                 embed the note's content here
![[image.png|400]]             embed an image, 400px wide
```

**Linking to a note that does not exist yet is a feature.** Obsidian creates it
the moment you click the link. Write the link first, fill the note in later.

## Backlinks

Every note automatically shows **which other notes link to it**, in the Backlinks
pane. You never maintain this. It is worked out from your files.

Below it, **Unlinked mentions** finds notes that mention this note's title
without linking, and offers to link them in one click.

This is why Obsidian compounds. In a normal folder you have to remember where you
filed something. Here, things find each other.

## Canvas

An infinite whiteboard. Notes, images, and text cards placed in 2D space and
connected with arrows. Create one from the command palette, **Create new canvas**.

- Double-click empty space for a new card
- **Drag a note in from the sidebar** to place the real note on the canvas
- Drag from a card's edge to another card to draw an arrow

**Put content in notes, put relationships on the canvas.** Text typed directly
into a canvas card is much harder to search than text in a note.

Graph view is the map that draws itself. Canvas is the map you draw on purpose.

## Community plugins, and the VS Code plugin

Community plugins are written by other people and can read your whole vault, so
Obsidian keeps them behind a gate.

1. **Settings**, then **Community plugins**
2. Turn off **Restricted Mode**
3. **Browse**, search, **Install**, then **Enable**

Install **VSCode Editor**. It puts the editor engine from VS Code inside an
Obsidian tab, so code files open with syntax highlighting instead of showing
"unsupported file format".

Configure it in its settings: add any file extensions you want it to handle. It
covers `ts, js, py, css, c, cpp, go, rs, java, lua, php` by default.

## Shortcuts

| Action | Keys |
|---|---|
| New note | `Ctrl+N` |
| Jump to any note | `Ctrl+O` |
| Every command by name | `Ctrl+P` |
| Search the vault | `Ctrl+Shift+F` |
| Toggle edit and reading view | `Ctrl+E` |
| Open link in a new tab | `Ctrl+Click` |

---

# 7. VS Code

From the terminal, standing in your folder:

```powershell
code .
```

Third window, same folder. Open the file sidebar with `Ctrl+Shift+E`.

**Obsidian shows your thinking. VS Code shows the files.** Open this window when
something is wrong, a file is hidden, or you want to see what an AI agent changed.

## Install an extension

Press `Ctrl+Shift+X`, search **Markdown All in One**, click Install. That is what
installing a plugin means: a search box and a button.

`Ctrl+Shift+V` previews a markdown file.

## The terminal inside VS Code

Press `` Ctrl+` ``, then `pwd`. It is already in `secondBrain`. Files on the
left, terminal underneath, one folder. This is how most people actually work.

---

# 8. Write your Module 0 note

In Obsidian, inside `2-areas`, create a folder called `ai-power-users`. Inside
it, create a note called `module-00`.

Fill it in, and **dictate some of it with Handy** instead of typing.

```markdown
# Module 00: Windows 11 Fundamentals

## Terminal
pwd     where am I
ls      what is here
cd      go somewhere
mkdir   make a folder
ni      make a file
cat     read a file
cls     clear the screen
start . open this folder in File Explorer
code .  open this folder in VS Code
Tab     complete a name
Up      last command
Ctrl+C  stop what is running

## Copy and paste
Ctrl+C copy, Ctrl+X cut, Ctrl+V paste, Ctrl+Z undo
In the terminal Ctrl+C only copies when text is selected

## Windows
Alt+Tab         switch application windows
Ctrl+Tab        switch tabs inside an app
Ctrl+T          new tab
Win+Left/Right  snap a window
Win+E           File Explorer
Win+D           show desktop

## File Explorer
Turn on File name extensions and Hidden items
Ctrl+Shift+N    new folder
F2              rename a file
Right-click, Pin to Quick access, to pin a folder to the sidebar
Right-click, Open in Terminal. start . goes back the other way

## Handy
Whisper model, 99+ languages including Khmer
Push to talk key: <mine>
History tab keeps every transcription
Custom Words fixes names it gets wrong

## Obsidian
[[double brackets]] link, ![[with a bang]] embeds
Backlinks pane shows who links here
Canvas is a whiteboard. Put content in notes, relationships on the canvas
Detect all file extensions shows non-markdown files
Ctrl+O jump to a note, Ctrl+P command palette

## VS Code
Ctrl+Shift+X    extensions
Ctrl+Shift+E    file sidebar
Ctrl+`          built-in terminal
```

Save it, then find the same file in File Explorer and in VS Code.


---

# Troubleshooting

| Problem | Fix |
|---|---|
| Whisper will not load | Try Whisper Small. If no Whisper model works, your machine cannot run them and you are limited to English-only CPU models |
| Handy types nothing | Check the microphone in Settings, General. Make sure your cursor is in a text field |
| My file is called `note` not `note.md` | Turn on File name extensions in File Explorer, View, Show |
| `cd secondBrain` cannot find the path | Run `ls` to see what is there. Check capitals. Use Tab completion |
| Obsidian shows an empty vault | Wrong folder. Open folder as vault, pick `Documents\secondBrain` |
| Files show a cloud icon and open slowly | Right-click `secondBrain`, Always keep on this device |
| A code file says "unsupported file format" | Install the VSCode Editor community plugin |

---

# Links

- Handy: https://handy.computer
- Handy docs: https://handy.computer/docs
- Obsidian: https://obsidian.md
- VS Code: https://code.visualstudio.com
- PARA method: https://fortelabs.com/blog/para
