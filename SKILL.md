---
name: flutter-ui-design-standards
description: Strict Flutter UI/UX design rules, localization font handling, Persian (fa) locale specifics, RTL/LTR layout, 8pt spacing system, design tokens and dark mode, extreme contrast typography, accessibility, forms and input UX, responsive layout, mandatory overflow verification, anti-AI aesthetics, smooth micro-animations, widget and image performance, adaptive architecture and state management, component modularity, latest pub.dev packages enforcement, memory leak prevention, error boundaries, logging, testing, and real-time VS Code diagnostics verification. Always apply these rules when creating, refactoring, or styling screens, widgets, dialogs, forms, or bottom sheets.
---

# Flutter UI/UX Design Standards & Constraints

You must strictly enforce the following UI/UX and code-quality constraints whenever generating, refactoring, or styling UI code in this project. Never deviate from these rules unless explicitly instructed by the user.

---

# Part A — Localization, Typography & Direction

## 1. Localization & Locale-Specific Typography
- **Asset Font Enforcement:** Always inspect `assets/font/` (or `assets/fonts/`) and `pubspec.yaml` to identify the exact custom font families configured for the project.
- **Dynamic Locale Switching:** In multi-language apps supporting Persian (`fa`) and English (`en`), never hardcode a single font family across the entire app if distinct fonts exist for each language. Dynamically resolve the `fontFamily` in `ThemeData` (and custom `TextTheme` styles) based on the active `Locale`:
  - Apply the designated Persian font family from `assets/font` when `locale.languageCode == 'fa'`.
  - Apply the designated English font family from `assets/font` when `locale.languageCode == 'en'`.
- **Fallback Prevention:** Ensure every custom `TextStyle` inherits or explicitly sets the active locale's font family so the app never falls back to raw system fonts.

## 2. Page Titles & AppBar Typography
- **Standardized Sizing:** Never use oversized or exaggerated font sizes for screen titles in `AppBar` or page headers.
- **Size Constraints:** Keep page title font sizes strictly within the standard mobile scale (`16.0` to `18.0`, with `FontWeight.w600` or `FontWeight.w500`). Never use `headlineLarge` or `headlineMedium` for toolbar titles.

## 3. Extreme Contrast Typography Hierarchy
- **Editorial Text Pairing:** Break away from monotonous, flat body text by utilizing extreme contrast in font weights:
  - Use ultra-light weights (`FontWeight.w300`) for secondary descriptions, captions, and subtitles.
  - Use bold/heavy weights (`FontWeight.w700` or `FontWeight.w800`) for primary metrics, amounts, and prominent section titles.
- **Dynamic Scale:** Ensure numerical financial data or key statistics stand out visually with distinct scale contrast.

## 4. Strict RTL/LTR Directional Layout Rules
- **Directional Widgets Only:** Always design layouts to seamlessly support both Persian (RTL) and English (LTR) without visual breakage:
  - Use `EdgeInsetsDirectional.only(start: ..., end: ...)` or `EdgeInsetsDirectional.fromSTEB(...)` instead of `EdgeInsets.only(left: ..., right: ...)`.
  - Use `AlignmentDirectional.centerStart` / `centerEnd` instead of `Alignment.centerLeft` / `Alignment.centerRight`.
  - Use `PositionedDirectional` instead of `Positioned` with left/right values.
  - Never hardcode `TextDirection.rtl` or `TextDirection.ltr` unless specifically formatting an isolated element like an international phone number, email, or URL.

## 5. Persian (fa) Locale Specifics
- **Digit Normalization on Input:** Before validating, parsing, comparing, or storing any user input, normalize Persian (`۰-۹`) and Arabic-Indic (`٠-٩`) digits to ASCII (`0-9`). Never send or persist non-ASCII digits to APIs or databases.
- **Locale-Aware Digit Display:** When the active locale is `fa`, display numbers, prices, and counters with Persian digits through a single shared formatting helper. Never mix Persian and ASCII digits on the same screen.
- **Character Unification:** Normalize Arabic `ي` → Persian `ی` and Arabic `ك` → Persian `ک` in search queries, comparisons, and stored text so search results stay consistent.
- **Jalali (Shamsi) Dates:** In the `fa` locale, never show raw Gregorian `DateTime.toString()` output. Display dates in the Jalali calendar (e.g., via `shamsi_date` or `persian_datetime_picker`) with Persian month names, and use a Jalali date picker for date input.
- **Bidi Isolation for Mixed Text:** Wrap Latin fragments embedded in Persian text (brand names, codes, phone numbers, emails, URLs, order IDs) in their own `Directionality(textDirection: TextDirection.ltr)` or bidi isolate characters (`⁦` ... `⁩`) so punctuation and digits never jump to the wrong side.

## 6. Number, Currency & Spacing Formatting
- **Locale-Aware Digits & Separators:** Format numbers, prices, and dates cleanly with proper thousands separators.
- **Proper Unit Spacing:** Always maintain a clean single space (or non-breaking space) between numbers and their units/currencies (e.g., `120,000 تومان` or `$120.00`). Ensure that in RTL mode, numbers and adjacent units do not jump or invert awkwardly.

## 7. Zero Emojis in UI Copy
- **Strictly Professional Text:** Never insert emojis (e.g., 🎉, ⚠️, ❌, ✅, 🚀) into UI labels, `SnackBar` messages, dialog descriptions, empty state texts, or error messages unless explicitly provided in the user's prompt.

---

# Part B — Visual System & Theming

## 8. Design Tokens, Theming & Dark Mode
- **No Hardcoded Colors in Widgets:** NEVER write `Color(0xFF...)`, `Colors.xxx`, or hex values inside widget code. Every color must come from `Theme.of(context).colorScheme` or a project `ThemeExtension`. Raw color values may only appear in the theme definition files.
- **Light & Dark Parity:** Every screen and component must render correctly in both light and dark themes. Define both `theme` and `darkTheme` and verify contrast, borders, and surfaces in each before considering UI work complete.
- **Spacing & Radius Tokens:** Define shared constants for spacing and corner radii (e.g., `AppSpacing.s4 / s8 / s16 / s24 / s32 / s48` and `AppRadius.button / card / sheet`) and use them instead of magic numbers. If the project already has such constants, reuse them.

## 9. Clean Visual Aesthetic (No AI-Style Clutter)
- **No Unrequested Gradients:** Never apply loud, neon, or multi-color linear gradients to backgrounds, cards, AppBars, or buttons. Stick strictly to solid, clean colors defined in the app's `ColorScheme` / theme palette.
- **Subtle Elevation & Borders:** Avoid heavy, dark drop shadows (`BoxShadow` with high opacity or large `blurRadius`). Prefer flat surfaces with subtle 1px borders (`Border.all(color: theme.dividerColor.withValues(alpha: 0.5))`) or minimal, diffused elevation (`elevation: 0` or `1`).

## 10. Anti-AI UI & Custom Aesthetic Guidelines
- **Avoid Generic AI Color Palettes:** Never default to the cliché "AI purple/indigo on dark background" or generic Material 3 pastel chips unless explicitly required by the brand. Use purposeful, high-contrast, premium color harmonies.
- **Customized Card & Surface Treatment:** Avoid repetitive flat white cards with identical light shadows. Vary container styling using subtle clean borders (`Border.all(color: theme.dividerColor.withValues(alpha: 0.3))`) and flat surfaces (`elevation: 0`).
- **Intentional Negative Space (Whitespace):** Do not cram elements together. Use generous, intentional whitespace (`SizedBox` heights of `24` to `32` for major section breaks).
- **Customized Empty & Loading States:** Never use raw, default `CircularProgressIndicator()` in the middle of a blank screen. Always design minimalist placeholder layouts or sleek contextual loading indicators.

## 11. Uniform Border Radius Scale
- **Consistent Corner Radii:** Do not mix pill-shaped (`StadiumBorder`), sharp-cornered, and heavily rounded elements randomly across screens. Enforce a unified border radius scale across the entire app:
  - **Buttons & TextFields:** `10.0` to `12.0` (`BorderRadius.circular(12)`).
  - **Cards & Containers:** `12.0` to `16.0` (`BorderRadius.circular(16)`).
  - **Dialogs & BottomSheets:** `16.0` to `20.0` (top corners for BottomSheets).

## 12. Strict 8pt Grid Spacing System
- **Consistent Multiples:** Enforce strict adherence to the 8pt grid system for layout padding, margins, and component gaps.
- **Allowed Spacing Values:** Only use spacing multiples of `4.0`, `8.0`, `16.0`, `24.0`, `32.0`, and `48.0` (e.g., `SizedBox(height: 16)` or `EdgeInsets.all(16)`). Never use arbitrary or random spacing values like `12`, `13`, `17`, or `22`.
- **Scope:** This rule governs spacing (padding, margins, gaps). Component dimensions such as corner radii, button heights, icon sizes, and tap targets follow their own rules.

## 13. Strict No-Decorative-Icons Policy
- **Text-Only Headers & Labels:** Do NOT place icons next to page titles, section headers, dialog titles, bottom sheet titles, or general text labels.
- **Explicit Permission Required:** Never add decorative leading/trailing icons to text elements to "enhance" the UI unless the user explicitly asks for an icon in that specific location.

## 14. Unified Back Button Iconography
- **Strict Consistency:** Every screen featuring a back button in the toolbar (`AppBar.leading`) must use the exact same icon asset or `IconData` across the entire application.
- **No Mixing:** Never mix `Icons.arrow_back`, `Icons.arrow_back_ios`, `Icons.arrow_back_ios_new`, or `CupertinoIcons.back` across different screens.
- **Implementation:** Use a single shared `CustomBackButton` widget or rely on the global `AppBarTheme` / `ActionIconTheme` to guarantee 100% visual uniformity and proper RTL/LTR directional mirroring.

---

# Part C — Components & Interaction

## 15. BottomSheet Layout, SafeArea & Action Buttons
- **System Navigation Bar Clearance:** Every `ModalBottomSheet` or persistent bottom sheet MUST account for system UI overlays (especially the Android 3-button / gesture navigation bar and iOS Home Indicator).
  - Always wrap bottom sheet content inside a `SafeArea(top: false, bottom: true)` or explicitly add `MediaQuery.of(context).viewPadding.bottom` (and `viewInsets.bottom` when keyboards appear) to the bottom padding.
  - Set `isScrollControlled: true` and `useSafeArea: true` when necessary to prevent clipping or hiding content under the system nav bar.
- **Action Button Alignment & Styling:**
  - When a bottom sheet contains two action buttons (e.g., Confirm and Cancel), place them **side-by-side horizontally in a `Row`** using equal `Expanded` wrappers and a consistent 8pt grid gap (e.g., `SizedBox(width: 8)`). Never stack them vertically with mismatched styles.
  - Both buttons must share the exact same height (`44.0` to `48.0`), border radius, and text size, with cohesive color pairing (e.g., a primary filled button next to a matching secondary tonal/filled button of identical dimensions).

## 16. Confirmation Dialogs (AlertDialogs)
- **Compact & Standard Typography:** Keep typography inside confirmation modals clean, proportional, and never oversized:
  - **Dialog Title:** `15.0` to `16.0` (`FontWeight.w600`). Never use large display sizes.
  - **Dialog Body/Content:** `13.0` to `14.0` (`FontWeight.w400`) with balanced line height (`height: 1.5`).
  - **Dialog Action Buttons:** `13.0` to `14.0` (`FontWeight.w500` or `w600`) with compact, uniform button heights (`40.0` to `44.0`). Avoid bulky padding or oversized button styles inside dialogs.

## 17. Forms & Input UX
- **Visible Labels:** Every input field must have a persistent visible label (`labelText` or a text label above the field). Never rely on `hintText` alone, since it disappears while typing.
- **Inline, Actionable Errors:** Show validation errors directly under the related field in plain text that explains how to fix the problem (e.g., "شماره موبایل باید ۱۱ رقم باشد"), not generic messages like "Invalid input".
- **Correct Keyboard & Autofill:** Always set the matching `keyboardType` (`TextInputType.phone`, `emailAddress`, `number`, etc.), `textInputAction`, and `autofillHints` (e.g., `AutofillHints.telephoneNumber`, `AutofillHints.email`) for each field.
- **Focus Chaining:** Use `textInputAction: TextInputAction.next` to move focus to the next field, and `TextInputAction.done` on the last field to submit or dismiss the keyboard.
- **Digit Normalization:** Apply the Persian digit normalization from Rule 5 in validators and before submission.

## 18. Keyboard Dismissal & Overflow Safety
- **Tap-to-Dismiss Keyboard:** Every screen or modal containing a `TextField` / `TextFormField` must dismiss the on-screen keyboard when the user taps outside the input area (e.g., via `onTapOutside: (_) => FocusManager.instance.primaryFocus?.unfocus()` or wrapping the screen in a `GestureDetector`).
- **Zero RenderFlex Overflows:** Forms and content screens must gracefully handle small screens and open keyboards without throwing `Bottom overflowed by X pixels` errors. Use `SingleChildScrollView`, `CustomScrollView`, or `Flexible`/`Expanded` appropriately, and always wrap multi-line dynamic text in `Expanded` or `Flexible` with proper `TextOverflow.ellipsis` where needed.

### 18.1 Mandatory Overflow Verification (Every UI Change)
Overflow safety is not assumed — it MUST be verified every time a screen, widget, dialog, form, or bottom sheet is created or modified, before the work is considered complete.

- **Step 1 — Static Overflow Review:** Before finishing, re-read every touched `build` method and check each of these high-risk patterns:
  - A `Text`, `TextField`, or dynamic child inside a `Row` without `Expanded` / `Flexible` (horizontal overflow in long Persian or English strings).
  - A `Column` inside a fixed-height parent (`SizedBox`, `Container(height: ...)`, bottom sheet, card) whose content can grow, or that is not scrollable when the keyboard opens.
  - Fixed `width` / `height` values on containers that hold text (these clip under text scaling).
  - `Text` without `maxLines` + `overflow: TextOverflow.ellipsis` where the content is user- or server-provided (names, titles, addresses, prices).
  - `Wrap` vs `Row`: chips, tags, and button groups whose count or label length is dynamic must use `Wrap`, not `Row`.
  - Nested scrollables (`ListView` inside `Column`) without `Expanded` or a bounded height (unbounded constraint errors).
  - `Stack` children positioned with fixed offsets that may run outside the parent on small screens.
- **Step 2 — Stress Conditions:** Every touched UI must remain overflow-free under ALL of these conditions:
  - Smallest supported phone size: `320 × 568` logical pixels.
  - System text scaling at `1.3×` and `2.0×` (`MediaQuery.textScaler`).
  - On-screen keyboard open on screens with inputs (`viewInsets.bottom` ≈ `300`).
  - Both `fa` (RTL) and `en` (LTR) locales, with long realistic strings (e.g., a 60+ character Persian product name, a long address, a 9-digit price with currency).
  - Landscape orientation.
- **Step 3 — Runtime Check:** When the app is running (hot reload, simulator, or device), inspect the debug console / `flutter logs` after navigating to every touched screen and search for `overflowed by`, `RenderFlex`, `unbounded`, and `was not laid out`. Any match is a blocking defect that must be fixed before continuing. Never dismiss the yellow-black overflow stripes as cosmetic.
- **Step 4 — Automated Overflow Test (shared components and new screens):** When the project has a `test/` folder, add or extend a widget test that pumps the widget under the stress conditions and fails on any layout exception:

```dart
Future<void> pumpOverflowCheck(
  WidgetTester tester,
  Widget child, {
  Size size = const Size(320, 568),
  double textScale = 2.0,
  Locale locale = const Locale('fa'),
}) async {
  tester.view.physicalSize = size;
  tester.view.devicePixelRatio = 1.0;
  addTearDown(tester.view.reset);
  await tester.pumpWidget(
    MaterialApp(
      locale: locale,
      localizationsDelegates: GlobalMaterialLocalizations.delegates,
      supportedLocales: const [Locale('fa'), Locale('en')],
      home: MediaQuery(
        data: MediaQueryData(
          size: size,
          textScaler: TextScaler.linear(textScale),
        ),
        child: Scaffold(body: child),
      ),
    ),
  );
  await tester.pumpAndSettle();
  expect(tester.takeException(), isNull);
}
```

  Run it for both `Locale('fa')` and `Locale('en')`. Adapt the wrapper (theme, providers, localization delegates) to match the project's existing test setup.
- **Reporting:** When finishing UI work, state briefly which overflow checks were performed (static review, stress conditions, runtime logs, tests) and the result. If a check could not be run (e.g., no running device), say so explicitly instead of implying it passed.

## 19. Button Loading & Debounce States
- **Dimension Preservation:** When an action button enters a `loading` state, it MUST maintain its exact width and height so the layout never jumps or shifts.
- **Double-Tap Prevention:** Disable the button (`onPressed: isLoading ? null : _handleSubmit`) while an async operation is in progress, and replace the label with a centered, appropriately sized `SizedBox(width: 18, height: 18, child: CircularProgressIndicator(strokeWidth: 2))`.

## 20. Purposeful & Smooth Micro-Animations (No Dead UI)
- **Avoid Static, Jerky UI Transitions:** Never leave screens or components feeling dead or instantly popping into existence. Integrate purposeful, subtle micro-animations across the app without overdoing it or causing jank.
- **Where to Apply Animations:**
  - **List Items / Feed Entries:** Use subtle entrance animations (e.g., lightweight `AnimatedSwitcher` or staggered fade-ins) when items load or update.
  - **State Switching (Tabs, Filters, Toggle Cards):** Use `AnimatedContainer`, `AnimatedCrossFade`, or smooth sizing/color transitions instead of abrupt hard switches.
  - **Buttons & Interactive Elements:** Provide immediate, polished tactile feedback on press (e.g., gentle scale down to `0.98` using `AnimatedContainer` or custom transitions).
- **Performance & Duration Control:** Keep animation durations short, snappy, and natural (between `200ms` to `350ms`) using smooth curves (e.g., `Curves.easeOutCubic`). Avoid long, slow, or distracting animations.
- **Respect Reduced Motion:** When `MediaQuery.of(context).disableAnimations` is `true`, replace motion with instant or simple fade transitions (`Duration.zero` or a short opacity change).

## 21. Haptic & Refresh Feedback
- **Subtle Haptics:** Use `HapticFeedback.selectionClick()` for selection changes (toggles, segmented controls, chips) and `HapticFeedback.lightImpact()` for confirmed primary actions. Never trigger haptics on scroll or on every rebuild.
- **Pull-to-Refresh:** Every list or feed whose data can be reloaded from the network must support `RefreshIndicator` (or `CupertinoSliverRefreshControl` on iOS-styled screens).

## 22. Accessibility (a11y)
- **Semantic Labels:** Every icon-only interactive element (`IconButton`, `InkWell` with an icon, `GestureDetector` on an image) MUST have a localized `tooltip` or `Semantics(label: ...)`. Decorative images must be excluded with `excludeFromSemantics: true` or `ExcludeSemantics`.
- **Minimum Tap Targets:** Every tappable element must have a hit area of at least `48 × 48` logical pixels, even when the visible icon is smaller (use padding or `constraints`, never shrink `materialTapTargetSize` below the default).
- **Color Contrast:** Body text must meet a contrast ratio of at least `4.5:1` against its background, and large text (≥ `18.0` regular or ≥ `14.0` bold) at least `3:1`, in both light and dark themes.
- **Never Rely on Color Alone:** States such as errors, success, selection, or required fields must also be conveyed with text or shape, not only with a color change.
- **Text Scaling:** Layouts must remain usable with system font scaling up to `2.0×`. Never clamp `MediaQuery.textScaler` to `1.0` globally, and never place text in fixed-height containers that clip when scaled.
- **Logical Reading Order:** Screen reader traversal order must match the visual order in both RTL and LTR. Group related content (e.g., a price and its label) with `MergeSemantics` when it should be read as one unit.

## 23. Responsive & Adaptive Layout
- **Breakpoint-Driven Layouts:** Use `LayoutBuilder` or `MediaQuery.sizeOf(context)` with explicit breakpoints (e.g., compact `< 600`, medium `600–840`, expanded `> 840`) instead of assuming a phone-width screen.
- **Constrain Content Width:** On tablets and wide screens, constrain reading content and forms (e.g., `ConstrainedBox(constraints: BoxConstraints(maxWidth: 600))`) rather than stretching them edge to edge.
- **Orientation Safety:** Screens must not overflow or break in landscape orientation. Prefer scrollable layouts over fixed-height stacks.

---

# Part D — Performance & Resources

## 24. Widget Build Performance
- **Const Everywhere Possible:** Mark constructors and widget instances `const` wherever possible to avoid unnecessary rebuilds.
- **Prefer Lightweight Widgets:** Use `Padding`, `SizedBox`, `ColoredBox`, `DecoratedBox`, and `Align` instead of `Container` when only a single property is needed.
- **Pure `build` Methods:** Never perform heavy computation, sorting, filtering, parsing, or object allocation of controllers inside `build()`. Compute once in state, view models, or memoized getters.
- **Scoped Rebuilds:** Rebuild only what changes. With `provider`, use `context.select`, `Selector`, or `Consumer` placed as deep as possible instead of `context.watch` at the top of a large screen. Apply the equivalent technique for other state management solutions.
- **Background Work:** Run heavy JSON parsing or data processing off the UI thread with `compute()` or `Isolate.run()`.

## 25. ListView & Lazy Loading Performance
- **Never Use Column for Long Lists:** NEVER wrap dynamic or long lists inside a `Column` or `ListView(shrinkWrap: true, physics: NeverScrollableScrollPhysics())` unless the total item count is strictly fixed and very small (e.g., 3 to 5 items).
- **Lazy Rendering Enforcement:** Always use `ListView.builder`, `ListView.separated`, or `SliverList` for dynamic or long datasets to ensure proper lazy loading, virtualization, and smooth 60/120fps scrolling performance.

## 26. Image & Asset Handling
- **Cached Network Images:** Load remote images with `cached_network_image` (or the project's existing equivalent), always providing a styled `placeholder` and an `errorWidget`. Never use bare `Image.network` without loading and error handling.
- **Decode at Display Size:** Set `cacheWidth` / `cacheHeight` (or `memCacheWidth` / `memCacheHeight`) to roughly the rendered size multiplied by the device pixel ratio, so full-resolution images are never decoded for small thumbnails.
- **Vector Icons:** Use `flutter_svg` for SVG assets, and tint them through theme colors (`ColorFilter.mode(colorScheme.onSurface, BlendMode.srcIn)`), never with hardcoded colors.
- **Precache Critical Images:** Use `precacheImage` for hero or above-the-fold images that must appear instantly.

## 27. Memory Leak Prevention & Controller Disposal
- **Mandatory Controller Disposal:** Whenever a stateful component initializes controllers (`TextEditingController`, `ScrollController`, `TabController`, `AnimationController`) or Subscriptions (`StreamSubscription`, `ChangeNotifier`), they MUST be explicitly disposed of inside the `dispose()` override method to prevent severe memory leaks.
- **No Dangling Listeners:** Always unsubscribe or cancel network streams and listeners when widgets are unmounted.

## 28. Graceful Error Boundaries
- **UI Fallback Protection:** Never let widget tree exceptions cause the infamous red "Screen of Death". Wrap critical asynchronous or data-driven builder widgets (like `FutureBuilder` or custom dynamic UI trees) with proper error checking and clean fallback widgets (`SizedBox.shrink()` or a polite, non-intrusive retry card) in case of null exceptions or network drops.

---

# Part E — Architecture & Code Structure

## 29. Core Architecture Adaptability & Clean Code Standards
- **Strict Adherence to Existing Architecture:** Always inspect and adapt to the existing project architecture (whether it's layered like `lib/providers/`, `lib/services/`, `lib/widgets/` or modular Feature-First). Never force a structural paradigm shift unless explicitly requested by the user.
- **Modern Dart 3.10+ & Material 3:** Always utilize Flutter 3.x modern features, Dart 3+ syntax, and Material 3 design specifications.

## 30. Adaptive State Management & Architecture Rules
- **Respect Existing State Management:** Inspect `pubspec.yaml` and existing codebase patterns before writing code.
  - If the project uses `provider` (e.g., `ChangeNotifier`, `Provider`), continue using it according to the existing project convention.
  - Only use alternative state management architectures if the project is newly initialized for them or if the user explicitly instructs you to migrate.
- **No Unsolicited Refactoring:** Never refactor working architectural patterns or state management setups of an existing project without direct user permission.

## 31. Explicit UI State Modeling
- **Exhaustive Screen States:** For new screens or features, model asynchronous UI state explicitly (e.g., a `sealed class` with `Initial`, `Loading`, `Error`, and `Ready` subclasses, or an equivalent enum plus data) and render it with an exhaustive `switch` expression so no state is left unhandled. This works with any state management solution, including `ChangeNotifier`.
- **One-Off Events Are Not State:** Navigation, `SnackBar`s, and dialogs triggered by actions must be handled as one-time events (callbacks, streams, or a consumed flag), never as persistent state that re-fires on every rebuild.
- **Modern Dart Idioms:** Prefer pattern matching, switch expressions, records, and collection `if` / `for` over verbose imperative code.

## 32. Puzzle-Like Modularity & Strict File Length Limits
- **Decomposition Principle (Like Puzzle Pieces):** Never build monolithic, oversized widgets where hundreds of lines of UI code are crammed into a single `build` method. Break down every screen into small, independent, reusable components (like assembling puzzle pieces). Each card, header, input field, and action bar must live in its own dedicated widget class.
- **Strict File Length Limit:** NEVER write massive files containing thousands of lines of code (keep individual files within a clean limit of 200–600 lines max). If a screen or feature grows, immediately extract sub-widgets into separate files inside the appropriate folder (`widgets/`, `components/`, etc.). Clean separation guarantees maximum readability and maintainability.
- **Existing Large Files:** Apply this limit to code you create or modify. Do not split existing large files that you are not otherwise changing without the user's permission (see Rule 30).

## 33. Latest Pub.dev Package Versions Enforcement
- **Always Use Up-to-Date Packages:** When adding dependencies or utilizing third-party packages, always target the **latest stable version** available on `pubspec.yaml` / `pub.dev`, avoiding outdated legacy versions unless explicitly restricted by the project.
- **Conflict Exception Handling:** If a package version creates a strict dependency conflict (`dependency constraint violation`) with existing packages in the project, gracefully fallback to a compatible matching version while notifying the user.
- **Prefer Existing Equivalents:** Before adding a package named in these rules (e.g., `cached_network_image`, `flutter_svg`, `shamsi_date`), check whether the project already uses an equivalent and reuse it instead.

---

# Part F — Quality Gates

## 34. Logging & Lint Suppression Hygiene
- **No `print`:** Never use `print()` in `lib/`. Use `debugPrint()` or `dart:developer` `log()`, and pass errors and stack traces as separate arguments (`log('Fetch failed', error: e, stackTrace: st)`) instead of interpolating them into strings.
- **No Silent Catches:** Never write empty `catch` blocks. Log the error and show the appropriate fallback UI (Rule 28).
- **Justified Ignores Only:** Every `// ignore:` or `// ignore_for_file:` comment must include a short reason on the same or previous line. Never use ignores to hide warnings that can be fixed.

## 35. Testing for Shared Components
- **Widget Tests for Reusable Components:** When creating a new shared or reusable widget (buttons, cards, form fields, sheets), add a widget test covering its key states (default, loading, disabled, error).
- **Both Directions & Themes:** Where golden tests exist in the project, cover the component in both RTL (`fa`) and LTR (`en`), and in both light and dark themes.

## 36. Immediate VS Code Problems & Diagnostics Resolution (Zero-Debt Rule)
- **Check After Every Single Edit:** Immediately after writing or modifying any code block or file—and BEFORE moving on to the next file or finishing the task—you MUST inspect the IDE diagnostics / VS Code **Problems** panel (using `get_diagnostics` / IDE linter tools or running `dart analyze` / `flutter analyze` on the touched files).
- **Formatting:** Run `dart format` on every created or modified file before considering it complete.
- **Mandatory Prevention of Common Flutter/Dart Analyzer Warnings:** You must proactively avoid writing patterns that trigger analyzer warnings:
  1. **`deprecated_member_use` (`withOpacity`):** NEVER use `.withOpacity(...)` on `Color`. Always use `.withValues(alpha: ...)` instead (e.g., `color.withValues(alpha: 0.5)`).
  2. **`deprecated_member_use` (`ColorScheme` properties):** NEVER use deprecated Material 3 `ColorScheme` fields:
     - Use `colorScheme.surfaceContainerHighest` instead of `colorScheme.surfaceVariant`.
     - Use `colorScheme.surface` instead of `colorScheme.background`.
     - Use `colorScheme.onSurface` instead of `colorScheme.onBackground`.
  3. **`deprecated_member_use` (State & Navigation):**
     - Use `WidgetStateProperty` / `WidgetState` instead of `MaterialStateProperty` / `MaterialState`.
     - Use `PopScope` (canPop and onPopInvokedWithResult) instead of WillPopScope.
  4. **`use_build_context_synchronously`:** NEVER use `BuildContext` across async gaps (`await`) without an immediate mounted check. Always place `if (!context.mounted) return;` right before accessing `context` after an `await`.
  5. **`unnecessary_import`:** NEVER import `dart:typed_data` when `package:flutter/foundation.dart` or `material.dart` is already imported.
  6. **`depend_on_referenced_packages`:** NEVER import a package without verifying it is explicitly listed under `dependencies:` in `pubspec.yaml`.
  7. **`unnecessary_underscores` (Dart 3.7+ Wildcard Variables):** NEVER use multiple underscores like `__` or `___` for unused callback parameters. Always write single `_` wildcards.
  8. **`avoid_print`:** Covered by Rule 34; never leave `print()` calls in `lib/`.
- **Completion Gate:** A task or step is NEVER considered complete until the VS Code Problems list for all created and modified files shows **0 errors, 0 warnings, and 0 info/lint hints**, AND the Mandatory Overflow Verification (Rule 18.1) has passed for every touched UI.
