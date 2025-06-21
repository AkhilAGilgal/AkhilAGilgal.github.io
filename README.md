# CSRF Vulnerability: Discovery and Proof of Concept

### Project Summary
This project provides a complete demonstration of a Cross-Site Request Forgery (CSRF) vulnerability, from discovery to exploitation. It includes:
1.  **A Python Crawler** to detect missing CSRF protections.
2.  **A Live Proof of Concept** hosted on GitHub Pages to demonstrate a real attack.

---

## Part 1: The Concept - What is CSRF?

CSRF (Cross-Site Request Forgery) is an attack that tricks a logged-in user into performing an unwanted action on a web application.

Imagine you are logged into your social media account. An attacker sends you a link to a "funny cat video." When you click it, the attacker's page secretly forces your browser to send a request to the social media site to change your email address. Because you were already logged in, the site trusts the request and performs the action without your knowledge.

This project demonstrates exactly how this kind of attack works.

---

## Part 2: The Crawler - Finding the Vulnerability

The first step in a security assessment is to find potential weaknesses. The included Python script is a simple crawler designed to do just that.

**File:** `csrf_crawler.py`

#### What it does:
*   It fetches a webpage.
*   It scans the page for all HTML `<form>` elements.
*   For each form, it checks for the presence of a hidden input field that looks like an anti-CSRF token (e.g., `csrf_token`, `_csrf`).
*   It reports any forms that are missing these tokens, flagging them as potentially vulnerable to a CSRF attack.

This script represents the **discovery phase** of the project.

---

## Part 3: The Proof of Concept - Exploiting the Vulnerability

This is a live demonstration using two web pages to show the vulnerability in action.

### The Files:
*   **`vulnerable.html`**: This page simulates a user's account settings page. **It contains the flaw:** it has a button that performs an action but doesn't check for an anti-CSRF token.
*   **`attacker.html`**: This page simulates a malicious website. It contains a disguised link that points to the vulnerable page but adds a malicious instruction (`?email=...`) to the end of the URL.

### Live Demonstration Steps:

1.  **Visit the Attacker's Page:** The user is tricked into visiting the attacker's website.
    *   **Link:** [https://akhilagilgal.io/attacker.html](https://akhilagilgal.io/attacker.html)

2.  **Click the Malicious Link:** The user clicks a link disguised as a prize or a required update. This action takes them to the vulnerable website, but with the attacker's malicious URL.

3.  **Trigger the Flaw:** On the vulnerable page, the user clicks a normal-looking button like "Apply Profile Update".

4.  **Attack Succeeds:** The script on the vulnerable page blindly reads the email address from the attacker's URL and performs the change. The user's email is changed without their consent.

---

## Required Screenshots (The Proof)

To properly document this proof of concept, three screenshots should be taken in order. Each screenshot should include the browser's address bar to show the URL.

#### Screenshot 1: The Lure
*   **What:** The `attacker.html` page.
*   **Why:** It shows the social engineering tactic used to trick the user.

#### Screenshot 2: The Payload in the URL
*   **What:** The `vulnerable.html` page *after* clicking the attacker's link, but *before* clicking the final button.
*   **Why:** This is the key evidence. It shows the victim's website loaded with the attacker's malicious data clearly visible in the URL.

#### Screenshot 3: The Successful Attack
*   **What:** The `vulnerable.html` page *after* clicking the "Apply Profile Update" button.
*   **Why:** This shows the final result. The email on the page has been changed, proving the attacker's payload was executed successfully.

---
