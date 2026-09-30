# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single static web page: the Nsight Health IT "New Hire Technology Overview." It walks new hires through setting up their MacBook on day one. There is no build step, package manager, linter, or test suite.

- The whole site is one file: `index.html`.
- To preview it, open the file in a browser, or serve it with `python3 -m http.server` and go to `http://localhost:8000/`.

## Structure of the HTML file

The page is fully self-contained: CSS, JS, and images are all inline.

- **`<style>` block (roughly the first 960 lines):** all CSS. Brand colors are CSS variables on `:root` (`--navy`, `--teal`, `--teal-light`, …). Reuse those instead of hard-coding hex values in new CSS. Some existing markup still uses inline `style=` attributes.
- **Disclaimer gate (`#disclaimer-gate`):** a full-screen modal shown on load. `<body class="gate-open">` locks scrolling while it's up. "Continue" stays disabled until `#gate-checkbox` is checked, and "Skip" closes it right away. The small IIFE in the only `<script>` block adds `.fade-out` and removes the gate from the DOM. That script is the only JS on the page.
- **Hero → "What you will need" checklist (`.before-card`) → Technology Overview steps.** Each step is a `.step-card` with `.step-header` (`.step-num`, `.step-title`, `.step-desc`, `.step-time`) and `.step-body` (`ul.step-instructions` plus either a YouTube `<iframe>` or an `a.video-placeholder` for videos that don't exist yet). There are currently 8 steps: OOBE, iru (MDM), Google Authenticator/2FA, 1Password, Chrome, Slack, equipment confirmation, and ready for Day 1. The hero says "8 short sections", so update that text if you add or remove a step.
- The logo is a base64 PNG inlined in an `<img>`. It's one enormous line, so avoid printing it (use `cut -c` or grep excluding `data:image` when inspecting the file).
- The only external dependencies are Google Fonts (Inter), a YouTube embed, and a monday.com form linked from the "Questions about your technology?" section.

## Content conventions

- The overview is written as **voluntary / informational**. Recent commits intentionally reworded content to that framing and added the gate. Keep new copy consistent: don't phrase steps as mandatory pre-start work.
- The gate footer makes a privacy promise: no analytics, cookies, or session tracking. Don't add tracking scripts, cookies, or storage (`localStorage` etc.) without the owner explicitly asking.
- "iru" is Nsight's device management (MDM) tool. The name is lowercase and intentional.
