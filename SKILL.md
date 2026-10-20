---
name: flutter-ui-design-standards
description: Strict Flutter UI/UX design rules, localization font handling, 8pt spacing system, extreme contrast typography, anti-AI aesthetics, smooth micro-animations, memory leak prevention, error boundaries, performance optimization, and real-time VS Code diagnostics verification.
---

# Flutter UI/UX Design Standards & Constraints

You must strictly enforce the following UI/UX and code-quality constraints whenever generating, refactoring, or styling UI code in this project. Never deviate from these rules unless explicitly instructed by the user.

## 1. Localization & Locale-Specific Typography
- **Asset Font Enforcement:** Always inspect `assets/font/` (or `assets/fonts/`) and `pubspec.yaml` to identify the exact custom font families configured for the project.
- **Dynamic Locale Switching:** In multi-language apps supporting Persian (`fa`) and English (`en`), never hardcode a single font family across the entire app if distinct fonts exist for each language. Dynamically resolve the `fontFamily` in `ThemeData` (and custom `TextTheme` styles) based on the active `Locale`:
  - Apply the designated Persian font family from `assets/font` when `locale.languageCode == 'fa'`.
  - Apply the designated English font family from `assets/font` when `locale.languageCode == 'en'`.
- **Fallback Prevention:** Ensure every custom `TextStyle` inherits or explicitly sets the active locale's font family so the app never falls back to raw system fonts.

## 2. Page Titles & AppBar Typography
- **Standardized Sizing:** Never use oversized or exaggerated font sizes for screen titles in `AppBar` or page headers.
- **Size Constraints:** Keep page title font sizes strictly within the standard mobile scale (`16.0` to `18.0`, with `FontWeight.w600` or `FontWeight.w500`). Never use `headlineLarge` or `headlineMedium` for toolbar titles.

## 3. Unified Back Button Iconography
- **Strict Consistency:** Every screen featuring a back button in the toolbar (`AppBar.leading`) must use the exact same icon asset or `IconData` across the entire application.
- **No Mixing:** Never mix `Icons.arrow_back`, `Icons.arrow_back_ios`, `Icons.arrow_back_ios_new`, or `CupertinoIcons.back` across different screens.
- **Implementation:** Use a single shared `CustomBackButton` widget or rely on the global `AppBarTheme` / `ActionIconTheme` to guarantee 100% visual uniformity and proper RTL/LTR directional mirroring.

## 4. BottomSheet Layout, SafeArea & Action Buttons
- **System Navigation Bar Clearance:** Every `ModalBottomSheet` or persistent bottom sheet MUST account for system UI overlays (especially the Android 3-button / gesture navigation bar and iOS Home Indicator).
  - Always wrap bottom sheet content inside a `SafeArea(top: false, bottom: true)` or explicitly add `MediaQuery.of(context).viewPadding.bottom` (and `viewInsets.bottom` when keyboards appear) to the bottom padding.
  - Set `isScrollControlled: true` and `useSafeArea: true` when necessary to prevent clipping or hiding content under the system nav bar.
- **Action Button Alignment & Styling:**
  - When a bottom sheet contains two action buttons (e.g., Confirm and Cancel), place them **side-by-side horizontally in a `Row`** using equal `Expanded` wrappers and a consistent 8pt grid gap (e.g., `SizedBox(width: 8)`). Never stack them vertically with mismatched styles.
  - Both buttons must share the exact same height (`44.0` to `48.0`), border radius, and text size, with cohesive color pairing (e.g., a primary filled button next to a matching secondary tonal/filled button of identical dimensions).

## 5. Confirmation Dialogs (AlertDialogs)
- **Compact & Standard Typography:** Keep typography inside confirmation modals clean, proportional, and never oversized:
  - **Dialog Title:** `15.0` to `16.0` (`FontWeight.w600`). Never use large display sizes.
  - **Dialog Body/Content:** `13.0` to `14.0` (`FontWeight.w400`) with balanced line height (`height: 1.5`).
  - **Dialog Action Buttons:** `13.0` to `14.0` (`FontWeight.w500` or `w600`) with compact, uniform button heights (`40.0` to `44.0`). Avoid bulky padding or oversized button styles inside dialogs.

## 6. Strict No-Decorative-Icons Policy
- **Text-Only Headers & Labels:** Do NOT place icons next to page titles, section headers, dialog titles, bottom sheet titles, or general text labels.
- **Explicit Permission Required:** Never add decorative leading/trailing icons to text elements to "enhance" the UI unless the user explicitly asks for an icon in that specific location.

## 7. Strict RTL/LTR Directional Layout Rules
- **Directional Widgets Only:** Always design layouts to seamlessly support both Persian (RTL) and English (LTR) without visual breakage:
  - Use `EdgeInsetsDirectional.only(start: ..., end: ...)` or `EdgeInsetsDirectional.fromSTEB(...)` instead of `EdgeInsets.only(left: ..., right: ...)`.
  - Use `AlignmentDirectional.centerStart` / `centerEnd` instead of `Alignment.centerLeft` / `Alignment.centerRight`.
  - Use `PositionedDirectional` instead of `Positioned` with left/right values.
  - Never hardcode `TextDirection.rtl` or `TextDirection.ltr` unless specifically formatting an isolated element like an international phone number, email, or URL.

## 8. Number, Currency & Spacing Formatting
- **Locale-Aware Digits & Separators:** Format numbers, prices, and dates cleanly with proper thousands separators.
- **Proper Unit Spacing:** Always maintain a clean single space (or non-breaking space) between numbers and their units/currencies (e.g., `120,000 تومان` or `$120.00`). Ensure that in RTL mode, numbers and adjacent units do not jump or invert awkwardly.

## 9. Clean Visual Aesthetic (No AI-Style Clutter)
- **No Unrequested Gradients:** Never apply loud, neon, or multi-color linear gradients to backgrounds, cards, AppBars, or buttons. Stick strictly to solid, clean colors defined in the app's `ColorScheme` / theme palette.
- **Subtle Elevation & Borders:** Avoid heavy, dark drop shadows (`BoxShadow` with high opacity or large `blurRadius`). Prefer flat surfaces with subtle 1px borders (`Border.all(color: theme.dividerColor.withValues(alpha: 0.5))`) or minimal, diffused elevation (`elevation: 0` or `1`).

## 10. Uniform Border Radius Scale
- **Consistent Corner Radii:** Do not mix pill-shaped (`StadiumBorder`), sharp-cornered, and heavily rounded elements randomly across screens. Enforce a unified border radius scale across the entire app:
  - **Buttons & TextFields:** `10.0` to `12.0` (`BorderRadius.circular(12)`).
  - **Cards & Containers:** `12.0` to `16.0` (`BorderRadius.circular(16)`).
  - **Dialogs & BottomSheets:** `16.0` to `20.0` (top corners for BottomSheets).

## 11. Strict 8pt Grid Spacing System
- **Consistent Multiples:** Enforce strict adherence to the 8pt grid system for layout padding, margins, and component gaps. 
- **Allowed Spacing Values:** Only use spacing multiples of `4.0`, `8.0`, `16.0`, `24.0`, `32.0`, and `48.0` (e.g., `SizedBox(height: 16)` or `EdgeInsets.all(16)`). Never use arbitrary or random spacing values like `12`, `13`, `17`, or `22`.

## 12. Extreme Contrast Typography Hierarchy
- **Editorial Text Pairing:** Break away from monotonous, flat body text by utilizing extreme contrast in font weights:
  - Use ultra-light weights (`FontWeight.w300`) for secondary descriptions, captions, and subtitles.
  - Use bold/heavy weights (`FontWeight.w700` or `FontWeight.w800`) for primary metrics, amounts, and prominent section titles.
- **Dynamic Scale:** Ensure numerical financial data or key statistics stand out visually with distinct scale contrast.

## 13. Keyboard Dismissal & Overflow Safety
- **Tap-to-Dismiss Keyboard:** Every screen or modal containing a `TextField` / `TextFormField` must dismiss the on-screen keyboard when the user taps outside the input area (e.g., via `onTapOutside: (_) => FocusManager.instance.primaryFocus?.unfocus()` or wrapping the screen in a `GestureDetector`).
- **Zero RenderFlex Overflows:** Forms and content screens must gracefully handle small screens and open keyboards without throwing `Bottom overflowed by X pixels` errors. Use `SingleChildScrollView`, `CustomScrollView`, or `Flexible`/`Expanded` appropriately, and always wrap multi-line dynamic text in `Expanded` or `Flexible` with proper `TextOverflow.ellipsis` where needed.

## 14. Button Loading & Debounce States
- **Dimension Preservation:** When an action button enters a `loading` state, it MUST maintain its exact width and height so the layout never jumps or shifts.
- **Double-Tap Prevention:** Disable the button (`onPressed: isLoading ? null : _handleSubmit`) while an async operation is in progress, and replace the label with a centered, appropriately sized `SizedBox(width: 18, height: 18, child: CircularProgressIndicator(strokeWidth: 2))`.

## 15. Zero Emojis in UI Copy
- **Strictly Professional Text:** Never insert emojis (e.g., 🎉, ⚠️, ❌, ✅, 🚀) into UI labels, `SnackBar` messages, dialog descriptions, empty state texts, or error messages unless explicitly provided in the user's prompt.

## 16. Anti-AI UI & Custom Aesthetic Guidelines
- **Avoid Generic AI Color Palettes:** Never default to the cliché "AI purple/indigo on dark background" or generic Material 3 pastel chips unless explicitly required by the brand. Use purposeful, high-contrast, premium color harmonies.
- **Customized Card & Surface Treatment:** Avoid repetitive flat white cards with identical light shadows. Vary container styling using subtle clean borders (`Border.all(color: theme.dividerColor.withValues(alpha: 0.3))`) and flat surfaces (`elevation: 0`).
- **Intentional Negative Space (Whitespace):** Do not cram elements together. Use generous, intentional whitespace (`SizedBox` heights of `24` to `32` for major section breaks).
- **Customized Empty & Loading States:** Never use raw, default `CircularProgressIndicator()` in the middle of a blank screen. Always design minimalist placeholder layouts or sleek contextual loading indicators.

## 17. Purposeful & Smooth Micro-Animations (No Dead UI)
- **Avoid Static, Jerky UI Transitions:** Never leave screens or components feeling dead or instantly popping into existence. Integrate purposeful, subtle micro-animations across the app without overdoing it or causing jank.
- **Where to Apply Animations:**
  - **List Items / Feed Entries:** Use subtle entrance animations (e.g., lightweight `AnimatedSwitcher` or staggered fade-ins) when items load or update.
  - **State Switching (Tabs, Filters, Toggle Cards):** Use `AnimatedContainer`, `AnimatedCrossFade`, or smooth sizing/color transitions instead of abrupt hard switches.
  - **Buttons & Interactive Elements:** Provide immediate, polished tactile feedback on press (e.g., gentle scale down to `0.98` using `AnimatedContainer` or custom transitions).
- **Performance & Duration Control:** Keep animation durations short, snappy, and natural (between `200ms` to `350ms`) using smooth curves (e.g., `Curves.easeOutCubic`). Avoid long, slow, or distracting animations.

## 18. Memory Leak Prevention & Controller Disposal (NEW)
- **Mandatory Controller Disposal:** Whenever a stateful component initializes controllers (`TextEditingController`, `ScrollController`, `TabController`, `AnimationController`) or Subscriptions (`StreamSubscription`, `ChangeNotifier`), they MUST be explicitly disposed of inside the `dispose()` override method to prevent severe memory leaks.
- **No Dangling Listeners:** Always unsubscribe or cancel network streams and listeners when widgets are unmounted.

## 19. ListView & Lazy Loading Performance (NEW)
- **Never Use Column for Long Lists:** NEVER wrap dynamic or long lists inside a `Column` or `ListView(shrinkWrap: true, physics: NeverScrollableScrollPhysics())` unless the total item count is strictly fixed and very small (e.g., 3 to 5 items).
- **Lazy Rendering Enforcement:** Always use `ListView.builder`, `ListView.separated`, or `SliverList` for dynamic or long datasets to ensure proper lazy loading, virtualization, and smooth 60/120fps scrolling performance.

## 20. Graceful Error Boundaries (NEW)
- **UI Fallback Protection:** Never let widget tree exceptions cause the infamous red "Screen of Death". Wrap critical asynchronous or data-driven builder widgets (like `FutureBuilder` or custom dynamic UI trees) with proper error checking and clean fallback widgets (`SizedBox.shrink()` or a polite, non-intrusive retry card) in case of null exceptions or network drops.

## 21. Immediate VS Code Problems & Diagnostics Resolution (Zero-Debt Rule)
- **Check After Every Single Edit:** Immediately after writing or modifying any code block or file—and BEFORE moving on to the next file or finishing the task—you MUST inspect the IDE diagnostics / VS Code **Problems** panel (using `get_diagnostics` / IDE linter tools or running `dart analyze` / `flutter analyze` on the touched files).
- **Mandatory Prevention of Common Flutter/Dart Analyzer Warnings:** You must proactively avoid writing patterns that trigger analyzer warnings:
  1. **`deprecated_member_use` (`withOpacity`):** NEVER use `.withOpacity(...)` on `Color`. Always use `.withValues(alpha: ...)` instead (e.g., `color.withValues(alpha: 0.5)`).
  2. **`deprecated_member_use` (`ColorScheme` properties):** NEVER use deprecated Material 3 `ColorScheme` fields:
     - Use `colorScheme.surfaceContainerHighest` instead of `colorScheme.surfaceVariant`.
     - Use `colorScheme.surface` instead of `colorScheme.background`.
     - Use `colorScheme.onSurface` instead of `colorScheme.onBackground`.
  3. **`deprecated_member_use` (State & Navigation):**
     - Use `WidgetStateProperty` / `WidgetState` instead of `MaterialStateProperty` / `MaterialState`.
     - Use `PopScope` (with `canPop` and `onPopInvokedWithResult`) instead of `WillPopScope` or `onPopInvoked`.
  4. **`use_build_context_synchronously`:** NEVER use `BuildContext` across async gaps (`await`) without an immediate mounted check. Always place `if (!context.mounted) return;` (or `if (!mounted) return;` inside `State`) right before accessing `context` after an `await`.
  5. **`unnecessary_import`:** NEVER import `dart:typed_data` when `package:flutter/foundation.dart`, `package:flutter/material.dart`, or `package:flutter/services.dart` is already imported in the file. Remove any redundant imports immediately.
  6. **`depend_on_referenced_packages`:** NEVER import a package without verifying it is explicitly listed under `dependencies:` in `pubspec.yaml`.
  7. **`unnecessary_underscores` (Dart 3.7+ Wildcard Variables):** NEVER use multiple underscores like `__` or `___` for unused callback parameters. In modern Dart, always write single `_` wildcards (e.g., `(context, _, _) => ...`).
- **Completion Gate:** A task or step is NEVER considered complete until the VS Code Problems list for all created and modified files shows **0 errors, 0 warnings, and 0 info/lint hints**.
