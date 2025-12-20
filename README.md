# Henry Nguyen - Portfolio Website

Personal portfolio website built with Jekyll and the Minimal Mistakes theme.

🌐 **Live Site:** [henrynvp.github.io](https://henrynvp.github.io)

## About

This repository contains my personal portfolio website showcasing my projects, experience, and skills in AI, Robotics, and Software Engineering.

## Tech Stack

- **Jekyll** - Static site generator
- **Minimal Mistakes Theme** - Jekyll theme for personal sites
- **GitHub Pages** - Hosting platform

## Local Development

To run the site locally:

1. Install dependencies:
   ```bash
   sudo apt-get install ruby-dev build-essential
   export PATH="$HOME/.local/share/gem/ruby/3.2.0/bin:$PATH"
   bundle config set --local path 'vendor/bundle'
   bundle install
   ```

2. Start the Jekyll server:
   ```bash
   bundle exec jekyll serve
   ```

3. Open your browser to `http://localhost:4000`

## Project Structure

```
.
├── _config.yml          # Jekyll configuration
├── index.md             # Homepage content
├── Gemfile              # Ruby dependencies
├── projects/            # Project documentation
└── README.md           # This file
```

## License

This website is open source and available under the [MIT License](LICENSE).
