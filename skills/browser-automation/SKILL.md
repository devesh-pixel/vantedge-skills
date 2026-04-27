---
name: browser-automation
version: 1.0.0
description: |
  Selenium browser automation using a persistent Chrome session pattern.
  Launch Chrome once with a debug port, then connect from multiple scripts
  without reopening the browser or re-authenticating.
  Use when the user needs to automate a website, scrape a JS-rendered page,
  fill forms, log into a CMS, or do any browser-based automation.
  Triggers: "selenium", "browser automation", "automate website", "scrape page",
  "fill form", "CMS automation", "persistent browser", "chrome session".
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Glob
  - Grep
---

# Persistent Browser Automation with Selenium

## Core Pattern: One Browser, Many Scripts

The key insight is to **never open and close Chrome repeatedly**. Sites detect
repeated logins and trigger CAPTCHAs, rate limits, and verification prompts.
Instead, launch Chrome once with remote debugging enabled, then connect to it
from any script.

### Step 1: Launch the persistent browser

Create a launcher script that opens Chrome with `--remote-debugging-port` and
a persistent user data directory. This keeps cookies and sessions alive.

```python
# launch_browser.py
from selenium import webdriver
from selenium.webdriver.chrome.options import Options

DEBUG_PORT = 9222

options = Options()
options.add_argument("--start-maximized")
options.add_argument("--disable-blink-features=AutomationControlled")
options.add_argument(f"--remote-debugging-port={DEBUG_PORT}")
options.add_argument("--user-data-dir=/tmp/chrome_persistent_session")

driver = webdriver.Chrome(options=options)
# ... do login, auth, etc ...

# Keep browser alive
print(f"Browser running on port {DEBUG_PORT}. Do NOT close this terminal.")
try:
    while True:
        import time; time.sleep(1)
except KeyboardInterrupt:
    driver.quit()
```

Run this in the background. The browser stays open.

### Step 2: Connect from any script

```python
# connect.py
from selenium import webdriver
from selenium.webdriver.chrome.options import Options

DEBUG_PORT = 9222

def get_driver():
    options = Options()
    options.add_experimental_option("debuggerAddress", f"127.0.0.1:{DEBUG_PORT}")
    return webdriver.Chrome(options=options)
```

Now any script can `from connect import get_driver` and interact with the
already-authenticated browser session. No re-login, no new window.

### Step 3: Work inside iframes (Sanity, Retool, etc.)

Many modern apps render inside iframes. You must switch context:

```python
driver.switch_to.default_content()
for f in driver.find_elements(By.TAG_NAME, "iframe"):
    if "target-domain" in (f.get_attribute("src") or ""):
        driver.switch_to.frame(f)
        break
```

Always switch back to `default_content()` before switching into a new iframe.

## Common Pitfalls & Solutions

### Click intercepted by overlays
Sanity and similar SPAs have floating toolbars that intercept clicks.

```python
# Bad: element.click()  — fails with ElementClickInterceptedException

# Good: scroll into view first, then use JS focus
driver.execute_script(
    "arguments[0].focus(); arguments[0].scrollIntoView({block: 'center'});",
    element
)
time.sleep(0.3)
element.send_keys("text")

# Or use JS click for buttons
driver.execute_script("arguments[0].click();", button)
```

### React/SPA forms don't register values set via JS
React controls its own state. Setting `.value` directly won't trigger React's
change handlers. Use `send_keys()` instead, or use the native input setter
pattern:

```python
# For select elements, use Selenium's Select helper
from selenium.webdriver.support.ui import Select
Select(select_el).select_by_visible_text("Option Name")

# For text inputs, send_keys works best with React
input_el.send_keys("value")

# For tags/chips, type + ENTER
tags_input.send_keys("tag name")
tags_input.send_keys(Keys.ENTER)
```

### File uploads
Find the hidden `<input type="file">` and send the absolute file path:

```python
file_input = driver.find_element(By.CSS_SELECTOR, 'input[type="file"]')
file_input.send_keys("/absolute/path/to/file.png")
```

### Google OAuth login in automation
Google detects Selenium and triggers extra verification (phone, device prompt).
Solutions:
1. Use `--user-data-dir` to persist the session after first manual login
2. Handle the verification step (wait for user to approve on phone)
3. Once logged in, cookies persist — no re-auth needed until they expire

```python
# Poll until redirect completes after user approves on phone
for i in range(120):
    if "target-site.com" in driver.current_url:
        break
    time.sleep(2)
```

### Finding elements in SPAs
SPAs like Sanity use dynamic classes. Prefer `data-testid` attributes:

```python
# Good
driver.find_element(By.CSS_SELECTOR, '[data-testid="field-name"] input')

# Bad — class names change between builds
driver.find_element(By.CSS_SELECTOR, '.StyledInput-sc-1d6h1o8')
```

When `data-testid` isn't available, use JS to search by text content:

```python
driver.execute_script("""
    var items = document.querySelectorAll('button');
    for (var i = 0; i < items.length; i++) {
        if (items[i].textContent.trim() === 'Generate') {
            items[i].click();
            break;
        }
    }
""")
```

## Recommended Project Structure

```
project/
├── launch_browser.py    # Start persistent Chrome (run once)
├── connect.py           # get_driver() helper (import everywhere)
├── scrape.py            # Scraping logic
├── automate.py          # Form filling / CMS automation
└── data/                # Input/output data
```

## Dependencies

```
pip install selenium beautifulsoup4 requests
```

ChromeDriver must match your Chrome version. Selenium 4.6+ auto-manages this.
