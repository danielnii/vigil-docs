# Challenge: Tailwind CSS v4 Migration Issues

## Date
2026-03-09

## Problem
After updating Tailwind to v4, the app failed with:
```
Cannot apply unknown utility class `border-obsidian-200`
```

## Root Cause
Tailwind CSS v4 changed how custom colors are processed and removed the need for `postcss.config.js`. Custom color classes weren't being generated.

## Solution
1. Uninstall v4 packages:
   ```bash
   npm uninstall tailwindcss postcss autoprefixer @tailwindcss/postcss
   ```
2. Install stable v3:
   ```bash
   npm install -D tailwindcss@3.4.1 postcss@8.4.35 autoprefixer@10.4.17
   ```
3. Recreate `postcss.config.js`:
   ```js
   export default {
     plugins: {
       tailwindcss: {},
       autoprefixer: {},
     },
   }
   ```
4. Keep `tailwind.config.js` with custom colors:
   ```js
   module.exports = {
     content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
     theme: {
       extend: {
         colors: {
           obsidian: {
             50: '#F5F5F7',
             200: '#D1D1D6',
             900: '#0F0F1A',
           },
           'apple-blue': '#0071E3',
         },
       },
     },
   }
   ```

## Lesson Learned
> Stick with stable versions for MVP. Upgrade to v4 in Phase 2.

## Prevention
Pin versions in `package.json`:
```json
{
  "devDependencies": {
    "tailwindcss": "3.4.1",
    "postcss": "8.4.35",
    "autoprefixer": "10.4.17"
  }
}
```
```