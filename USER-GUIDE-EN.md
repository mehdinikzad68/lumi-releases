# Lumi — User Guide

> 🇮🇷 راهنمای فارسی: [USER-GUIDE-FA.md](USER-GUIDE-FA.md)

*This guide describes Lumi as of v1.9.14.*

## What is Lumi

Lumi is a voice-first AI companion for Android. You can talk to it, type to it,
and ask it to do things on your phone:

- **Chat** with an AI assistant, organized into separate **sessions**
- **Live voice conversations** — talk and listen in real time
- An always-ready **wake word**: just say **«لومی»** ("Lumi")
- **Voice notes** that are transcribed and saved as tidy Markdown files
- **Tasks and reminders**, including tasks Lumi carries out for you at a set time
- **Media control** and a mini-player for whatever is playing
- A durable **Memory** of facts you teach it, and a **Role** text that shapes
  how it behaves in every conversation

Lumi is bilingual: the whole interface is available in **English and Persian**,
and each language is fully self-contained — switch in Settings and everything,
including voice features, follows.

Lumi works with **your own AI provider keys** (bring-your-own-key). There is no
Lumi account and no Lumi server: your keys stay on your phone and your requests
go directly to the provider you chose.

## Install

1. Open the **Releases** page of this repository.
2. Download the APK file from the latest release.
3. Open the downloaded file. Android will ask you to allow installations from
   the app you used to download it (browser or Files) — allow it for that app.
4. Tap **Install**. The setup wizard opens on first launch.

**About the signature:** every official Lumi APK is signed with the same owner
key. Android only accepts an update if the new APK carries the *same*
signature as the installed app, so an APK from any other source will simply be
rejected. Only ever install Lumi from this repository's Releases.

## First run & permissions

The setup wizard has five steps:

1. **Welcome** — what Lumi is, and the terms to accept.
2. **Permissions** — the grants Lumi needs, each explained on screen.
3. **Provider** — add an AI provider key now, or skip and add it later
   (see the next section).
4. **Voice** — pick the live transport and voice options.
5. **Done** — a summary, and you are in.

Why each permission is needed:

| Permission | Why Lumi needs it |
|---|---|
| **Microphone** | Voice notes, dictation, live voice conversations, and the wake word. |
| **Accessibility service** | Lets Lumi operate the phone on your behalf — opening apps and stepping through screens when you ask it to do something. |
| **Notification access** | Lets Lumi see the notifications it needs to act on reminders and notification-based triggers. |
| **Notifications** | So Lumi can show reminders, task results, and status. |
| **Battery exemption** | Keeps the wake-word listener and scheduled tasks alive; without it, Android may put Lumi to sleep in the background. |

You can review or revoke any of these later in Android's Settings. If a feature
loses a permission it depends on, Lumi will tell you instead of failing quietly.

## Add a provider key

1. Open **Settings → AI**.
2. Pick a provider and paste your API key.
3. Save. The key is stored **only in the Android Keystore** on your phone —
   it is never written to a file, a log, or anywhere else, and it is sent only
   to that provider when you use it.

**Providers and model slots.** The provider list is a ready-made
catalogue (in the personal build it starts with **Radium**, whose
`hal-1.0` model is pre-selected for you the first time you fetch its
model list — your own explicit pick always wins). For each role
(**Agent** — the brain, **Vision**, **Screen**, **Text**, **Live**) you
choose a provider profile and a model; a role can hold a small
**rotation pool** that Lumi walks on rate limits or failures. Since
v1.9.12 the Vision, Screen and Text roles **follow the Agent pick by
default** — set the brain once and they come along; you can still give
any of them its own pick, or reset it back to following. Live is the
one exception: it keeps its own pick, because a live session needs a
live-capable model.

What needs a key:

- **Chat** — any supported chat provider.
- **Live voice** — a live-capable provider: **Gemini Live** or **Deepgram**.
  Choose which one Lumi uses under **Settings → AI → Live transport**, and make
  sure that provider's key is saved.
- **Voice-note transcription** — **Deepgram**.
- **On-device features** (wake word, recording notes, reminders) work with no
  key at all.

## Chat & sessions

The **Agent** tab is your home base.

- **Type:** tap the keyboard button on the dock to open the compose box.
  While the box is empty, its button is a **microphone** (dictation — what you
  say becomes text, nothing is sent); once there is text, it becomes **Send**.
- **Talk:** tap the big orange **Live** disc to start a live voice
  conversation (next section).
- **Sessions:** tap the floating **menu (☰) button** at the top of the chat to
  open the session list.
  - **+ New session** starts a fresh conversation with its own context.
  - Each session gets an **automatic title** from your first message; renaming
    it stops the auto-titling.
  - The **⋮ menu** on a session offers **Rename**, **Pin** (pinned sessions
    stay at the top), and **Delete**.
  - Sessions are independent: Lumi only sees the current session's thread,
    plus your durable Memory and Role.
  - While a live conversation is running, session changes are paused —
    you can still rename, but not switch, delete, or clear.
- **Clearing a chat** (in the current session's ⋮ menu):
  - **Clear view** — hides the visible thread, but Lumi still remembers the
    conversation's context.
  - **Clear view + memory** — also drops that context, a true fresh start
    for this session.
  - Clearing **never** touches your notes, tasks, or durable Memory.
- **Long tasks:** a single command runs in segments of up to 12
  action rounds; when a segment ends and work remains, Lumi
  **continues automatically** (up to 4 continuations, 100 executed
  steps in total) and marks each continuation in the run report — you
  never have to say "continue" to get a long job finished.
- **Back key:** Back from any tab returns to the Agent (home) tab. On the
  Agent tab, Back first closes anything open (compose box, session list),
  and then exits the app. Tapping a tab always opens that section's first
  page, not wherever you left it.

## Live voice

Tap the **Live** disc and just talk — Lumi listens and answers out loud, in
Persian or English. The in-session bar lets you **mute**, toggle **captions
(CC)**, and **end** the conversation.

**How Live runs.** Under **Settings → Voice & models** you choose the
**Live execution mode**:

- **Live model** (default) — the live model itself hears you, speaks,
  and acts, in one session.
- **Brain (relay)** — the live model is only the ears and the voice;
  your **Agent brain** does the thinking and the acting, and its reply
  is spoken back to you word for word.

In relay mode you can talk **while Lumi is working**, and what you say
is handled the way a good assistant would:

- A new instruction **waits in a queue** and runs right after the
  current one (this is the default; you can switch to *interrupt
  immediately* in the same settings screen).
- **"Wait"** — or **«صبر کن»** — **pauses** the work exactly where it
  is; **"Continue"** resumes it from the same point.
- **"Stop"** — or **«توقف کن»** / **«لغو کن»** — **stops** the work
  completely. A sentence that starts with a wait word and ends with a
  stop word (for example *"wait, cancel that"*) counts as a stop.
- Thinking out loud — *"let me think"*, **«دارم فکر می‌کنم»** — is
  understood as thinking, not as an instruction: Lumi stays quiet and
  waits for you.
- While the relay is idle, the same words are simply conversation —
  *"cancel my subscription"* is an instruction for the brain, not a
  stop command.

Three voice commands work inside a live conversation:

- **"Note this"** — or say **«این را نوت کن»**. Everything said since the
  conversation started (or since the last time you said it) is saved as a
  note in the Notes panel, and Lumi confirms out loud. Say it again later to
  save only the new part.
- **"Set a task"** — or **«تسک تنظیم کن»**. Lumi works out the task and its
  time, then **reads them back to you** and only creates the task after you
  confirm. Consequential steps are never confirmed automatically — Lumi
  always asks first.
- **"Remember this"** — or **«این را به خاطر بسپار»**. The fact is saved to
  your durable Memory (see *Memory & role*).

## Wake word

Say **«لومی»** ("Lumi") out loud and Lumi wakes up and listens — no need to
touch the phone.

- Listening for the wake word happens **entirely on your phone** (offline,
  on-device speech detection). While Lumi waits for the wake word, no audio
  is recorded, stored, or sent anywhere.
- **Turn it on/off:** Guard tab → **Guard settings → Permissions &
  mechanisms → "Wake word 'Lumi'"**.
- **Is it listening?** The **Status** tab shows a **Wake engine** line:
  *Listening* means the ear is open.
- **Find my phone:** if you've mislaid the phone nearby, say
  **«گوشی کجایی»** ("where's my phone?") out loud — or ask in chat — and
  Lumi makes the phone ring and flash so you can track it down.

## Voice notes

The **Notes** tab is where voice notes live. (The record button exists only
here — plus the home-screen widget, for one-tap capture without opening
the app.)

- Tap **Record**, speak, tap stop. While recording you see a **timer** and a
  **live waveform**; the **✕** throws the take away immediately.
- The note joins the **transcription queue**. Deepgram turns the audio into
  text, and the note ends up as files in your notes folder:
  - the **audio** file,
  - a **raw** Markdown transcript,
  - an **edited** Markdown version, cleaned up for reading,
  - all indexed in a **Lumi Notes Index** file, so the folder works nicely
    with Markdown tools such as Obsidian.
- **Where files go:** by default, `Documents/Lumi-Notes`. To change it:
  Settings → **Notes destination → Choose folder**, and pick any folder.
- **Local only:** switch on **Local only** before recording and that note's
  audio **never leaves your phone** — it is not uploaded for transcription.
  Use it for anything sensitive.
- **Notes from conversations:** when you say *"Note this"* in a live
  conversation, the saved text note appears in this same panel.
- You can open, rename, and delete notes in the panel; renames and deletions
  keep the folder's index consistent automatically.

## Tasks & reminders

The **Tasks** tab shows your list first — no form in the way.

- Tap **+ New task** to open the form: a title, a time, and optionally an
  **instruction** for Lumi.
- At the set time:
  - a **reminder notification** fires, and
  - if the task has an instruction, **Lumi carries it out** and reports the
    result. Steps with real-world consequences pause and ask for your
    confirmation — they are never auto-confirmed.
- You can also create tasks **by voice** in a live conversation
  ("Set a task" — see *Live voice*); Lumi reads the task back before
  creating it.

## Media

Ask Lumi to play, pause, or skip, and it drives the phone's media session
for you.

Whenever something is **playing or paused**, a slim **mini-player drawer**
appears at the bottom of the app:

- **Collapsed** (the default): just the track title and a play/pause button.
- **Tap it or swipe up** to expand: previous / play-pause / next.
- **Swipe down** to collapse it again.
- It remembers whether you left it open or closed, and a new track never
  pops it open by itself. When playback ends, it disappears.

## Memory & role

Open **Settings → Memory & role**.

- **Memory** is the list of durable facts Lumi should always know — for
  example *"The owner's favorite tea is cardamom tea."* Add a fact in the
  **"Something to remember…"** field, or by voice with *"Remember this"*.
  Memory is injected into **every** session, including brand-new ones, so
  Lumi knows these facts without being told again. You can edit any entry,
  and delete one (deletion asks you twice).
- **Role** is a standing instruction text — tone, language, fixed rules —
  that governs how Lumi behaves in all sessions. Edit it and tap
  **Save role**.
- Chat clearing never deletes Memory. The only ways Memory changes are this
  screen and the *"Remember this"* command.

## Updating the app

Lumi updates itself from **this repository's Releases**:

1. When a new version is available, Lumi offers the update (you can also
   check manually in the app's Settings).
2. Tap to download the new APK.
3. Before installing, Lumi **verifies the file's SHA-256 hash** against the
   value published with the release. If it doesn't match, the download is
   discarded.
4. Android's own installer then asks you to confirm — the update is never
   installed silently.

Updates install over the existing app and **keep everything**: notes, tasks,
memory, sessions, and your provider keys. That works because every release
carries the same owner signature (see *Install*).

## Privacy & safety

- **Keys:** provider API keys live only in the Android Keystore and are sent
  only to the provider you configured.
- **Wake word:** detection is fully on-device; the waiting microphone stream
  never leaves the phone.
- **What does leave the phone:** live-voice audio goes to the live provider
  you picked (Gemini or Deepgram), and note audio goes to Deepgram for
  transcription — unless the note is marked **Local only**. Chat messages go
  to your chat provider. Nothing goes to any Lumi server; there isn't one.
- **Banking blocklist:** Lumi refuses to operate inside banking apps.
- **Confirmation gates:** actions with real consequences — sending, deleting,
  changing things — stop and ask for your explicit confirmation first.
- **Audit log:** Lumi keeps an on-device log of what the agent did, which
  you can review under **Diagnostics → Logs**.
- **Desktop bridge pairing:** if you pair Lumi with the desktop bridge
  (Settings → Pairing), the address matters: `wss://` is accepted
  anywhere; plain `ws://` is accepted **only** for this device or your
  Tailscale network and shows an "unencrypted connection" warning —
  a public `ws://` address is refused, because your pairing token
  would travel unencrypted.
- **Kill switch:** the Guard section has an emergency stop that halts the
  agent immediately; while stopped, a red banner in the Agent panel says so.

## Troubleshooting

**Wake word is not listening**
Check the **Status** tab's *Wake engine* line. If it says *Not listening*:
make sure the toggle is on (Guard → Guard settings → Permissions &
mechanisms → "Wake word 'Lumi'"), the **microphone permission** is granted,
and Lumi has its **battery exemption** — aggressive battery saving is the
most common reason the listener gets killed. After changing permissions,
reopen the app.

**An update is not offered**
Updates are published as Releases in this repository. Check that you're
online, then try the manual check in Settings. If you originally installed
Lumi from somewhere else, Android will reject updates whose signature
doesn't match — reinstall from this repository's Releases page.

**A note is stuck on "Awaiting transcription"**
Transcription needs an internet connection and a saved **Deepgram key**
(Settings → AI). Local-only notes never enter the queue — that's by
design.

**A transcript is in the wrong language**
Transcription follows the language you actually spoke; Persian and English
are both supported. If you spoke Persian but the note is garbled, check the
**raw** Markdown file — the edited version is a cleaned-up copy of the same
transcript, so a bad raw transcript means the audio itself was unclear
(noise, distance). Re-record closer to the microphone.

**Live voice won't start**
Live voice needs a live-capable provider key (Gemini or Deepgram) saved in
Settings → AI, the matching **Live transport** selected there too, and a
working internet connection.

**Lumi forgot our conversation after I cleared the chat**
That's the difference between the two clear options: **Clear view** keeps
the context, **Clear view + memory** deliberately drops it. Facts you want
Lumi to keep forever belong in **Memory & role** — those survive every
clear.
