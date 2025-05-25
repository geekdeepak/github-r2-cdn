# Demo Assets

This folder demonstrates the proper folder structure for the GitHub R2 CDN platform.

## 📁 Folder Structure

```
demo-assets/
├── images/           # Place images here for compression only
│   └── .gitkeep     # Keeps folder in git
└── images-webp/     # Place images here for compression + WebP generation
    └── .gitkeep     # Keeps folder in git
```

## 🔄 How It Works

1. **images/ folder**: Images placed here will be compressed but keep their original format
2. **images-webp/ folder**: Images placed here will be compressed AND converted to WebP format

## 🚀 Getting Started

1. Replace this README with your own assets
2. Add your images to the appropriate folders
3. Push to trigger the optimization workflow
4. Watch your images get optimized automatically!

## 📋 Supported Formats

- ✅ PNG
- ✅ JPEG/JPG
- ✅ GIF (copied as-is)
- ✅ SVG (copied as-is)

## 🎯 Best Practices

- Use **images/** for logos, icons, and graphics you want to keep in original format
- Use **images-webp/** for photos, hero images, and content where WebP provides significant savings
- Organize by content type or page sections for easier management