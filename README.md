# Photo Gallery & Portfolio

A professional photo gallery and portfolio application built with Next.js 15, TypeScript, and Tailwind CSS. This project is designed for **demoing GitHub Copilot features** in a real-world, component-driven Next.js application. The included demos showcase how Copilot can assist with code generation, refactoring, UI building, and more.

## Demos

- All demo guides and examples are in the [`demos/`](demos/) folder.
- For more information about each demo, refer to the [README](demos/README.md) file in the `demos/` directory.
- To get started, check out the first demo [`features-demo.md`](demos/features-demo.md) for a walkthrough of gallery features and Copilot capabilities.
- Before a workshop, complete the [`WORKSHOP_PREFLIGHT.md`](WORKSHOP_PREFLIGHT.md) checklist.

### Creating a New Demo

If you want to contribute and create a new demo, follow these steps:

1. Open GitHub Copilot Chat.
2. Type the prompt `/create-copilot-demo` with an explanation of your demo idea.
3. Copilot will generate a new demo file in the `demos/` directory.
4. Fill in remaining sections with detailed instructions, examples, and expected results.

After finishing the demo, don't forget this quick follow-up:

1. Add in the overview, key skills, and demo link to the [demo README](demos/README.md)

## Getting Started

### Technical Requirements

- **Node.js 22 LTS**
- **npm** (the repository includes `package-lock.json`)

### Quick Start with GitHub Codespaces

The fastest way to get started is using GitHub Codespaces:

1. Click the **"Code"** button on the GitHub repository page
2. Select the **"Codespaces"** tab
3. Click **"Create codespace on main"** (or your current branch)
4. Wait for the codespace to build and start

The codespace will automatically:
- Install all dependencies (`npm ci`) when the container is created
- Start the development server (`npm run dev`) when the container starts
- Configure GitHub Copilot and essential VS Code extensions
- Forward port 3000 for the Next.js application

Once ready, you can access the application at the forwarded port URL provided in the terminal.

### Local Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/agarciamears/copilot-intermediate-gallery-repo.git
   cd copilot-intermediate-gallery-repo
   ```
2. Install dependencies:
   ```bash
   npm ci
   ```
3. Start the development server:
   ```bash
   npm run dev
   ```
   Open [http://localhost:3000](http://localhost:3000) in your browser.

### Available Commands

```bash
npm run dev    # Development server
npm run lint   # ESLint checks
npm run build  # Production build
npm run start  # Serve a completed production build
```

The baseline intentionally has no test runner or `npm test` script. The
[testing demo](demos/testing-with-copilot-demo.md) adds that infrastructure
during the exercise.

> **Workshop scope:** Gallery data and uploads are simulated in the browser.
> There is no backend, persistence, authentication, or photo API in the
> baseline. Download, share, and photo-detail controls are visual placeholders.

## Project Structure

```bash
src/
├── app/                 # Next.js 15 App Router pages
├── components/          # Reusable React components
├── lib/                 # Utility functions and helpers
demos/                   # Demo guides and templates
```