# Grave App Website Template

This folder contains everything you need to deploy your website for universal links.

## 📁 Files Included

- `index.html` - Beautiful landing page
- `apple-app-site-association` - Required for universal links (in root)
- `.well-known/apple-app-site-association` - Same file, alternative location

## 🚀 Quick Deploy Options

### Option 1: GitHub Pages (Free)

1. Create a new GitHub repository
2. Upload all files from this folder
3. Go to Settings → Pages
4. Enable Pages from main branch
5. Point your domain `grave.app` to GitHub Pages

### Option 2: Vercel (Free & Easiest)

1. Sign up at [vercel.com](https://vercel.com)
2. Click "New Project"
3. Upload this folder (or connect to GitHub)
4. Point your domain `grave.app`
5. Done! Automatic SSL and deployment

### Option 3: Cloudflare Pages (Free)

1. Sign up at [cloudflare.com](https://cloudflare.com)
2. Pages → Create a project
3. Upload this folder
4. Point your domain

## 🔧 Domain Setup

You need to own `grave.app` (or change it in your app code).

**To point your domain:**
- **GitHub Pages**: Add CNAME record to `YOUR-USERNAME.github.io`
- **Vercel**: Add DNS records as shown in Vercel dashboard
- **Cloudflare**: Automatic if using Cloudflare as DNS

## ✅ Verify Setup

After deploying:

1. Visit `https://grave.app/apple-app-site-association`
   - Should show JSON (not 404)
   - Must be HTTPS

2. Test with Apple's validator:
   - https://search.developer.apple.com/appsearch-validation-tool/

3. Wait 24 hours for Apple's CDN to cache

## 📝 Customization

### Update the Landing Page
Edit `index.html`:
- Change colors in the `<style>` section
- Update tagline and features
- Add screenshots or app preview

### Update Links
The page automatically redirects iOS users to your App Store page.
Your App Store ID is already configured: `6753152159`

## 🧪 Testing

1. Deploy the site
2. Send yourself: `https://grave.app/profile/test123`
3. Tap on iPhone (with app installed)
4. Should open your app directly

## ⚠️ Important

- File must be served over HTTPS (not HTTP)
- No redirects for the apple-app-site-association file
- Content-Type should be `application/json`
- Wait up to 24 hours for universal links to work after first deployment

## 🆘 Need Help?

See the main `UNIVERSAL_LINKS_SETUP.md` in the root folder for detailed instructions.


