# Challenges and Solutions

## 1. Node.js and npm not found on Self-Hosted Runner
**Challenge:** The workflow failed at the `npm install` step with `npm: command not found`.
**Solution:** Added the `actions/setup-node@v4` step at the start of each job in the workflow to ensure Node.js and npm are available.

---

## 2. Sudo Password Prompt During PM2 Install
**Challenge:** The workflow failed with `sudo: a password is required` when trying to install PM2 globally.
**Solution:** Switched to installing PM2 as a local devDependency and used `npx pm2` in the workflow, eliminating the need for `sudo`.

---

## 3. Redundant npm install and npm ci
**Challenge:** Both `npm install` and `npm ci --production` were being run, which is redundant and can cause issues with devDependencies.
**Solution:** Removed `npm install` and used only `npm ci` for a clean, reproducible install in CI.

---

## 4. Vite or Plugin Not Found During Build
**Challenge:** Build failed with `vite: command not found` or `Cannot find module '@vitejs/plugin-react'`.
**Solution:** Ensured all devDependencies (like Vite and plugins) are installed by running `npm ci` (not `npm ci --production`) before the build. Added missing packages (`@vitejs/plugin-react`, `react`, `react-dom`) to `package.json`.

---

## 5. PostCSS Config Syntax Error
**Challenge:** Build failed with `SyntaxError: Unexpected token 'export'` in `postcss.config.js`.
**Solution:** Changed the config from ESM (`export default`) to CommonJS (`module.exports = { ... }`).

---

## 6. Tailwind CSS Plugin Error
**Challenge:** Build failed with `Cannot find module 'tailwindcss'` or required a new PostCSS plugin.
**Solution:** Installed `tailwindcss` and `autoprefixer` as devDependencies. Updated `postcss.config.js` to use the correct plugin syntax.

---

## 7. Out of Memory (OOM) Kills on Self-Hosted Runner
**Challenge:** The runner process was killed due to insufficient memory (`oom-kill`).
**Solution:** Increased the server's RAM and/or added swap space to prevent OOM kills during builds.

---

## 8. Test Failures Due to Missing Static Files
**Challenge:** Tests failed because the server could not find `public/index.html`.
**Solution:** Ensured the Vite build runs before tests, or that a static `index.html` exists in the correct directory. 