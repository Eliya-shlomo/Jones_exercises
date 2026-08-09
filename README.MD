### What the Automation Does
Navigates to https://test.netlify.app/
Fills in the Name, Email, Phone, Company and Website fields
Bonus: changes Number of Employees from 1-10 to 51-500
Takes a full-page screenshot before submitting
Clicks the "Request a call back" button
Logs to the console once the Thank You page is reached

### Getting Started

Prerequisites: Node.js 18+

bash
npm install
npx playwright install chromium
node automation.js

### output

```
Employees = 51-500
Reached the thank you page:
 https://test.netlify.app/thank-you.html?name=Israel+Israeli&email=...
```

צילום המסך נשמר תחת `screenshots/before-submit-<timestamp>.png`.


