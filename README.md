# WP Solution Co., Ltd. Website

Official website for **WP Solution Co., Ltd.**, experts in Industrial Automation & Software Architecture.  
Built with [**Hugo**](https://gohugo.io/) and the [**PaperMod**](https://github.com/adityatelange/hugo-PaperMod) theme.

## 🚀 Features

- **Multi-language Support**: 🇹🇭 Thai (Default) and 🇬🇧 English.
- **Responsive Design**: Modern, clean, and mobile-friendly interface.
- **Dark Mode**: Automatic syntax highlighting and UI adjustment.
- **SEO Optimized**: Built-in SEO features for better visibility.
- **Fast Performance**: Static site generation ensures high speed and security.

## 🛠️ Tech Stack

- **Generator**: [Hugo](https://gohugo.io/) (Extended version recommended)
- **Theme**: [PaperMod](https://github.com/adityatelange/hugo-PaperMod)
- **Deployment**: GitHub Pages

## 📋 Prerequisites

Ensure you have **Hugo** installed on your machine.

- **Windows** (via Chocolatey):
  ```powershell
  choco install hugo-extended -confirm
  ```
- **macOS** (via Homebrew):
  ```bash
  brew install hugo
  ```
- **Linux** (via Snap):
  ```bash
  snap install hugo
  ```

Check installation:
```bash
hugo version
```

## 💻 Installation & Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/wisit/wp-solution-web.git
   cd wp-solution-web
   ```

2. **Run Development Server**
   Start the local server with drafts enabled:
   ```bash
   hugo server -D
   ```
   Navigate to `http://localhost:1313/` to see the site.

## 📂 Project Structure

```plaintext
wp-solution-web/
├── content/           # Website content (Markdown files)
│   ├── th/            # Thai content
│   └── en/            # English content
├── static/            # Static assets (images, css, js)
├── themes/            # Hugo themes (PaperMod)
├── hugo.toml          # Main configuration file
└── .github/           # GitHub Actions workflows
```

## 🚀 Deployment

This project is configured to deploy automatically to **GitHub Pages** via GitHub Actions.
Any push to the `main` branch will trigger a build and deployment.

## 📞 Contact

- **Email**: [wisit.paewkratok@gmail.com](mailto:wisit.paewkratok@gmail.com)
- **Line**: [wisit.p](https://line.me/ti/p/~wisit.p)
- **GitHub**: [wpsolution](https://github.com/wpsolution)

---
© WP Solution Co., Ltd.
