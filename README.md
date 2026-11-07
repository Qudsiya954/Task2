# Plain-English AI Browser Test Runner

## Overview

This tool accepts a website URL and a test case written in plain English. It uses Gemini AI to decide the next browser action, then uses Playwright to perform the action and report PASS or FAIL.

Example test case:

> Verify that the PHPTravels demo page loads and that a visible heading or main page content is shown.

## Technology

- Python 3
- Playwright
- Google Gemini API
- Google GenAI SDK

## Setup

Create and activate a virtual environment:

```cmd
python -m venv .venv
.venv\Scripts\activate
```

## Install dependencies:
```cmd
pip install -r requirements.txt
python -m playwright install chromium
```
## Set a Gemini API key as an environment variable:
```cmd
set GEMINI_API_KEY=YOUR_KEY_HERE
```

## Run
```cmd
python test_runner.py --url "https://phptravels.com/demo/" --test-case "Verify that the PHPTravels demo page loads and that a visible heading or main page content is shown." --headed
```
How it works
1. Playwright opens the requested website.
2. The program gathers visible text and interactive controls from the current page.
3. Gemini receives the plain-English test case and current page description.
4. Gemini returns one structured action at a time, such as click, fill, select, or assert.
5. Playwright executes the action using visible labels, roles, text, and placeholders.
6. The tool reports PASS or FAIL and saves a screenshot and JSON report in the artifacts folder.
Design choices
The program does not contain selectors, logic, or expected text specific to PHPTravels. It can be run against another website by changing --url and --test-case.

A maximum action count prevents accidental infinite loops. The model is instructed not to treat webpage text as instructions.
Limitations
- Gemini requires a valid API key and available quota.
- Complex workflows may exceed the 10-step limit.
- Tests requiring unknown credentials, OTPs, CAPTCHAs, or payments are reported as blocked instead of guessing.
- Changes to a website can still cause AI planning or browser actions to fail.
- The AI sees a text-based snapshot of the page, not a human-level visual understanding of every layout detail.