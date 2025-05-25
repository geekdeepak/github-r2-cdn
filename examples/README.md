# 📚 Usage Examples

This directory contains real-world examples of how to integrate the GitHub R2 CDN with popular frameworks and use cases.

## 🚀 Framework Integrations

### Next.js Integration
```javascript
// utils/imageLoader.js
export default function cloudflareLoader({ src, width, quality }) {
  const params = new URLSearchParams()
  params.set('w', width.toString())
  if (quality) params.set('q', quality.toString())
  
  // Use WebP for supported browsers
  const supportsWebP = typeof window !== 'undefined' 
    && window.navigator?.userAgent?.includes('Chrome')
  
  const format = supportsWebP ? 'webp' : 'jpg'
  const basePath = supportsWebP ? 'images-webp' : 'images'
  
  return `https://cdn.yourdomain.com/assets/${basePath}/${src}?${params}`
}

// next.config.js
module.exports = {
  images: {
    loader: 'custom',
    loaderFile: './utils/imageLoader.js',
  },
}

// components/OptimizedImage.jsx
import Image from 'next/image'
import { useState, useEffect } from 'react'

export default function OptimizedImage({ src, alt, ...props }) {
  const [manifest, setManifest] = useState(null)
  
  useEffect(() => {
    fetch('https://cdn.yourdomain.com/assets/manifest.json')
      .then(res => res.json())
      .then(setManifest)
  }, [])
  
  return (
    <Image
      src={src}
      alt={alt}
      {...props}
      priority={manifest?.files?.find(f => f.name === src)?.size > 100000}
    />
  )
}
```

### React Hook for Asset Loading
```javascript
// hooks/useAssetManifest.js
import { useState, useEffect } from 'react'

export function useAssetManifest(folder = 'assets') {
  const [manifest, setManifest] = useState(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)
  
  useEffect(() => {
    fetch(`https://cdn.yourdomain.com/${folder}/manifest.json`)
      .then(res => res.json())
      .then(data => {
        setManifest(data)
        setLoading(false)
      })
      .catch(err => {
        setError(err)
        setLoading(false)
      })
  }, [folder])
  
  const getOptimizedUrl = (filename, preferWebP = true) => {
    if (!manifest) return null
    
    const file = manifest.files.find(f => f.name === filename)
    if (!file) return null
    
    // Try WebP version first
    if (preferWebP) {
      const webpFile = manifest.files.find(f => 
        f.name === filename.replace(/\.(jpg|png)$/, '.webp')
      )
      if (webpFile) return webpFile.url
    }
    
    return file.url
  }
  
  return { manifest, loading, error, getOptimizedUrl }
}

// Usage in component
function Gallery() {
  const { manifest, loading, getOptimizedUrl } = useAssetManifest('gallery')
  
  if (loading) return <div>Loading images...</div>
  
  return (
    <div className="grid grid-cols-3 gap-4">
      {manifest?.files?.map(file => (
        <img 
          key={file.name}
          src={getOptimizedUrl(file.name)}
          alt={file.name}
          loading="lazy"
        />
      ))}
    </div>
  )
}
```

### Vue/Nuxt Example
```vue
<template>
  <div>
    <img 
      :src="getOptimizedImage('hero.jpg')" 
      alt="Hero Image"
      loading="lazy"
    />
    
    <!-- Progressive enhancement with WebP -->
    <picture>
      <source :srcset="getWebPImage('gallery.jpg')" type="image/webp">
      <img :src="getOptimizedImage('gallery.jpg')" alt="Gallery">
    </picture>
  </div>
</template>

<script>
export default {
  data() {
    return {
      manifest: null
    }
  },
  
  async mounted() {
    try {
      const response = await fetch('https://cdn.yourdomain.com/assets/manifest.json')
      this.manifest = await response.json()
    } catch (error) {
      console.error('Failed to load manifest:', error)
    }
  },
  
  methods: {
    getOptimizedImage(filename) {
      if (!this.manifest) return `/fallback/${filename}`
      
      const file = this.manifest.files.find(f => f.name === filename)
      return file?.url || `/fallback/${filename}`
    },
    
    getWebPImage(filename) {
      if (!this.manifest) return null
      
      const webpName = filename.replace(/\.(jpg|png)$/, '.webp')
      const file = this.manifest.files.find(f => f.name === webpName)
      return file?.url
    }
  }
}
</script>
```

## 📱 Mobile Optimizations

### Progressive Loading with Intersection Observer
```javascript
// Progressive image loading
class OptimizedImageLoader {
  constructor() {
    this.observer = new IntersectionObserver(this.handleIntersection.bind(this))
    this.manifest = null
    this.loadManifest()
  }
  
  async loadManifest() {
    try {
      const response = await fetch('/assets/manifest.json')
      this.manifest = await response.json()
    } catch (error) {
      console.error('Failed to load manifest:', error)
    }
  }
  
  observe(img) {
    this.observer.observe(img)
  }
  
  handleIntersection(entries) {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        const img = entry.target
        const filename = img.dataset.src
        
        this.loadProgressive(img, filename)
        this.observer.unobserve(img)
      }
    })
  }
  
  async loadProgressive(img, filename) {
    if (!this.manifest) return
    
    const file = this.manifest.files.find(f => f.name === filename)
    if (!file) return
    
    // Check for WebP support
    const supportsWebP = await this.checkWebPSupport()
    
    if (supportsWebP) {
      const webpFile = this.manifest.files.find(f => 
        f.name === filename.replace(/\.(jpg|png)$/, '.webp')
      )
      if (webpFile) {
        img.src = webpFile.url
        return
      }
    }
    
    img.src = file.url
  }
  
  async checkWebPSupport() {
    return new Promise(resolve => {
      const webP = new Image()
      webP.onload = webP.onerror = () => {
        resolve(webP.height === 2)
      }
      webP.src = 'data:image/webp;base64,UklGRjoAAABXRUJQVlA4IC4AAACyAgCdASoCAAIALmk0mk0iIiIiIgBoSygABc6WWgAA/veff/0PP8bA//LwYAAA'
    })
  }
}

// Usage
const loader = new OptimizedImageLoader()
document.querySelectorAll('[data-src]').forEach(img => {
  loader.observe(img)
})
```

## 🔧 Advanced Configurations

### E-commerce Product Images
```javascript
// Product gallery with multiple sizes
class ProductGallery {
  constructor(productId) {
    this.productId = productId
    this.sizes = ['thumbnail', 'medium', 'large', 'zoom']
    this.loadManifest()
  }
  
  async loadManifest() {
    const response = await fetch(`/products/${this.productId}/manifest.json`)
    this.manifest = await response.json()
  }
  
  getImageUrl(filename, size = 'medium') {
    const sizedFilename = filename.replace('.', `-${size}.`)
    const file = this.manifest.files.find(f => f.name === sizedFilename)
    return file?.url || this.getImageUrl(filename, 'large')
  }
  
  render() {
    return `
      <div class="product-gallery">
        <img src="${this.getImageUrl('product-main.jpg', 'large')}" 
             alt="Product main image" 
             class="main-image">
        <div class="thumbnails">
          ${this.getThumbnails().map(thumb => `
            <img src="${thumb.url}" 
                 alt="${thumb.alt}"
                 onclick="showLarge('${thumb.large}')"
                 class="thumbnail">
          `).join('')}
        </div>
      </div>
    `
  }
}
```

### Performance Monitoring
```javascript
// Track image loading performance
function trackImagePerformance() {
  const observer = new PerformanceObserver((list) => {
    list.getEntries().forEach((entry) => {
      if (entry.initiatorType === 'img') {
        // Send to analytics
        gtag('event', 'image_load', {
          custom_parameter_url: entry.name,
          custom_parameter_duration: entry.duration,
          custom_parameter_size: entry.transferSize
        })
      }
    })
  })
  
  observer.observe({ entryTypes: ['resource'] })
}

// Compare WebP vs fallback performance
async function compareFormats() {
  const manifest = await fetch('/assets/manifest.json').then(r => r.json())
  
  manifest.files.forEach(file => {
    if (file.optimization === 'webp') {
      const originalFile = manifest.files.find(f => 
        f.name === file.name.replace('.webp', '.jpg')
      )
      
      if (originalFile) {
        const savings = ((originalFile.size - file.size) / originalFile.size) * 100
        console.log(`${file.name}: ${savings.toFixed(1)}% smaller with WebP`)
      }
    }
  })
}
```

---

## 🔗 More Examples

Each example includes complete setup instructions and customization options for different use cases.