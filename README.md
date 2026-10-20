# Flutter UI/UX & Zero-Problems Skill (`flutter-ui-design-standards`)

> **🤖 AI AGENT DIRECTIVE (FOR CLAUDE CODE, GOOGLE ANTIGRAVITY, CURSOR, & AI ASSISTANTS):**
> If the user has provided you with the link to this repository, your immediate task is to **read, adopt, and enforce** the rules defined in this repository for all Flutter/Dart development in the current workspace.
>
> ### Action Required by AI Agent:
> 1. **Fetch & Read the Skill:** Locate and read `.claude/skills/flutter-ui-standards/SKILL.md` (or `SKILL.md` in the root directory if placed there).
> 2. **Install Locally (If Requested):** If the user asks you to install or add this skill to their current Flutter project:
>    - Copy the `SKILL.md` file into `.claude/skills/flutter-ui-standards/SKILL.md` (for Claude Code) and/or `.antigravity/rules.md` (for Google Antigravity).
> 3. **Strict Enforcement:** Treat every rule inside `SKILL.md` as a **non-negotiable system constraint** before writing, refactoring, or styling any Flutter widget, screen, dialog, or bottom sheet.

---

## 📌 Overview

This repository hosts a specialized **AI Agent Skill (`SKILL.md`)** designed for **Flutter & Dart** mobile and cross-platform applications. It eliminates common AI-generated UI inconsistencies, localization bugs (specifically for bilingual **Persian RTL** & **English LTR** apps), layout overflows, and accumulated IDE linter warnings in **VS Code**.

---

## 🎯 What This Skill Enforces (Summary for AI & Developers)

When active, the AI assistant MUST strictly follow these 14 core pillars:

1. **Bilingual Font Management (`fa` & `en`):** Always use the custom fonts located in `assets/font/` (or `assets/fonts/`) configured in `pubspec.yaml`, switching dynamically based on the active `Locale` (`fa` vs `en`).
2. **Standardized AppBar Typography:** Keep toolbar/page titles proportional (`16.0`–`18.0`, Medium/SemiBold). Never use oversized display headers.
3. **Unified Back Button:** Use a single, consistent back icon across the entire app. Never mix `arrow_back`, `arrow_back_ios`, and `CupertinoIcons.back`.
4. **BottomSheet SafeArea & Symmetrical Actions:** Prevent system navigation bar overlap on Android/iOS (`SafeArea` / `viewPadding.bottom`) and place dual action buttons side-by-side in a `Row` with matching dimensions and cohesive styling.
5. **Compact Confirmation Dialogs:** Enforce clean, standard font sizes for dialog titles (`15–16`), body text (`13–14`), and compact buttons (`40–44` height).
6. **Zero Unrequested Icons:** Never add decorative icons next to titles, headers, or text labels without explicit user permission.
7. **Strict RTL/LTR Directional Layouts:** Exclusively use directional APIs (`EdgeInsetsDirectional`, `AlignmentDirectional`, `PositionedDirectional`) so Persian and English layouts never break.
8. **Locale-Aware Number & Unit Formatting:** Properly format numbers/currencies with thousands separators and clean spacing in both RTL and LTR contexts.
9. **Clean Visual Aesthetic:** Avoid unrequested neon gradients and heavy drop shadows; stick to theme colors and subtle borders/elevation.
10. **Unified Border Radius & 8pt Grid:** Maintain consistent corner radii across buttons (`10–12`), cards (`12–16`), and sheets/modals (`16–20`).
11. **Keyboard Dismissal & Overflow Safety:** Dismiss keyboards on outside tap and prevent `RenderFlex` bottom overflow errors on smaller screens.
12. **Stable Button Loading States:** Preserve exact button width/height during async loading states and disable double-taps.
13. **Zero Emojis in UI Copy:** Never inject emojis into UI labels, SnackBars, dialogs, or empty states unless explicitly asked.
14. **Real-Time VS Code "Problems" Resolution (Zero-Debt Rule):** Inspect IDE diagnostics (`get_diagnostics` / `flutter analyze`) immediately after editing any file. Proactively prevent:
    - `.withOpacity()` -> Use `.withValues(alpha: ...)`
    - `surfaceVariant` -> Use `surfaceContainerHighest`
    - `MaterialStateProperty` / `WillPopScope` -> Use `WidgetStateProperty` / `PopScope`
    - `use_build_context_synchronously` -> Always guard async gaps with `if (!context.mounted) return;`
    - `unnecessary_import` -> Do not import `dart:typed_data` when `package:flutter/foundation.dart` or `material.dart` is imported.
    - `depend_on_referenced_packages` -> Verify every imported package is explicitly declared under `dependencies:` in `pubspec.yaml`.
    - `unnecessary_underscores` -> Use single `_` wildcards `(context, _, _)` instead of `__` or `___`.

---

## 🚀 How to Use with AI Agents

### Option 1: Direct URL Prompting
Simply paste your repository link into **Claude Code** or **Google Antigravity** and say:
> *"Read the skill in this repository and install/apply its rules to this Flutter project: `https://github.com/<your-username>/<your-repo>`"*

### Option 2: One-Line Terminal Installation (Inside Any Flutter Project)
Run this command in the root of any Flutter project to download the skill directly into both `.claude` and `.antigravity` directories:

```bash
mkdir -p .claude/skills/flutter-ui-standards .antigravity && \
curl -fsSL [https://raw.githubusercontent.com/](https://raw.githubusercontent.com/)<YOUR_GITHUB_USERNAME>/<YOUR_REPO_NAME>/main/.claude/skills/flutter-ui-standards/SKILL.md \
-o .claude/skills/flutter-ui-standards/SKILL.md && \
cp .claude/skills/flutter-ui-standards/SKILL.md .antigravity/rules.md
