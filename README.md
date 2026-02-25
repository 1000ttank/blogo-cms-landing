# Blogo CMS Landing Page

Minimal, technical landing page for [Blogo CMS](https://github.com/1000ttank/Hexo-NX-CMS) built with [Hugo](https://gohugo.io) and [Hextra](https://github.com/imfing/hextra) theme.

## Quick Start

### Prerequisites

- [Hugo (extended)](https://gohugo.io/installation/)
- [Git](https://git-scm.com/)

### Local Development

```bash
# Clone with submodules
git clone --recursive https://github.com/1000ttank/blogo-cms-landing.git
cd blogo-cms-landing

# Or if already cloned, init submodules
git submodule update --init

# Start dev server
hugo server --disableFastRender -p 1313
```

Open `http://localhost:1313`.

### Build for Production

```bash
hugo --gc --minify
```

Output is in `public/`.

## Deployment

### GitHub Pages

1. Push to a GitHub repository
2. Enable **GitHub Pages** in Settings → Pages → Source: **GitHub Actions**
3. The workflow in `.github/workflows/pages.yaml` builds and deploys on push to `main`

### Self-host

Build the site and serve the `public/` directory with any static file server (nginx, Caddy, etc.).

### Docker

```bash
# Build
docker build -t blogo-landing .

# Run (serves on port 8080)
docker run -p 8080:8080 blogo-landing
```

(Add a `Dockerfile` if Docker support is needed.)

## License

MIT © 2026 1000ttank
