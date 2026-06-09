# Particle Text Animation

Transform your words into mesmerizing 3D particle morphs with this interactive web experience.

---

## Demo / Preview

**Live Demo:** [https://particle-text-animation-plum.vercel.app](https://particle-text-animation-plum.vercel.app)

Type any text (up to 20 characters) and watch 12,000 particles dynamically morph into your words, then smoothly transform back into a rotating 3D sphere. Perfect for creative portfolios, landing pages, and interactive presentations.

---

## About the Project

Particle Text Animation is a cutting-edge interactive visualization that combines **3D graphics**, **particle physics**, and **smooth animations** to create a stunning text morphing effect. 

**What problem does it solve?**
- Traditional text displays are static and boring. This project brings text to life by morphing particles in 3D space, creating an unforgettable visual experience.
- It's a creative tool for developers and designers who want to add wow-factor to their web projects without complex third-party services.
- Perfect for **hero sections**, **about pages**, **portfolio showcases**, or any interaction that demands attention.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **3D Graphics** | Three.js (r128) |
| **Animation Engine** | GSAP (Greensock Animation Platform) v3.7.1 |
| **Frontend** | Vanilla JavaScript (ES6+) |
| **Styling** | CSS3 (Flexbox, Gradients, Backdrop Filters) |
| **Markup** | HTML5 |
| **Fonts** | Google Fonts (Inter: 400, 500, 600, 700 weights) |
| **Deployment** | Vercel |

---

## Features

- Real-time Particle Morphing: Converts user input text into 12,000-point particle formations in 2 seconds
- Dynamic 3D Sphere: Auto-rotating particle sphere with color-coded depth layering using HSL color blending
- Smooth Transitions: GSAP-powered easing animations (power2.inOut) for silky-smooth state changes
- Interactive Input: Type and press Enter or click "Create" button to trigger morphing
- Responsive Design: Full-screen canvas with adaptive window resizing
- Glass Morphism UI: Frosted glass effect on input and controls with backdrop blur
- Additive Blending: Particle rendering with additive blending for glowing, dimensional effect
- Canvas-based Text Rasterization: Converts typography to particle coordinates using canvas pixel data
- Auto-cycle Animation: Text display loops back to sphere after 4 seconds
- Mobile Responsive: Button text hides on small screens; input stretches to fill available space

---

## Project Structure

```
Particle-text-Animation/
├── index.html              # HTML structure with Three.js & GSAP CDN links
├── script.js               # Core animation logic (256 lines)
├── style.css               # Styling with responsive breakpoints (248 lines)
└── README.md               # This file
```

### File Breakdown

| File | Purpose | Key Functions |
|------|---------|---|
| **index.html** | Entry point; sets up canvas container and input UI | Loads CDN resources; defines `#container`, `#morphText` input, `#typeBtn` button |
| **script.js** | Animation engine & particle system | `init()`, `createParticles()`, `createTextPoints()`, `morphToText()`, `morphToCircle()`, `animate()`, `setupEventListeners()` |
| **style.css** | Visual design & layout | `.input-container`, `.controls`, `.button-content`, responsive media queries for mobile |

---

## Getting Started

### Prerequisites

- **Node.js** (optional; only needed if you plan to serve locally with a dev server)
- **Modern web browser** supporting WebGL (Chrome, Firefox, Safari, Edge)
- **Text editor** (VS Code, Sublime, etc.) to edit files

### Installation & Setup

#### Option 1: Direct File Usage (Easiest)

1. **Clone or download** this repository:
   ```bash
   git clone https://github.com/Ares006-007/Particle-text-Animation.git
   cd Particle-text-Animation
   ```

2. **Open `index.html`** directly in your browser:
   - Double-click `index.html` in your file explorer, OR
   - Drag `index.html` into your browser tab

3. **Start typing!**

#### Option 2: Local Dev Server (Recommended for development)

If you have Node.js installed, use a lightweight HTTP server:

```bash
# Using http-server (install globally if not present)
npm install -g http-server
http-server

# Or using Python 3
python3 -m http.server 8000

# Or using Python 2
python -m SimpleHTTPServer 8000
```

Then open `http://localhost:8000` in your browser.

### How to Run Locally

1. **Navigate** to the project directory:
   ```bash
   cd Particle-text-Animation
   ```

2. **Start your server** (see Option 1 or 2 above)

3. **Access** the application in your browser (usually `http://localhost:8000` or `file:///path/to/index.html`)

4. **Interact**:
   - Type text in the input field (max 20 characters)
   - Press **Enter** or click the **Create** button
   - Watch particles morph to your text
   - After 4 seconds, particles auto-reset to rotating sphere

### Environment Variables

This project does **not require** environment variables. All dependencies are loaded via CDN:
- Three.js: CDN (r128 version)
- GSAP: CDN (v3.7.1)
- Google Fonts: CDN (Inter font family)

---

## How It Works

### Architecture Overview

```
User Input → Text Rasterization → Particle Positioning → GSAP Animation → Renderer Output
```

### Step-by-Step Flow

1. **Initialization** (init()):
   - Creates Three.js scene, camera, WebGL renderer
   - Generates 12,000 particles distributed in a 3D sphere using spherical coordinate math
   - Sets up event listeners for user input

2. **Particle Creation** (createParticles()):
   - Uses **spherical distribution** algorithm:
     - `phi = arccos(-1 + 2*i/count)` (latitude)
     - `theta = sqrt(count*π)*phi` (longitude)
   - Positions particles at `(8*cos(θ)*sin(φ), 8*sin(θ)*sin(φ), 8*cos(φ))`
   - Assigns colors using HSL color space based on particle depth
   - Applies additive blending for glow effect

3. **Text-to-Particles** (createTextPoints()):
   - Renders user text to an off-screen canvas using `<canvas>` API
   - Extracts pixel data from canvas bitmap
   - Identifies white pixels above threshold (128)
   - Samples 30% of text pixels to reduce particle count
   - Converts pixel coordinates to 3D space

4. **Animation** (morphToText()):
   - Uses GSAP to interpolate each particle's position from sphere → text over 2 seconds
   - Particles not needed for text are scattered in a halo around the text
   - After 4 seconds (setTimeout), triggers morphToCircle()

5. **Rendering Loop** (animate()):
   - Continuously calls requestAnimationFrame()
   - In sphere state: rotates particles on Y-axis at 0.002 rad/frame
   - Updates renderer each frame at full screen resolution

6. **Responsiveness**:
   - Window resize listener updates camera aspect ratio and renderer size
   - Responsive CSS ensures input UI scales on mobile

### Key Algorithms

**Spherical Distribution** (Fibonacci sphere packing):
```javascript
const phi = Math.acos(-1 + (2 * i) / count);  // Golden angle distribution
const theta = Math.sqrt(count * Math.PI) * phi;
```

**Text Rasterization**:
```javascript
// Canvas rasterization → pixel extraction → threshold sampling → 3D projection
```

**Color Encoding** (HSL-based depth):
```javascript
const depth = Math.sqrt(x² + y² + z²) / 8;
color.setHSL(0.5 + depth*0.2, 0.7, 0.4 + depth*0.3);
```

---

## Deployment

This project is **pre-configured for Vercel**. The repository links to a live deployment at:

**[https://particle-text-animation-plum.vercel.app](https://particle-text-animation-plum.vercel.app)**

### Deploy Your Own to Vercel

1. **Fork or clone** this repository to your GitHub account
2. **Connect to Vercel**:
   - Go to [vercel.com](https://vercel.com)
   - Click "New Project"
   - Select your GitHub repository
   - Vercel auto-detects it as a static site
   - Click "Deploy"

3. **Custom domain** (optional):
   - In Vercel dashboard, go to "Settings" → "Domains"
   - Add your custom domain

### Alternative Deployments

**GitHub Pages**:
```bash
# Commit and push to main branch
git add .
git commit -m "Deploy to GitHub Pages"
git push origin main

# Enable GitHub Pages in repository settings → Source: main branch
```

**Netlify**:
- Drag and drop the project folder to [netlify.com](https://netlify.com)
- Or connect your GitHub repo and enable auto-deploys

**Traditional Hosting** (Apache, Nginx, etc.):
- Upload all files to your web server's public directory
- Ensure CORS is configured if loading resources from other origins

---

## Contributing

Contributions are welcome! Here's how to get involved:

### Steps to Contribute

1. **Fork** the repository on GitHub
   ```bash
   # Visit: https://github.com/Ares006-007/Particle-text-Animation
   # Click "Fork" in top-right corner
   ```

2. **Clone** your fork locally:
   ```bash
   git clone https://github.com/YOUR_USERNAME/Particle-text-Animation.git
   cd Particle-text-Animation
   ```

3. **Create a feature branch**:
   ```bash
   git checkout -b feature/your-feature-name
   # Examples: feature/add-rotation-controls, feature/particle-trails
   ```

4. **Make your changes** and test locally:
   - Edit files (e.g., script.js, style.css)
   - Test in browser
   - Ensure responsiveness on mobile

5. **Commit** with clear messages:
   ```bash
   git add .
   git commit -m "Add: [brief description of feature]"
   ```

6. **Push** to your fork:
   ```bash
   git push origin feature/your-feature-name
   ```

7. **Create a Pull Request** on the main repository:
   - Go to: https://github.com/Ares006-007/Particle-text-Animation/pulls
   - Click "New Pull Request"
   - Select your feature branch
   - Describe your changes and submit

### Ideas for Contributions

- Add color theme selector (cosmic, neon, sunset, ocean)
- Add particle count slider for performance tuning
- Add keyboard shortcuts (Esc to reset, Space to animate)
- Improve mobile UX (touch events, gesture controls)
- Sync animations to audio input
- Add screenshot/download functionality
- Improve accessibility (ARIA labels, keyboard navigation)
- Add unit tests for animation functions

---

## License

This project is licensed under the **MIT License** (inferred; no LICENSE file present in repository).

You are free to:
- Use, modify, and distribute this project
- Include it in commercial projects
- Sublicense under your own terms

**Attribution** appreciated but not required.

For full MIT license text, see: [MIT License](https://opensource.org/licenses/MIT)

---

## Author

**Ares** (@Ares006-007 on GitHub)

- GitHub: [https://github.com/Ares006-007](https://github.com/Ares006-007)
- Project: [Particle-text-Animation](https://github.com/Ares006-007/Particle-text-Animation)

---

## Quick Links

| Resource | Link |
|----------|------|
| Live Demo | [particle-text-animation-plum.vercel.app](https://particle-text-animation-plum.vercel.app) |
| Repository | [github.com/Ares006-007/Particle-text-Animation](https://github.com/Ares006-007/Particle-text-Animation) |
| Three.js Docs | [threejs.org/docs](https://threejs.org/docs) |
| GSAP Docs | [gsap.com](https://gsap.com) |
| Canvas API | [developer.mozilla.org/canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API) |

---

## Support & Feedback

- Report a bug: [Open an issue](https://github.com/Ares006-007/Particle-text-Animation/issues)
- Suggest a feature: [Start a discussion](https://github.com/Ares006-007/Particle-text-Animation/discussions)
- Like this project? Give it a star on GitHub!

---

Made with love using Three.js and GSAP
