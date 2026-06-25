# o2cloud_login — Glass Néon 🌙

A dark **glassmorphism** login skin for RoundCube Webmail. Animated indigo/cyan gradient halos, a frosted-glass card, FontAwesome icons and a smooth hover-lift button — only the login page changes, the rest of your webmail keeps the standard **Elastic** look and feel.

![Démo](https://img.shields.io/badge/Démo-en%20ligne-06b6d4?style=for-the-badge) ← [Voir la démo](https://o2cloud.fr/demos/demo-glass-neon.html)

## ✨ Features

- **Glassmorphism card** : translucent background, blur, subtle border, soft shadow
- **Animated gradient background** : two floating blurred halos (indigo / cyan) on a deep navy base
- **FontAwesome icons** in the username/password fields
- **Show/hide password** toggle
- **Chrome autofill fix** : no more yellow background on autofilled fields
- **Fully responsive** : adapts down to small mobile screens
- **Zero impact on the rest of the webmail** : only the login page is touched, the inbox/settings/etc. stay on standard Elastic
- **No build step** : plain CSS + vanilla JS, just drop the folder in and go

## 🚀 Installation

### Prerequisites

- RoundCube Webmail ≥ 1.6 (skin inheritance via `meta.json` `extends` requires this)
- The **Elastic** skin must remain installed (this skin extends it, it does not replace it)
- Outbound HTTPS access from the server (Google Fonts + FontAwesome CDN)

### Quick install

```bash
# 1. Extract the skin into RoundCube's skins/ directory
unzip o2cloud_login_skin.zip -d /var/www/roundcube/skins/

# 2. Fix ownership/permissions
chown -R www-data:www-data /var/www/roundcube/skins/o2cloud_login

# 3. Activate it in config.inc.php
echo "\$config['skin'] = 'o2cloud_login';" >> /var/www/roundcube/config/config.inc.php

# 4. Clear RoundCube's compiled-template cache
rm -rf /var/www/roundcube/temp/*
```

Hard-refresh your browser (Ctrl+Shift+R) and you're done.

---

## ⚙️ Customization

All visual settings are CSS variables at the top of `styles/login.css` :

```css
:root {
    --o2c-primary: #6366f1;        /* Indigo */
    --o2c-accent: #06b6d4;         /* Cyan */
    --o2c-bg-1: #0b1021;
    --o2c-bg-2: #181133;
    --o2c-bg-3: #0f172a;
    --o2c-radius-lg: 22px;
}
```

Welcome text and placeholders are set in `scripts/login.js`, function `init()`.

---

## 📁 File structure

| Path | Description |
|---|---|
| `meta.json` | Skin declaration — `"extends": "elastic"` |
| `templates/login.html` | Login page template (overrides Elastic's, adds our CSS/JS) |
| `styles/login.css` | All visual styling, scoped to `body.task-login` |
| `scripts/login.js` | Injects icons, password toggle, title — never touches auth logic |

No other file is provided or modified: every other page of RoundCube (inbox, settings, calendar...) keeps rendering through the standard Elastic skin.

---

## 🔍 Troubleshooting

**Page doesn't change after install**
```bash
grep "skin" config/config.inc.php   # must show $config['skin'] = 'o2cloud_login';
rm -rf temp/*                       # clear compiled-template cache
```
Then hard-refresh (Ctrl+Shift+R) — RoundCube compiles `.html` templates to PHP and caches them.

**CSS/JS return 404**
Don't prefix asset paths with the skin folder name. RoundCube already resolves root-relative paths (`/styles/login.css`) against the *active* skin folder — `/skins/o2cloud_login/styles/login.css` would resolve twice and 404.

**Username/password icon sits above the input instead of beside it**
This happens if `.input-group-prepend` (Elastic's own Bootstrap icon) isn't hidden. Already handled in `styles/login.css` — if a future RoundCube version changes that markup, check that rule first.

**Card appears too low / not vertically centered**
Elastic's core CSS sets `#login-form { top: 20vh; }`. This skin neutralizes it with `top: 0 !important` and centers `#layout-content` with `position: fixed` + flexbox — robust regardless of other hidden elements in the page.

---

## 🛠️ Technical details

- **Skin inheritance** : declared via `"extends": "elastic"` in `meta.json`. Only `templates/login.html` is overridden — every other Elastic template/asset is inherited automatically, including future RoundCube core updates.
- **Field selection** : `scripts/login.js` selects inputs by `name="_user"` / `name="_pass"` rather than by `id`, since the exact ID (`rcmloginuser` vs `rcmloginpwd`, etc.) can vary across RoundCube versions/configs.
- **No authentication logic is touched** : the script only adds decorative DOM (icons, labels, button replacement) around RoundCube's own form fields. The real `<input>` elements stay in the form and submit exactly as before.
- **Zero dependencies / no build step** : plain CSS + vanilla JS, loaded via `<link>`/`<script>` tags. Google Fonts and FontAwesome are loaded from their public CDNs.

---

## ⚠️ Security notes

- This skin contains no credentials and no server-side code — it's pure front-end (CSS/JS) layered on top of RoundCube's own login form.
- It depends on two third-party CDNs (Google Fonts, FontAwesome via cdnjs). For air-gapped or high-security environments, self-host these assets and update the `<link>` tags in `templates/login.html` accordingly.
- Always review third-party skins before deploying to production, as with any code that runs in your users' browsers.

---

## 📄 Changelog

| Version | Date | Changes |
|---|---|---|
| v1.0.0 | June 2026 | Initial release — glassmorphism dark theme, animated halos, FontAwesome icons, password toggle, Chrome autofill fix, fixed vertical centering |

---

## 🤝 Contributing

1. Fork the project
2. Create a feature branch (`git checkout -b feature/my-feature`)
3. Commit your changes (`git commit -m 'Add my feature'`)
4. Push to the branch (`git push origin feature/my-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## 👤 Author

**o2Cloud**
- GitHub: [@o2Cloud-fr](https://github.com/o2Cloud-fr)
- Email: github@o2cloud.fr

---

[![RoundCube](https://img.shields.io/badge/RoundCube-%E2%89%A5%201.6-1a82e2?style=for-the-badge)](https://roundcube.net)
[![Elastic skin required](https://img.shields.io/badge/Requires-Elastic%20skin-6366f1?style=for-the-badge)](https://github.com/roundcube/roundcubemail)
[![No build step](https://img.shields.io/badge/Build%20step-none-success?style=for-the-badge)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

---

⭐ **If this skin helps you, feel free to give it a star!**

> **Note**: This skin only replaces the RoundCube login page. It does not modify authentication logic, account data, or any other part of the webmail.
