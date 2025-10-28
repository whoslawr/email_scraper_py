# Simple Email Scraper (Python)

A minimal web crawler that starts from a user-provided URL, follows links, and extracts email addresses from page HTML. It uses a queue (BFS) to visit pages, resolves relative links to absolute URLs, de-duplicates results, and prints all unique emails to stdout.

> ⚠️ For demo/educational use. Not a real-world harvester. Always respect websites’ terms, robots.txt, and local laws.

## How it works
- Prompts for a **Target URL**.
- Maintains a **queue** of URLs to visit (`collections.deque`) and two **sets** for visited URLs and found emails.
- For each page (up to **100 pages** max), fetches HTML with `requests`, parses links with **BeautifulSoup (lxml)**, and resolves:
  - `/path` → `https://host/path`
  - `relative.html` → `https://host/current/relative.html`
- Extracts emails via a simple regex:
  [a-z0-9.-+]+@[a-z0-9.-+]+.[a-z]+  (case-insensitive)
- Skips invalid/missing-schema or failed connections gracefully.
- Prints unique emails at the end.

## Notes & limitations
- Scope: Not restricted to the start domain; will enqueue any absolute http(s) links it encounters.
- Politeness: No robots.txt checks or rate limiting.
- Parsing: Regex catches standard emails only (won’t decode obfuscations like name [at] domain [dot] com).
- JavaScript: Doesn’t execute JS; only parses static HTML.
- Config: Max pages is fixed at 100 in code (adjust if count == 100).

## Possible improvements
- Add domain whitelist and max depth settings.
- Respect robots.txt and add delays.
- Export results to CSV/JSON.
- Detect obfuscated emails.
- Add timeouts, custom User-Agent, and better error handling.
