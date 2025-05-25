# 🤝 Contributing to GitHub R2 CDN

Thanks for your interest in contributing! This project aims to be the best asset delivery platform for GitHub + Cloudflare R2.

## 🚀 Quick Start for Contributors

1. **Fork & Clone**
   ```bash
   git clone https://github.com/your-username/github-r2-cdn.git
   cd github-r2-cdn
   ```

2. **Test the Workflow**
   - Add test images to different folder structures
   - Set up R2 secrets in your fork
   - Verify optimization and sync works

3. **Make Your Changes**
   - Follow the existing code style
   - Add comments for complex logic
   - Test with various image types and sizes

## 🎯 Areas We'd Love Help With

### High Priority
- [ ] **AVIF Support** - Next-gen image format after WebP
- [ ] **Progressive JPEG** - Better loading experience
- [ ] **SVG Minification** - Optimize vector graphics
- [ ] **Multiple R2 Buckets** - Support for staging/production splits

### Medium Priority  
- [ ] **Smart Resizing** - Generate multiple sizes (thumbnails, etc.)
- [ ] **Alt Text Generation** - AI-powered accessibility improvements
- [ ] **Image Analysis** - Detect and report optimization opportunities
- [ ] **Workflow Templates** - Pre-configured setups for popular frameworks

### Nice to Have
- [ ] **Other CDN Support** - AWS CloudFront, Bunny CDN, etc.
- [ ] **Rollback Mechanism** - Revert optimizations if needed
- [ ] **Analytics Integration** - Track performance improvements
- [ ] **Custom Watermarking** - Brand protection features

## 📝 Contribution Guidelines

### Code Style
- Use clear, descriptive variable names
- Add comments for non-obvious logic
- Follow existing shell script patterns
- Include error handling for external commands

### Commit Messages
Follow [Conventional Commits](https://conventionalcommits.org/):

```
feat: add AVIF format support
fix: handle spaces in filenames properly  
docs: update README with new configuration options
perf: optimize image processing for large files
```

### Testing Your Changes

1. **Local Testing**
   ```bash
   # Test image optimization locally
   docker run --rm -v $(pwd):/workspace -w /workspace ubuntu:latest bash
   apt-get update && apt-get install -y imagemagick webp
   # Run optimization commands manually
   ```

2. **Integration Testing**
   - Create test images of various types and sizes
   - Test both `images/` and `images-webp/` folder structures
   - Verify manifest generation
   - Test R2 sync (use a test bucket)

### Pull Request Process

1. **Branch Naming**
   ```bash
   git checkout -b feature/avif-support
   git checkout -b fix/filename-spaces
   git checkout -b docs/setup-guide
   ```

2. **PR Description Template**
   ```markdown
   ## 🎯 What This PR Does
   Brief description of changes

   ## 🧪 Testing Done
   - [ ] Tested with PNG/JPEG/WebP images
   - [ ] Verified manifest generation
   - [ ] Tested R2 sync functionality
   - [ ] Updated documentation

   ## 📸 Screenshots/Examples
   Before/after comparisons if applicable

   ## 🔄 Breaking Changes
   List any breaking changes and migration steps

   ## 📋 Checklist
   - [ ] Code follows project style guidelines
   - [ ] Self-review completed
   - [ ] Documentation updated
   - [ ] Tests pass locally
   ```

3. **Review Process**
   - All PRs need at least one review
   - Maintainers will test with real R2 setups
   - We aim to review within 48 hours

## 🛠️ Development Setup

### Required Tools
```bash
# For local testing
sudo apt-get install imagemagick webp jq

# For manifest validation
npm install -g ajv-cli

# For YAML validation  
pip install yamllint
```

### Environment Variables for Testing
```bash
export MAX_WIDTH=1920
export MAX_HEIGHT=1080
export JPEG_QUALITY=85
export WEBP_QUALITY=85
export BASE_URL="https://test-cdn.example.com"
```

### Test Image Sets
Create these test scenarios:
```
test-images/
├── large/          # >5MB images
├── small/          # <100KB images  
├── mixed-formats/  # PNG, JPEG, GIF, SVG
├── edge-cases/     # Spaces in names, special chars
└── already-optimized/ # Pre-compressed images
```

## 🐛 Bug Reports

### Good Bug Report Template
```markdown
**Description**
Clear description of the issue

**Reproduction Steps**
1. Step one
2. Step two  
3. Step three

**Expected vs Actual**
- Expected: Images should be optimized
- Actual: Workflow fails with error X

**Environment**
- Repository: [link]
- Workflow run: [link to GitHub Actions]
- Image types: PNG, JPEG, etc.
- File sizes: Small/Large

**Logs**
```
Paste relevant workflow logs here
```

**Additional Context**
Screenshots, error messages, etc.
```

### Priority Labels
- 🔥 **Critical**: Workflow completely broken
- ⚠️ **High**: Major functionality affected  
- 📝 **Medium**: Minor issues, edge cases
- 💡 **Low**: Enhancements, nice-to-haves

## 🎨 Feature Requests

### Good Feature Request Template
```markdown
**Problem Statement**
What problem does this solve?

**Proposed Solution**
How would you implement this?

**Alternatives Considered**
Other ways to solve this problem

**Implementation Notes**
Technical details, potential challenges

**Use Cases**
Who would benefit and how?
```

### Feature Evaluation Criteria
- **Impact**: How many users benefit?
- **Complexity**: Implementation difficulty
- **Maintenance**: Ongoing support needs
- **Performance**: Effect on workflow speed
- **Compatibility**: Works with existing setups

## 🌟 Recognition

### Contributors Wall
All contributors get:
- ✨ Listed in README contributors section
- 🏆 GitHub contributor badge
- 📢 Shoutout in release notes
- ⭐ Priority support for issues

### Special Recognition
- 🥇 **Top Contributors**: Multiple significant PRs
- 🏅 **Bug Hunters**: Find and fix critical issues  
- 📖 **Documentation Heroes**: Improve guides and examples
- 🚀 **Performance Optimizers**: Speed improvements

## 📜 Code of Conduct

We follow the [Contributor Covenant](https://www.contributor-covenant.org/). In summary:

- ✅ Be respectful and inclusive
- ✅ Focus on constructive feedback
- ✅ Help newcomers learn and contribute
- ❌ No harassment, discrimination, or trolling
- ❌ No spam or self-promotion

### Enforcement
Violations can be reported to the maintainers. We'll investigate and take appropriate action, including warnings or bans for serious violations.

---

## 🎉 Thank You!

Every contribution makes this project better for the entire community. Whether you're fixing a typo, adding a feature, or helping with documentation - you're making a difference!

**Let's build the best asset delivery platform together! 🚀**