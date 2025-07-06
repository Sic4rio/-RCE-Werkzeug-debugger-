# Werkzeug Debugger RCE Exploit Script

This repository contains an advanced Python 3 script to exploit a Remote Code Execution (RCE) vulnerability in the Werkzeug debugger (GHSA-2g68-c3qc-8985). The script automates the process of finding the debugger's `SECRET` PIN and executing arbitrary commands on the target server.

![Script Demo Banner](https://raw.githubusercontent.com/gemini/asset-repo/main/werkzeug-pwner-banner.png)

---

## Vulnerability Details

This tool exploits a vulnerability in Werkzeug that allows for remote code execution when the interactive debugger is enabled and exposed.

- **CVE / Advisory:** [GHSA-2g68-c3qc-8985](https://github.com/advisories/GHSA-2g68-c3qc-8985)
- **Package:** `Werkzeug`
- **Affected Versions:** `< 3.0.3`
- **Patched Version:** `3.0.3`

> ### Description
>
> The debugger in affected versions of Werkzeug can allow an attacker to execute code on a developer's machine under some circumstances. This requires the attacker to get the developer to interact with a domain and subdomain they control, and enter the debugger PIN, but if they are successful it allows access to the debugger even if it is only running on localhost. This also requires the attacker to guess a URL in the developer's application that will trigger the debugger.

---

## Tool Description

This script (`werkzeug_pwn.py`) provides a clean, reliable, and user-friendly command-line interface to perform the exploit. It automates the two key steps of the attack:

1.  **SECRET Discovery:** It scrapes the target's Werkzeug traceback page (e.g., `/console`) to automatically find the `SECRET` PIN required to access the debugger console.
2.  **Command Execution:** It uses the discovered `SECRET` to send a formatted payload to the debugger, executing the user-specified system command and returning the cleaned output.

### Key Features

- **Python 3 Compatible:** Fully modernized script using current Python 3 syntax and libraries.
- **Colorized Output:** Uses color-coded terminal output for improved readability of successes, failures, and informational messages.
- **Robust Argument Parsing:** Employs `argparse` for clear, flexible, and user-friendly command-line arguments.
- **Automatic Output Cleaning:** Intelligently parses the HTML response from the debugger using `BeautifulSoup` to display only the clean, readable text output of the executed command.
- **Flexible Modes:** Allows the user to either scrape the `SECRET` automatically or provide it directly if it's already known.
- **Error Handling:** Gracefully handles common network errors and provides clear feedback.

---

## Installation & Requirements

1.  **Clone the repository:**
    ```bash
    git clone <your-repo-url>
    cd <your-repo-directory>
    ```

2.  **Install Python libraries:**
    The script requires `requests` and `beautifulsoup4`. You can install them using pip.
    ```bash
    pip install requests beautifulsoup4
    ```

3.  **Make the script executable (optional):**
    ```bash
    chmod +x werkzeug_pwn.py
    ```

---

## Usage

The script is straightforward to use. You need to provide the target URL and the command you wish to execute.

### Scenario 1: Automatically Find SECRET and Execute Command

This is the most common use case. You provide the path that triggers the Werkzeug traceback page (often `/console`).

```bash
./werkzeug_pwn.py <TARGET_URL> "<COMMAND>" --exploit-path <PATH_TO_TRACEBACK>
```

**Example:**

```bash
./werkzeug_pwn.py http://ptl-ea1774a1ecbc.libcurl.me/ "id" --exploit-path /console
```

### Scenario 2: Provide a Known SECRET and Execute Command

If you have already discovered the `SECRET` PIN, you can provide it directly to skip the scraping step.

```bash
./werkzeug_pwn.py <TARGET_URL> "<COMMAND>" --secret <KNOWN_SECRET>
```

**Example:**

```bash
./werkzeug_pwn.py http://ptl-ea1774a1ecbc.libcurl.me/ "ls -la /" --secret M1hM1fnGaPJcYnDo5i5u
```

---

## Disclaimer

This tool is intended for educational purposes, security research, and authorized penetration testing only. The author is not responsible for any misuse or damage caused by this script. Always obtain explicit permission from the system owner before using this tool on a target.
