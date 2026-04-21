# TickTrack: Professional Countdown Timer

![Version](https://img.shields.io/badge/version-1.0.1-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Vue.js](https://img.shields.io/badge/Vue.js-3.x-4fc08d?logo=vue.js)
![TypeScript](https://img.shields.io/badge/TypeScript-6.x-3178c6?logo=typescript)
![Vite](https://img.shields.io/badge/Vite-8.x-646CFF?logo=vite)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-v4-06B6D4?logo=tailwindcss)

## 📖 Overview

TickTrack is a sleek, minimalist Chrome Extension designed to help users effortlessly keep track of important deadlines and events. It provides a clean and intuitive interface for managing multiple countdown timers, ensuring you never miss a beat. With its focus on user experience and efficiency, TickTrack solves the problem of scattered reminders by centralizing all your crucial countdowns in one easily accessible popup.

---

## ✨ Key Features

-   **Real-time Countdown Updates**: All timers update automatically every second, providing precise time remaining until your events.
-   **Automatic Chronological Sorting**: Timers are intelligently sorted, always displaying the closest upcoming event at the top of your list, ensuring you prioritize what matters most.
-   **Collapsible 'Add Timer' Form**: A space-saving accordion-style form allows for adding new timers, expanding only when needed to maintain a compact UI.
-   **Custom Color Coding**: Organize and visually categorize your timers with 6 distinct pastel color schemes. Each scheme features dynamic text contrast for optimal readability and accessibility.
-   **Persistent Storage**: All your countdown timers are securely saved and synced using the Chrome Storage API, with a fallback to `window.localStorage` for broader compatibility during development.

---

## 🛠️ Tech Stack

-   **Frontend Framework**: [Vue 3](https://vuejs.org/) (Composition API)
-   **Styling Framework**: [Tailwind CSS v4](https://tailwindcss.com/blog/tailwindcss-v4-alpha) (Utility-first CSS framework)
-   **Build Tool**: [Vite](https://vitejs.dev/) with [@crxjs/vite-plugin](https://crxjs.dev/) for Chrome Extension development
-   **Language**: [TypeScript](https://www.typescriptlang.org/) (for type-safe development)

---

## 🚀 Installation (for Developers)

To set up and run TickTrack locally, follow these steps:

1.  **Clone the repository**:
    ```bash
    git clone https://github.com/Ali-Allouch/tick-track.git
    cd tick-track
    ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Build the extension:**
   ```bash
   npm run build
   ```
    This will generate a `dist` folder in your project directory.

4.  **Load into Chrome as an unpacked extension**:
    -  Open Chrome and navigate to `chrome://extensions`.
    -  Enable "Developer mode" by toggling the switch in the top right corner.
    -  Click on "Load unpacked" and select the `dist` folder generated in the previous step.

    Your TickTrack extension should now appear in your Chrome extensions list and toolbar.

---

## 📂 Project Structure

```
.gitignore
index.html
LICENSE
manifest.json
package-lock.json
package.json
PRIVACY.md
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
---

## 📄 Privacy Policy

We value your privacy. This extension does not collect or transmit any user data. Read the full [Privacy Policy](PRIVACY.md) for more details.

---

## ⚖️ License

Distributed under the MIT License. See `LICENSE` for more information.

---
*Developed with focus on scalability and clean code by [**Ali Allouche**](https://aliallouche.me/).*