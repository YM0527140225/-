### 📄 README.md

```markdown
# Universal Google Login Page (GitHub Pages)

דף התחברות אוניברסלי עם Google, מבוסס על **Google Identity Services (GIS)**.  
ניתן להשתמש בו מכל אתר ע"י הפניה עם פרמטר `redirect`.  
לאחר התחברות המשתמש, הדף יחזיר אותו חזרה לכתובת שהוגדרה עם פרטי המשתמש.

---

## 🚀 התקנה

1. העתק את הקובץ [`login.html`](./login.html) לריפוזיטורי שלך.
2. הפעל **GitHub Pages** בריפוזיטורי (מומלץ `main` → `/root`).
3. ה־URL של הדף יהיה בסגנון:
```

https://<username>.github.io/<repo>/login.html

```

---

## ⚙️ הגדרות Google Cloud

1. עבור ל־[Google Cloud Console](https://console.cloud.google.com/).
2. צור או בחר **OAuth 2.0 Client ID** מסוג **Web Application**.
3. הוסף את כתובת GitHub Pages שלך לרשימת **Authorized JavaScript origins**.  
לדוגמה:
```

https://<username>.github.io

````
4. קבל את ה־**Client ID** והחלף את הערך בשורה הזו בתוך `login.html`:
```html
data-client_id="YOUR_CLIENT_ID.apps.googleusercontent.com"
````

---

## 🖥️ שימוש

הפנה את המשתמש לכתובת `login.html` עם פרמטר `redirect` לכתובת אליה תרצה שיחזור:

```
https://<username>.github.io/<repo>/login.html?redirect=https://yourapp.com/callback
```

### דוגמה:

```
https://myname.github.io/google-login/login.html?redirect=https://script.google.com/macros/s/AKfycbx12345/exec
```

---

## 📤 נתונים שמוחזרים

לאחר התחברות, המשתמש מופנה חזרה לכתובת `redirect` עם פרמטרים:

* `email` — כתובת המייל של המשתמש.
* `name` — שם מלא (אם זמין).
* `token` — ה־ID Token שהתקבל מגוגל.

### דוגמה:

```
https://yourapp.com/callback?email=user%40gmail.com&name=John%20Doe&token=eyJhbGciOiJSUzI1NiIsImtpZCI6IjY...
```

---

## ❌ טיפול בשגיאות

* אם ההתחברות נכשלה, המשתמש יראה הודעה:

  ```
  לא הצלחנו לחבר אותך, אנא נסה שוב / התחבר עם חשבון אחר.
  ```
* במקרה כזה הוא יישאר בדף ההתחברות (`login.html`).

---

## 🎨 ממשק

* בעת התחברות מוצלחת:

  * מוצגת הודעת **"ברוך הבא + המייל"**
  * אנימציית טעינה
  * לאחר ~2 שניות מתבצעת הפניה אוטומטית ל־`redirect`

---

## 📌 הערות

* ה־`token` שמוחזר הוא **ID Token JWT** של Google.
  ניתן לאמת אותו בשרת בעזרת קריאה ל־`https://oauth2.googleapis.com/tokeninfo?id_token=...`.
* לשימוש מאובטח – עדיף לאמת את ה־token בצד שרת ולא להסתמך רק על הפרמטרים מהדפדפן.
