---

### 📄 README.md (להכניס לגיטהאב)

```markdown
# 🔑 Universal Google Login Redirect

This project provides a simple **Google Sign-In page** that can be hosted on **GitHub Pages**.  
It allows any website to redirect users here for login, then send them back with their **Google email** and **ID token**.

---

## 🚀 How it works

1. Any website sends the user to:
```

[https://YOUR_USERNAME.github.io/login.html?return_to=https://example.com/after](https://YOUR_USERNAME.github.io/login.html?return_to=https://example.com/after)

```
2. The user signs in with Google.  
3. If successful → user sees "Welcome" message with animation.  
Then automatically redirected to `return_to` with:
- `email` (Google account email)
- `id_token` (JWT that can be verified by your server)

Example final redirect:
```

[https://example.com/after?email=user@gmail.com&id_token=XYZ](https://example.com/after?email=user@gmail.com&id_token=XYZ)

```

4. If login fails → user sees:
```

לא הצלחנו לחבר אותך, אנא נסה שוב / התחבר עם חשבון אחר.

```

---

## ⚙️ Setup

### 1. Create Google OAuth Client
- Go to [Google Cloud Console](https://console.cloud.google.com/apis/credentials).
- Create a **OAuth Client ID** for **Web Application**.
- Under **Authorized JavaScript origins**, add:
```

[https://YOUR_USERNAME.github.io](https://YOUR_USERNAME.github.io)

````
- Copy your **Client ID**.

### 2. Configure `login.html`
- Open `login.html`.
- Replace:
```html
data-client_id="YOUR_CLIENT_ID.apps.googleusercontent.com"
````

with your real Client ID.

### 3. Deploy to GitHub Pages

* Commit `login.html` and `README.md` to your repo.
* In repo settings → **Pages** → set branch to `main` → `/ (root)` folder.
* Access page at:

  ```
  https://YOUR_USERNAME.github.io/login.html
  ```

---

## 📄 Example usage

### From your website:

```html
<a href="https://YOUR_USERNAME.github.io/login.html?return_to=https://mysite.com/auth-done">
  Login with Google
</a>
```

### On your site (receiver):

The `auth-done` page receives `email` and `id_token`:

```js
const params = new URLSearchParams(window.location.search);
const email = params.get("email");
const idToken = params.get("id_token");

console.log("User email:", email);
console.log("Google ID Token:", idToken);

// (Optional) Verify token on your backend:
fetch("https://oauth2.googleapis.com/tokeninfo?id_token=" + encodeURIComponent(idToken))
  .then(r => r.json())
  .then(data => console.log("Verified payload:", data));
```

---

## 📝 Notes

* The `id_token` is a **JWT** signed by Google.
  Always verify it on the backend before granting full access.
* The login page itself does **not** store anything. It just returns `email` + `id_token`.
* Universal: Any site can use it by specifying `return_to` parameter.

---

## 🔄 Example Flow

1. User clicks "Login with Google" on your website.
2. Redirected to GitHub Pages login.
3. Signs in → sees welcome screen.
4. Redirected back to your site with email + id_token.

```
YourSite → GitHub Pages (login.html) → Back to YourSite
```

````

---

### 📄 config.json (אופציונלי, אם תרצה לשמור Client ID בנפרד)
```json
{
  "client_id": "YOUR_CLIENT_ID.apps.googleusercontent.com"
}
````

ואז בקובץ `login.html` תוכל לטעון אותו עם:

```js
fetch("config.json")
  .then(r => r.json())
  .then(cfg => {
    document.getElementById("g_id_onload").setAttribute("data-client_id", cfg.client_id);
  });
```

---
?
