### מה האוטומציה עושה

1. נכנסת ל-https://test.netlify.app/
2. ממלאת את השדות Name, Email, Phone, Company, Website
3. **בונוס:** משנה את Number of Employees מ-`1-10` ל-`51-500`
4. מצלמת את העמוד לפני הלחיצה על "Request a call back"
5. לוחצת על "Request a call back"
6. מדפיסה ל-console כשמגיעה לעמוד ה-Thank You

### output

```
Employees = 51-500
Reached the thank you page:
 https://test.netlify.app/thank-you.html?name=Israel+Israeli&email=...
```

צילום המסך נשמר תחת `screenshots/before-submit-<timestamp>.png`.


## code

```javascript
const { chromium } = require('playwright');

(async () => {
  const browser = await chromium.launch({ headless: false, slowMo: 300 });
  const page = await browser.newPage();

  try {
    await page.goto('https://test.netlify.app/');

    await page.getByLabel('Name').fill('Israel Israeli');
    await page.getByLabel('Email').fill('israel@example.com');
    await page.getByLabel('Phone').fill('0501234567');
    await page.getByLabel('Company').fill('Jones Automation');
    await page.getByLabel('Website').fill('https://example.com');

    const employees = page.getByLabel('Number of Employees');
    await employees.selectOption({ label: '51-500' });
    console.log('Employees =', await employees.inputValue());

    await page.locator(':focus').blur();

    const stamp = new Date().toISOString().replace(/[:.]/g, '-');
    await page.screenshot({ path: `screenshots/before-submit-${stamp}.png`, fullPage: true });

    await page.getByRole('button', { name: 'Request a call back' }).click();

    await Promise.race([
      page.waitForURL(/thank|success/i),
      page.getByText(/thank you/i).waitFor(),
    ]);

    console.log('Reached the thank you page:\n', page.url());
  } catch (err) {
    await page.screenshot({ path: 'error.png', fullPage: true });
    console.error('❌ Failed:', err.message);
    throw err;
  } finally {
    await browser.close();
  }
})();
```
