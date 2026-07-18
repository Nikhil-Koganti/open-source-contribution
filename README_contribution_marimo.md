# Contribution 1: Fix Altair Zoom and Pan on Linux/GNOME/Wayland

**Contribution Number:** 1  
**Student:** [Your Name]  
**Course:** CodePath AI301  
**Project:** marimo  
**Issue:** https://github.com/marimo-team/marimo/issues/3812  
**Status:** Phase I — In Progress

---

## Why I Chose This Issue

I chose this issue because I use Fedora 44 Workstation with GNOME/Wayland, which closely matches the environment where the bug was reported. My laptop also dual-boots Windows 11, allowing me to compare the same chart interactions across operating systems and browsers on the same hardware.

This contribution will help me gain practical experience with an established open-source project, TypeScript frontend code, browser input events, cross-platform debugging, regression testing, and communication with maintainers. Although marimo is a Python notebook project used for data and AI workflows, this issue focuses mainly on its frontend chart interaction behavior.

---

## Understanding the Issue

### Problem Description

marimo's Altair chart component is intended to support zooming and panning through a modifier key combined with scrolling or dragging. On Linux systems using GNOME and Wayland, the expected interaction may not work because modifier-key combinations can be handled differently by the browser, desktop environment, or display server.

The original report states that zoom and pan did not work in Firefox, Chromium, or Vivaldi on GNOME/Wayland. Some key combinations instead triggered browser actions, chart selections, window actions, or workspace switching.

A possible workaround is to add Altair's `.interactive()` method to the chart. Before implementing a fix, I need maintainer confirmation on whether the issue is still intended to address marimo's built-in chart interaction without requiring `.interactive()`.

### Expected Behavior

Users should be able to zoom and pan an Altair chart in marimo using a documented and reliable interaction that works on supported Linux desktop environments without conflicting with browser or GNOME shortcuts.

The final expected behavior must be confirmed with the maintainers, including:

- which modifier key should be used on Linux;
- whether behavior should vary by operating system;
- whether `.interactive()` is only a workaround or the intended solution;
- how zoom and pan should coexist with chart selection features.

### Current Behavior

According to the issue report on GNOME/Wayland:

- Shift-based interactions do not zoom or pan;
- Ctrl-based interactions may scroll the browser or create an interval selection;
- the Super/Windows key may switch workspaces or affect the browser window;
- the behavior occurs across multiple browsers;
- adding `.interactive()` may provide native Altair zoom and pan, but it is unclear whether that resolves the intended marimo behavior.

### Affected Components

The investigation will likely involve:

- marimo's Altair chart frontend component;
- TypeScript keyboard, pointer, drag, and wheel event handling;
- modifier-key detection such as `metaKey`, `ctrlKey`, `altKey`, or `shiftKey`;
- Vega or Vega-Lite chart interaction behavior;
- chart selection behavior and related frontend tests;
- platform-specific behavior in GNOME/Wayland and Windows 11.

The exact files will be recorded after tracing the chart interaction implementation in the repository.

---

## Reproduction Process

### Environment Setup

Primary Linux test environment:

- **Laptop:** ASUS ROG Zephyrus G14 (2024)
- **Operating system:** Fedora 44 Workstation
- **Desktop:** GNOME
- **Display server:** Wayland
- **Browsers:** Firefox and Chromium-based browser
- **Python version:** [Record version]
- **Node version:** [Record version]
- **marimo version/commit:** [Record version or commit hash]
- **Altair version:** [Record version]

Comparison environment:

- **Operating system:** Windows 11
- **Browsers:** Firefox and Chromium-based browser
- **marimo version/commit:** Same version used on Fedora

Setup tasks:

1. Fork and clone the marimo repository.
2. Follow the project's frontend development instructions.
3. Install the required Python and JavaScript dependencies.
4. Run the reproduction notebook from the issue.
5. Confirm the desktop session with `echo $XDG_SESSION_TYPE` and `echo $XDG_CURRENT_DESKTOP`.
6. Record any setup problems and their solutions.

### Steps to Reproduce

1. Run marimo from the latest development branch.
2. Open a notebook containing the following chart:

```python
import marimo as mo
import altair as alt
from vega_datasets import data

cars = data.cars()

chart = mo.ui.altair_chart(
    alt.Chart(cars).mark_point().encode(
        x="Horsepower",
        y="Miles_per_Gallon",
        color="Origin",
    )
)

chart
```

3. Test scrolling and click-dragging with Shift, Ctrl, Alt, and Super/Meta.
4. Record which actions are received by the chart and which are intercepted by the browser or desktop.
5. Repeat the test after adding `.interactive()` to the Altair chart.
6. Repeat the same tests on Windows 11 using the same marimo commit and browser versions where possible.

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in fork]
- **Screen recording or screenshots:** [Link]
- **Browser console output:** [Link or summary]
- **Fedora results:** [Not tested yet]
- **Windows results:** [Not tested yet]
- **Finding about `.interactive()`:** [Not tested yet]
- **Maintainer clarification:** [Link to issue comment when received]

---

## Solution Approach

### Analysis

The root cause has not yet been confirmed. Initial hypotheses include:

1. marimo treats the Meta key as the primary zoom/pan modifier on every platform, while Linux browsers report or reserve that key differently;
2. GNOME intercepts some Super or Alt combinations before the browser can deliver the event;
3. marimo's chart-selection handlers take priority over zoom and pan handlers;
4. Vega's native `.interactive()` behavior uses a different event configuration from marimo's built-in interaction;
5. browser-specific wheel or pointer-event handling causes inconsistent behavior.

I will avoid selecting a fix until I reproduce the issue, trace the event flow, and confirm the intended behavior with maintainers.

### Proposed Solution

The solution will be based on the confirmed root cause. A likely approach is to make the zoom/pan modifier platform-aware or to use a modifier combination that is consistently delivered to the browser on Linux. Any change must preserve chart selection behavior and avoid breaking macOS or Windows interactions.

Possible outcomes include:

- selecting a Linux-compatible modifier key;
- mapping modifiers by platform;
- changing event precedence between selection and navigation;
- updating user-facing documentation;
- adding automated regression tests for wheel and drag events;
- documenting `.interactive()` if maintainers determine that no code change is needed.

### Implementation Plan

Using the adapted UMPIRE framework:

**Understand:** Reproduce the reported failure and clarify whether marimo's built-in chart navigation should work without Altair's `.interactive()` method.

**Match:** Locate the implementation introduced for Altair zoom and pan, review related tests, and identify existing platform-detection and keyboard-event conventions in the marimo frontend.

**Plan:**

1. Reproduce the behavior on Fedora 44 GNOME/Wayland.
2. Compare behavior on Windows 11.
3. Log keyboard, wheel, pointer, and drag event properties during each interaction.
4. Trace the relevant TypeScript handlers and Vega event configuration.
5. Confirm expected behavior with maintainers.
6. Implement the smallest cross-platform fix supported by the findings.
7. Add or update tests for Linux-compatible modifier handling.
8. Test chart selection, zoom, and pan for regressions.
9. Run the repository's formatting, linting, type-checking, and test commands.
10. Submit a focused pull request with reproduction evidence and test results.

**Implement:** [Link to branch and commits]

**Review:**

- [ ] The change follows marimo's contribution guidelines.
- [ ] The fix addresses the confirmed issue rather than only one browser symptom.
- [ ] Existing chart-selection behavior remains functional.
- [ ] Tests cover the changed interaction logic.
- [ ] Comments explain platform-specific behavior where necessary.
- [ ] Formatting, linting, type-checking, and tests pass.
- [ ] The pull request clearly describes manual test environments.

**Evaluate:** Verify behavior on Fedora 44 GNOME/Wayland and Windows 11 in Firefox and at least one Chromium-based browser. Compare behavior with and without `.interactive()` and record any remaining platform limitations.

---

## Testing Strategy

### Unit Tests

- [ ] Verify that the intended Linux modifier activates chart navigation.
- [ ] Verify that unrelated modifier combinations do not activate navigation.
- [ ] Verify that modifier detection does not break Windows or macOS behavior.
- [ ] Verify that selection events still work when navigation is inactive.
- [ ] Verify wheel and drag handlers use the intended event properties.

The exact tests will depend on the existing frontend test utilities and the confirmed implementation.

### Integration Tests

- [ ] Render an Altair chart through `mo.ui.altair_chart` and verify zoom interaction.
- [ ] Verify pan interaction on a rendered chart.
- [ ] Verify that interval or point selection remains functional.
- [ ] Verify behavior with and without Altair's `.interactive()` method.

### Manual Testing

| Platform | Browser | Zoom | Pan | Selection | Notes |
|---|---|---:|---:|---:|---|
| Fedora 44 / GNOME / Wayland | Firefox | Pending | Pending | Pending | |
| Fedora 44 / GNOME / Wayland | Chromium | Pending | Pending | Pending | |
| Windows 11 | Firefox | Pending | Pending | Pending | |
| Windows 11 | Chromium | Pending | Pending | Pending | |

Manual testing will include checking that modifier combinations do not unexpectedly trigger browser zoom, workspace switching, window movement, or conflicting chart selections.

---

## Implementation Notes

### Week 1: Issue Validation and Setup

- Contact maintainers to confirm that the issue is still actionable.
- Ask whether `.interactive()` is considered a workaround or the intended behavior.
- Fork and build marimo locally.
- Reproduce the issue on Fedora 44.
- Record browser and environment details.

### Week 2: Codebase Investigation

- Locate the Altair chart event-handling code.
- Review the original zoom-and-pan implementation and related tests.
- Compare event values across Fedora and Windows.
- Document the likely root cause.

### Week 3: Implementation and Testing

- Implement the agreed solution.
- Add or update automated tests.
- Run manual tests across both operating systems and browsers.
- Check for regressions in chart selection.

### Week 4: Pull Request and Review

- Submit the pull request.
- Respond to maintainer feedback.
- Revise tests or implementation as requested.
- Document the final result, including whether the PR was merged.

### Code Changes

- **Files modified:** [List after implementation]
- **Key commits:** [Links]
- **Approach decisions:** [Record decisions and supporting evidence]
- **Commands used for validation:** [List test, lint, and formatting commands]

---

## Pull Request

**PR Link:** [GitHub pull request URL]

**Draft PR Summary:**

This contribution investigates and fixes Altair chart zoom and pan behavior on Linux systems using GNOME/Wayland. The work includes cross-platform reproduction on Fedora 44 and Windows 11, analysis of browser modifier and pointer events, a focused TypeScript change, and regression testing for chart navigation and selection behavior.

**Maintainer Feedback:**

- [Date]: [Summary of feedback]
- [Date]: [How the feedback was addressed]

**Status:** Not submitted

---

## Learnings and Reflections

### Technical Skills Gained

Expected learning areas include:

- contributing to a large open-source repository;
- TypeScript frontend development;
- browser keyboard, wheel, pointer, and drag events;
- Linux GNOME/Wayland behavior;
- Vega, Vega-Lite, Altair, and marimo chart interactions;
- cross-platform debugging;
- frontend regression testing;
- writing technical pull-request descriptions.

### Challenges Overcome

[Document setup issues, reproduction inconsistencies, codebase navigation, test design, and maintainer feedback as the project progresses.]

### What I Would Do Differently Next Time

[Complete after the contribution. Consider issue-scoping, early maintainer communication, reproduction quality, and testing strategy.]

---

## Project Deliverables

- [ ] Maintainer confirmation of the issue's current scope
- [ ] Reproducible example on Fedora 44 GNOME/Wayland
- [ ] Windows 11 comparison results
- [ ] Root-cause analysis
- [ ] Implementation or documentation change
- [ ] Automated tests where practical
- [ ] Manual cross-browser test matrix
- [ ] Pull request submitted upstream
- [ ] Final reflection and presentation for CodePath AI301

A merged pull request is the preferred outcome, but the class project will also document the investigation, evidence, attempted solution, tests, and maintainer feedback if upstream review is not completed during the course.

---

## Resources Used

- marimo issue #3812: https://github.com/marimo-team/marimo/issues/3812
- marimo repository: https://github.com/marimo-team/marimo
- marimo contribution documentation: [Add link]
- Original zoom-and-pan implementation or related pull request: [Add link]
- Altair interaction documentation: [Add link]
- Vega/Vega-Lite event documentation: [Add link]
- Fedora, GNOME, or Wayland documentation used during debugging: [Add links]
