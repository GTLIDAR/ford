# ReLIC Project Site — Decap CMS + Jekyll + GitHub Pages

## Step-by-Step Setup Guide

### Step 1: Push this repo to GitHub.com

```bash
cd relic-project
git init
git add .
git commit -m "initial site setup"
git remote add origin https://github.com/YOUR-USERNAME/relic-project.git
git push -u origin main
```

> Replace `YOUR-USERNAME` with your actual GitHub username.

---

### Step 2: Enable GitHub Pages

1. Go to your repo on github.com
2. Click **Settings** → **Pages**
3. Under "Source", select **Deploy from a branch**
4. Choose **main** branch, **/ (root)** folder
5. Click **Save**
6. Wait 1-2 minutes — your site will be live at:
   ```
   https://YOUR-USERNAME.github.io/relic-project/
   ```

---

### Step 3: Set up Netlify for authentication (required for Decap CMS)

Decap CMS needs an OAuth provider to let you log in with GitHub. Netlify provides this for free.

1. Go to [netlify.com](https://www.netlify.com/) and sign up (free)
2. Click **Add new site** → **Import an existing project**
3. Connect your GitHub account and select the `relic-project` repo
4. Deploy settings:
   - Build command: `jekyll build`
   - Publish directory: `_site`
5. Click **Deploy**

> **Note:** You don't need to use Netlify for hosting — you're only using it for OAuth. Your site stays on GitHub Pages.

---

### Step 4: Enable GitHub OAuth on Netlify

1. In Netlify, go to **Site configuration** → **Identity**
2. Click **Enable Identity**
3. Under **Registration**, select **Invite only**
4. Under **External providers**, click **Add provider** → **GitHub**
5. Leave defaults and click **Enable GitHub**
6. Go to **Services** → **Git Gateway** → **Enable Git Gateway**

---

### Step 5: Add Netlify Identity widget to your site

This is already done! The `admin/index.html` file loads Decap CMS from the CDN.

But you also need to add the Netlify Identity widget to your main site. Add this script tag to your `_layouts/default.html` in the `<head>`:

```html
<script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
```

And add this before `</body>`:

```html
<script>
  if (window.netlifyIdentity) {
    window.netlifyIdentity.on("init", user => {
      if (!user) {
        window.netlifyIdentity.on("login", () => {
          document.location.href = "/admin/";
        });
      }
    });
  }
</script>
```

---

### Step 6: Invite yourself as a user

1. In Netlify dashboard → **Identity** → **Invite users**
2. Enter your email address
3. Check your email and accept the invitation
4. Set a password

---

### Step 7: Access the CMS!

Go to:
```
https://YOUR-USERNAME.github.io/relic-project/admin/
```

Log in with the email/password you set up in Step 6. You'll see a GUI where you can edit:
- Hero title
- Authors list
- Abstract text
- Links
- Method description
- BibTeX

Any changes you make will be committed directly to your GitHub repo, and GitHub Pages will rebuild automatically.

---

## File Structure

```
relic-project/
├── _config.yml              # Jekyll configuration
├── _layouts/
│   └── default.html         # HTML template with all CSS
├── _data/
│   └── site_content.yml     # ← Decap CMS edits THIS file
├── index.html               # Main page (reads from site_content.yml)
├── admin/
│   ├── index.html           # Decap CMS entry point
│   └── config.yml           # Decap CMS config (defines editable fields)
├── assets/
│   ├── images/              # Put your images here
│   └── videos/              # Put your videos here
├── Gemfile                  # Ruby dependencies
└── README.md                # This file
```

## How it works

1. **You** open `/admin/` and edit content in the GUI
2. **Decap CMS** saves changes to `_data/site_content.yml` in your GitHub repo
3. **GitHub Pages** detects the commit and rebuilds the Jekyll site
4. **Jekyll** reads `site_content.yml` and injects the data into `index.html`
5. **Your site** updates automatically in 1-2 minutes

## Editing tips

- To add images: use the media upload button in the CMS
- To edit HTML/CSS: edit `_layouts/default.html` directly on GitHub
- To add new sections: edit both `admin/config.yml` (add fields) and `index.html` (add template code)
