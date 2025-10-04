# 📱 iMessage Beautiful Link Preview Setup

## Overview

When users share your Grave profile links in iMessage, they'll now see a beautiful rich preview with:
- 🖼️ Custom image/thumbnail
- 📝 Title: "Check out my dating graveyard 🪦"
- 💬 Description about the app
- 🔗 The profile link

## What I've Set Up For You

### ✅ 1. Updated HTML with Open Graph Meta Tags

**Files Updated:**
- `index.html` - Main landing page with OG tags
- `profile.html` - New profile page template with OG tags

**Meta Tags Added:**
```html
<meta property="og:title" content="Check out my dating graveyard 🪦">
<meta property="og:description" content="I'm tracking my dates on Grave...">
<meta property="og:image" content="https://graveapp.org/og-image.png">
<meta property="og:image:width" content="1200">
<meta property="og:image:height" content="630">
```

### ✅ 2. Created OG Image Generator

**File:** `og-image-generator.html`

This is a tool to create your Open Graph image easily. Just:
1. Open `og-image-generator.html` in a browser
2. Click "Download Image"
3. Save as `og-image.png`

### ✅ 3. Fixed Share Duplication

**File:** `GraveyardProfileView.swift`

Fixed the duplicate URL issue - now sharing works cleanly:
```swift
activityItems: [shareText, profileURL]  // Clean, no duplication
```

---

## 🚀 Quick Start Guide

### Step 1: Generate Your OG Image

**Option A: Use the Generator (Easiest)**
1. Open `website_template/og-image-generator.html` in your browser
2. Click "Download Image" button
3. Save as `og-image.png`

**Option B: Create in Design Tool**
Use Figma, Canva, or Photoshop:
- Size: **1200x630 pixels** (required)
- Format: PNG or JPG
- Max file size: 5MB (recommended: under 1MB)

**Design Tips:**
- Use the purple gradient: `#667eea` to `#764ba2`
- Include the 🪦 emoji prominently
- Add text: "Check out my graveyard on Grave!"
- Make it eye-catching and on-brand

### Step 2: Upload to Your Website

Your website structure should be:
```
graveapp.org/
├── index.html
├── profile.html
├── og-image.png          ← Add this!
└── .well-known/
    └── apple-app-site-association
```

### Step 3: Deploy Your Website

**Recommended Hosts:**
- **Vercel** (easiest, free, auto HTTPS)
- **Netlify** (great for static sites)
- **GitHub Pages** (free, but manual HTTPS setup)
- **Cloudflare Pages** (fast, free)

**Deployment Steps (Vercel):**
```bash
# Install Vercel CLI
npm i -g vercel

# Deploy from website_template directory
cd website_template
vercel

# Set up your custom domain
vercel domains add graveapp.org
```

### Step 4: Configure Your Domain

Point your domain (`graveapp.org`) to your host:
- **Vercel/Netlify**: Follow their domain setup guide
- Make sure HTTPS is enabled (usually automatic)
- Wait for DNS propagation (5-30 minutes)

### Step 5: Update Your App

The app already points to `https://graveapp.org`, so once your website is live, it should work automatically!

### Step 6: Test It!

1. Build and run your iOS app
2. Open a graveyard profile
3. Tap the share button
4. Send the link to yourself in iMessage
5. Wait 2-3 seconds for the preview to load
6. 🎉 See your beautiful preview!

---

## 🧪 Testing & Debugging

### Test Your Link Preview

Before testing in iMessage, validate your OG tags:

1. **Facebook Sharing Debugger** (works for iMessage too!)
   https://developers.facebook.com/tools/debug/
   - Enter your URL
   - Click "Scrape Again" to refresh cache
   - Check that image, title, and description load

2. **Twitter Card Validator**
   https://cards-dev.twitter.com/validator
   - Good for testing cross-platform compatibility

3. **LinkedIn Post Inspector**
   https://www.linkedin.com/post-inspector/

### Common Issues & Fixes

**❌ No preview shows in iMessage**
- **Cause**: Website not accessible via HTTPS
- **Fix**: Make sure your site is deployed and HTTPS is enabled
- **Fix**: Check that `og-image.png` is publicly accessible

**❌ Old preview shows after updating**
- **Cause**: iMessage caches aggressively (24-48 hours)
- **Fix**: Change image filename to `og-image-v2.png` and update HTML
- **Fix**: Use Facebook Debugger to force refresh
- **Fix**: Wait 24-48 hours for cache to clear

**❌ Image doesn't display**
- **Cause**: Wrong image size or format
- **Fix**: Verify image is exactly 1200x630px
- **Fix**: Make sure file size is under 5MB
- **Fix**: Use absolute URL: `https://graveapp.org/og-image.png`

**❌ App doesn't open from link**
- **Cause**: Universal Links not configured
- **Fix**: Check `apple-app-site-association` file is at `/.well-known/`
- **Fix**: Verify domain in Xcode capabilities
- **Fix**: Make sure website and app are deployed

---

## 🎨 Design Examples

### Simple & Clean
```
┌────────────────────────────────────┐
│                                    │
│           🪦                       │
│                                    │
│   Check out my graveyard!          │
│                                    │
│   Download Grave to see my         │
│   dating fails and yours           │
│                                    │
└────────────────────────────────────┘
```

### With Stats (if you want dynamic later)
```
┌────────────────────────────────────┐
│   🪦 Sarah's Graveyard             │
│                                    │
│       42 Dates                     │
│       🏆 #1 on Leaderboard         │
│                                    │
│   Download Grave to see more       │
└────────────────────────────────────┘
```

---

## 📊 Advanced: Dynamic Per-User Images

For personalized OG images showing each user's stats:

### Option 1: Vercel + @vercel/og (Recommended)

Create a Next.js app with dynamic OG generation:

```typescript
// app/api/og/route.tsx
import { ImageResponse } from '@vercel/og';

export async function GET(request: Request) {
  const { searchParams } = new URL(request.url);
  const userId = searchParams.get('userId');
  
  // Fetch from Supabase
  const user = await supabase
    .from('profiles')
    .select('*')
    .eq('id', userId)
    .single();
  
  return new ImageResponse(
    (
      <div style={{
        background: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)',
        width: '100%',
        height: '100%',
        display: 'flex',
        flexDirection: 'column',
        alignItems: 'center',
        justifyContent: 'center',
        color: 'white',
      }}>
        <div style={{ fontSize: 80 }}>🪦</div>
        <div style={{ fontSize: 60, fontWeight: 'bold' }}>
          {user.display_name}'s Graveyard
        </div>
        <div style={{ fontSize: 40 }}>
          {user.roster_entries_count} dates
        </div>
      </div>
    ),
    {
      width: 1200,
      height: 630,
    }
  );
}
```

Then update your meta tags to:
```html
<meta property="og:image" content="https://graveapp.org/api/og?userId=USER_ID">
```

### Option 2: Cloudinary Transformations

Use Cloudinary's URL-based image generation to overlay text on a template.

### Option 3: Pre-generated Images

Generate images on the server when users update their profiles and store in cloud storage.

---

## 🎯 Checklist

- [ ] Generate `og-image.png` (1200x630px)
- [ ] Upload to website root
- [ ] Deploy website to `graveapp.org`
- [ ] Verify HTTPS is enabled
- [ ] Test with Facebook Debugger
- [ ] Test in iMessage with real link
- [ ] Verify universal links open the app
- [ ] Share with friends and celebrate! 🎉

---

## 📚 Resources

- [Open Graph Protocol](https://ogp.me/)
- [Facebook Sharing Debugger](https://developers.facebook.com/tools/debug/)
- [Twitter Card Validator](https://cards-dev.twitter.com/validator)
- [Vercel OG Image Generation](https://vercel.com/docs/concepts/functions/edge-functions/og-image-generation)
- [Apple Universal Links](https://developer.apple.com/ios/universal-links/)

---

## 💡 Pro Tips

1. **Test early and often** - Don't wait until production to test OG images
2. **Use version numbers** - Add `?v=2` to image URL when updating
3. **Keep it simple** - A clean design works better than cluttered
4. **Brand consistency** - Match your app's colors and style
5. **Mobile-first** - Remember most people will see this on mobile
6. **Fast loading** - Keep image under 1MB for quick previews

---

## Need Help?

If previews aren't working:
1. Check Facebook Debugger first
2. Verify image is publicly accessible
3. Confirm HTTPS is working
4. Clear iMessage cache (restart app)
5. Wait 24-48 hours for DNS/cache propagation

Enjoy your beautiful iMessage previews! 🎉🪦

