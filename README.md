# TickTrack: Professional Countdown Timer

<!-- Logo Placeholder -->
<!-- <p align="center">
  <img src="path/to/your/logo.svg" alt="TickTrack Logo" width="150"/>
</p> -->

## Badges

![Vue 3](https://img.shields.io/badge/Vue.js-3.x-4FC08D?style=for-the-badge&logo=vue.js&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-6.x-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-8.x-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-v4-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)

## Introduction

TickTrack is a sleek, minimalist Chrome Extension designed to help users effortlessly keep track of important deadlines and events. It provides a clean and intuitive interface for managing multiple countdown timers, ensuring you never miss a beat. With its focus on user experience and efficiency, TickTrack solves the problem of scattered reminders by centralizing all your crucial countdowns in one easily accessible popup.

## Key Features

-   **Real-time Countdown Updates**: All timers update automatically every second, providing precise time remaining until your events.
-   **Automatic Chronological Sorting**: Timers are intelligently sorted, always displaying the closest upcoming event at the top of your list, ensuring you prioritize what matters most.
-   **Collapsible 'Add Timer' Form**: A space-saving accordion-style form allows for adding new timers, expanding only when needed to maintain a compact UI.
-   **Custom Color Coding**: Organize and visually categorize your timers with 6 distinct pastel color schemes. Each scheme features dynamic text contrast for optimal readability and accessibility.
-   **Persistent Storage**: All your countdown timers are securely saved and synced using the Chrome Storage API, with a fallback to `window.localStorage` for broader compatibility during development.

## Tech Stack

-   **Frontend Framework**: [Vue 3](https://vuejs.org/) (Composition API)
-   **Styling Framework**: [Tailwind CSS v4](https://tailwindcss.com/blog/tailwindcss-v4-alpha) (Utility-first CSS framework)
-   **Build Tool**: [Vite](https://vitejs.dev/) with [@crxjs/vite-plugin](https://crxjs.dev/) for Chrome Extension development
-   **Language**: [TypeScript](https://www.typescriptlang.org/) (for type-safe development)

## Installation (for Developers)

To set up and run TickTrack locally, follow these steps:

1.  **Clone the repository**:
    ```bash
    git clone https://github.com/Ali-Allouch/ticktrack.git
    cd ticktrack
    ```

2.  **Install dependencies and build the project**:
    ```bash
    npm install
    npm run build
    ```
    This will generate a `dist` folder in your project directory.

3.  **Load into Chrome as an unpacked extension**:
    a.  Open Chrome and navigate to `chrome://extensions`.
    b.  Enable "Developer mode" by toggling the switch in the top right corner.
    c.  Click on "Load unpacked" and select the `dist` folder generated in the previous step.

    Your TickTrack extension should now appear in your Chrome extensions list and toolbar.

## Project Structure

```
.gitignore
index.html
manifest.json
package-lock.json
package.json
README.md
tsconfig.app.json
tsconfig.json
tsconfig.node.json
vite.config.ts
public/
└── favicon/
src/
├── App.vue
├── main.ts
└── style.css
```
