# Image File Usage Analysis

This document provides a comprehensive overview of where image files are used in the codebase.

## Image Storage Locations

### Static Directory (`/static/`)
The main location for publicly accessible image files:
- `My-Unsplash-ss.png` - Screenshot for My Unsplash project
- `PortfolioSS-sm.png` - Portfolio screenshot (small)
- `PortfolioSS.png` - Portfolio screenshot (full size)
- `Shortly.webp` - Screenshot for Shortly project
- `Strength-hub.png` - Screenshot for Strength Hub project
- `SunnysideSS.png` - Screenshot for Sunnyside project
- `country-quiz.webp` - Screenshot for Country Quiz project
- `cppASCII.png` - ASCII art for C++ project
- `cppLogo.svg` - C++ logo
- `etch-a-sketch.png` - Screenshot for Etch-a-Sketch project
- `expenses-analyzer-ascii.png` - ASCII art for Expenses Analyzer
- `expensesAnalyzer.png` - Screenshot for Expenses Analyzer project
- `image-uploader-screenshot.webp` - Screenshot for Image Uploader project
- `ip-tracker.webp` - Screenshot for IP Tracker project
- `mojephoto.jpg` - Personal photo
- `mojephotoASCII.png` - ASCII version of personal photo
- `phonebook.png` - Screenshot for Phonebook project
- `phonebookASCII.png` - ASCII art for Phonebook project
- `portfolioss1600x900.png` - Portfolio screenshot (specific size)

### Source Directory (`/src/images/`)
- `mojePhotoSquare.jpg` - Square version of personal photo (used as site icon)

## Image Usage Patterns

### 1. Gatsby Configuration (`gatsby-config.js`)
```javascript
// Site metadata
image: "/PortfolioSS-sm.png" // Default social media image

// PWA manifest icon
icon: `src/images/mojePhotoSquare.jpg`
```

### 2. React Components

#### PopupTerminalWindow Component (`src/components/PopupTerminalWindow.js`)
- **Props**: `popupImageSrc`, `popupImageAlt`
- **Usage**: 
  - Background image: `style={{ backgroundImage: url(${popupImageSrc}) }}`
  - Conditional image display based on URL pattern:
    ```javascript
    src={`${
      /^https/.test(popupImageSrc)
        ? popupImageSrc
        : "/" + popupImageSrc
    }`}
    ```
- **Logic**: Supports both external URLs (https://) and local static files (prefixed with "/")

#### ItemsList Component (`src/components/ItemsList.js`)
- **GraphQL Query**: Fetches `popupImageSrc` from markdown frontmatter
- **Usage**: Passes image data to popup windows

#### Page Template (`src/templates/pageTemplate.js`)
- **GraphQL Query**: Fetches `popupImageSrc` and `popupImageAlt` from markdown
- **Usage**: Passes image props to PopupTerminalWindow component

### 3. SEO Component (`src/components/seo.js`)
- **Props**: `image` (optional)
- **Usage**: 
  - Social media meta tags (Open Graph, Twitter)
  - Falls back to default image from site metadata
  - Full URL construction: `${siteUrl}${image || defaultImage}`

### 4. Markdown Frontmatter
All project markdown files in `src/markdown-pages/projects/` contain:
```yaml
popupImageSrc: "filename.png" # Local static file
# OR
popupImageSrc: "https://..." # External URL
popupImageAlt: "Description"
```

#### Local Image References:
- `etch-a-sketch.md` → `etch-a-sketch.png`
- `shortly.md` → `Shortly.webp`
- `ip-tracker.md` → `ip-tracker.webp`
- `expensesAnalyzer.md` → `expensesAnalyzer.png`
- `myUnsplash.md` → `My-Unsplash-ss.png`
- `country-quiz.md` → `country-quiz.webp`
- `sunnyside.md` → `SunnysideSS.png`

#### External Image References:
- `arkanoid.md` → GitHub raw URL
- `arduino-knight-rider.md` → GitHub raw URL
- `spottedify.md` → GitHub raw URL
- `insurance-company-database.md` → GitHub raw URL
- `perceptron.md` → GitHub raw URL
- `anyGrabber.md` → GitHub raw URL

#### Info Page:
- `src/markdown-pages/info/about.md` → `mojephoto.jpg`

## Video File Usage

### Static Directory Video Files:
- `IP-tracker-view.mp4` - Demo video for IP Tracker
- `IP-tracker-view.webm` - Demo video (WebM format)
- `myUnsplash.mp4` - Demo video for My Unsplash
- `imageUploaderView.mp4` - Demo video for Image Uploader
- `imageUploaderView.webm` - Demo video (WebM format)

### Video Usage in Markdown:
```yaml
video: "/myUnsplash.mp4"  # myUnsplash.md
video: "/IP-tracker-view.mp4"  # ip-tracker.md
video: "false"  # Most other projects (shows static image instead)
```

### Video Rendering in PopupTerminalWindow:
- Conditional rendering: if `video !== "false"`, shows video player
- Otherwise shows static image
- Video element includes: controls, autoplay, muted, loop, playsinline

## CSS/SCSS Image Usage

### Winbox Styles (`src/styles/winbox.scss`)
- `.svgIcon` class for SVG icon styling
- Filter effects for SVG colorization
- Responsive image styling for `.popupTerminaWindowImage`

## Image Processing Logic

### URL Pattern Detection:
```javascript
/^https/.test(popupImageSrc)
  ? popupImageSrc           // External URL - use as-is
  : "/" + popupImageSrc     // Local file - prefix with "/"
```

### Static File Serving:
- Files in `/static/` are served from site root (`/filename.png`)
- Files in `/src/images/` are processed by Gatsby's image processing pipeline

## Security Considerations
- External images are loaded from GitHub raw URLs
- Local images are served as static assets
- No dynamic image uploads or user-generated content

## Verification Results ✅

**Build Status**: ✅ All images successfully processed and copied to public directory

**Static Files**: ✅ All 24 image/video files copied from `/static/` to `/public/`
- All PNG, JPG, WEBP, SVG files: ✅ Available
- All MP4, WEBM video files: ✅ Available

**Source Images**: ✅ Gatsby image processing working correctly
- `mojePhotoSquare.jpg`: ✅ Processed and used as PWA manifest icon

**Markdown References**: ✅ All image references valid
- Local file references: ✅ All files found in public directory
- External URL references: ✅ All URLs use GitHub raw CDN
- Video file references: ✅ All video files found and accessible

**Configuration**: ✅ All configuration references valid
- Default social media image: ✅ `PortfolioSS-sm.png` found
- PWA manifest icon: ✅ `mojePhotoSquare.jpg` found

## Image Processing Pipeline

1. **Static Files** (`/static/` → `/public/`)
   - Files are copied as-is during Gatsby build
   - Accessible via root path (e.g., `/image.png`)

2. **Source Images** (`/src/images/` → Gatsby processing)
   - Processed by Gatsby's image optimization pipeline
   - Used for PWA manifest icons and optimized delivery

3. **External Images**
   - Loaded directly from remote URLs (primarily GitHub)
   - No local processing or caching

4. **Conditional Path Resolution**
   ```javascript
   /^https/.test(popupImageSrc)
     ? popupImageSrc           // External URL - use as-is
     : "/" + popupImageSrc     // Local file - prefix with "/"
   ```