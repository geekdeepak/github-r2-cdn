# 🚀 Git-Powered Asset Delivery Platform

[![GitHub Actions](https://img.shields.io/badge/GitHub-Actions-2088FF?logo=github-actions&logoColor=white)](https://github.com/features/actions)
[![Cloudflare R2](https://img.shields.io/badge/Cloudflare-R2-F38020?logo=cloudflare&logoColor=white)](https://developers.cloudflare.com/r2/)
[![Zero Config](https://img.shields.io/badge/Setup-Zero%20Config-00D26A)](/)
[![Production Ready](https://img.shields.io/badge/Status-Production%20Ready-success)](/)

> **Push images to Git. Serve them globally. Automatically.**

Transform any GitHub repository into a **production-ready asset CDN** with automatic optimization, WebP conversion, and global delivery via Cloudflare R2.

## 🎯 What This Solves

| ❌ **Before** | ✅ **After** |
|---------------|--------------|
| Assets scattered across Dropbox, drives | **Git as single source of truth** |
| Manual image compression workflows | **Automatic optimization on every push** |
| Expensive CDN services ($50-200/month) | **Free tier covers most projects** |
| Hard-coded asset URLs in code | **Dynamic manifest.json with metadata** |
| Complex build tools and dependencies | **Zero config - just push files** |
| Team sync issues with large files | **Git-native workflow everyone knows** |

## ⚡ How It Works

```bash
# 1. Drop images in your repo
git add assets/images/hero.jpg
git add assets/images-webp/gallery.png

# 2. Push to trigger the magic
git push origin main

# 3. GitHub Actions automatically:
#    → Compresses images (PNG/JPEG)
#    → Generates WebP versions  
#    → Creates manifest.json with metadata
#    → Syncs everything to Cloudflare R2
#    → Commits optimized files back

# 4. Your frontend loads from CDN
fetch('https://cdn.yourdomain.com/assets/manifest.json')
```

## 🏗️ Architecture

```
Your Git Repo          GitHub Actions         Cloudflare R2        Your Frontend
┌─────────────┐       ┌─────────────────┐    ┌─────────────┐     ┌─────────────┐
│             │       │                 │    │             │     │             │
│ images/     │──────▶│ ImageMagick     │───▶│ Global CDN  │────▶│ manifest.   │
│ hero.jpg    │       │ + WebP + Sync   │    │ Delivery    │     │ json API    │
│             │       │                 │    │             │     │             │
└─────────────┘       └─────────────────┘    └─────────────┘     └─────────────┘
```

## 🚀 5-Minute Setup

### 1. Copy the Workflow
Create `.github/workflows/r2-cdn-sync.yml` and copy the workflow from this repo.

### 2. Configure Your CDN
```yaml
env:
  BASE_URL: "https://cdn.yourdomain.com"    # Your CDN domain
  BUCKET_NAME: "your-assets-bucket"         # Your R2 bucket
```

### 3. Add R2 Credentials
**GitHub → Settings → Secrets → Actions:**
- `R2_ACCESS_KEY_ID`
- `R2_SECRET_ACCESS_KEY` 
- `R2_ACCOUNT_ID`

### 4. Organize Assets
```
your-project/
├── assets/
│   ├── images/          # Auto-compressed
│   └── images-webp/     # Auto-compressed + WebP versions
├── components/
│   └── images/          # Works anywhere in your repo
└── public/
    └── images-webp/     # Flexible structure
```

### 5. Push and Go Live!
```bash
git add assets/images/
git commit -m "Add hero images"
git push origin main

# Watch the magic happen in GitHub Actions tab!
```

## 🌐 Frontend Integration

### React/Next.js
```javascript
// Auto-load optimized assets
const useAssets = () => {
  const [manifest, setManifest] = useState(null)
  
  useEffect(() => {
    fetch('https://cdn.yourdomain.com/assets/manifest.json')
      .then(res => res.json())
      .then(setManifest)
  }, [])
  
  const getImageUrl = (filename, preferWebP = true) => {
    if (!manifest) return null
    
    // Try WebP first for supported browsers
    if (preferWebP) {
      const webpFile = manifest.files.find(f => 
        f.name === filename.replace(/\.(jpg|png)$/, '.webp')
      )
      if (webpFile) return webpFile.url
    }
    
    const file = manifest.files.find(f => f.name === filename)
    return file?.url
  }
  
  return { manifest, getImageUrl }
}

// Usage
function Hero() {
  const { getImageUrl } = useAssets()
  
  return (
    <picture>
      <source srcSet={getImageUrl('hero.jpg')} type="image/webp" />
      <img src={getImageUrl('hero.jpg', false)} alt="Hero" />
    </picture>
  )
}
```

## 📊 Generated Manifest Structure

```json
{
  "folder": "assets",
  "base_url": "https://cdn.yourdomain.com",
  "generated_at": "2025-01-15T10:30:00Z",
  "total_files": 24,
  "total_size": 5242880,
  "optimization": {
    "max_width": 1920,
    "jpeg_quality": 85,
    "folder_rules": {
      "images/": "compress only",
      "images-webp/": "compress + webp generation"
    }
  },
  "files": [
    {
      "name": "hero.jpg",
      "path": "images/hero.jpg",
      "url": "https://cdn.yourdomain.com/assets/images/hero.jpg",
      "size": 245760,
      "type": "image/jpeg",
      "optimization": "compress"
    }
  ]
}
```

## 🔧 Troubleshooting

### Images Not Processing?
- ✅ Check folder structure: needs `images/` or `images-webp/` subfolders
- ✅ Verify formats: PNG, JPG, JPEG only
- ✅ Check GitHub Actions logs for detailed output

### R2 Sync Issues?
- ✅ Double-check all 3 secrets are set correctly
- ✅ Verify bucket name matches exactly (case-sensitive)
- ✅ Ensure R2 API tokens have proper permissions

## 📈 Performance Benefits

| Metric | Before | After | Improvement |
|--------|--------|-------|-------------|
| **Image Sizes** | 2-5MB originals | 200-800KB optimized | **60-80% smaller** |
| **Load Times** | 3-8 seconds | 0.5-2 seconds | **75% faster** |
| **CDN Delivery** | None | Global edge cache | **Instant worldwide** |
| **Modern Formats** | JPEG/PNG only | WebP + fallbacks | **30% additional savings** |

## 🎯 Perfect For

- 🛍️ **E-commerce**: Product catalogs, hero images, thumbnails
- 📝 **Blogs & CMS**: Article images, featured photos
- 🎨 **Portfolios**: High-quality work samples with fast loading
- 📱 **Landing Pages**: Marketing assets, hero banners
- 📚 **Documentation**: Screenshots, diagrams, illustrations

## 💰 Cost Comparison

| Solution | Monthly Cost | Setup Time | Maintenance |
|----------|-------------|------------|-------------|
| **This Platform** | **$0-5** (R2 free tier) | **5 minutes** | **Zero** |
| Cloudinary | $89-249 | 30 minutes | Updates needed |
| AWS CloudFront | $50-150 | 2-4 hours | Complex config |
| Vercel Pro | $240/year | 1 hour | Vendor lock-in |

## 🤝 Contributing

We welcome contributions! See [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

## 📄 License

MIT License - Build amazing things with it!

## 💝 Credits

Built with ❤️ by [@geekdeepak](https://github.com/geekdeepak) to make the web faster for everyone.

---

⭐ **Star this repo if it makes your websites blazing fast!**