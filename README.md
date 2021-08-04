## Why [Tailwindcss](https://tailwindcss.com/)
- A Utility first CSS framework.
  - No need to create your own classes.
    - You don't have to name it.
    - No need to fine-tune your style.
  - Easy to support responsive and dark mode.
  - Good performance.
    - Unused classes can be purge in the production environment.
Reference [Tailwindcss course #1](https://www.youtube.com/watch?v=5TymbaeyV-0&t=630s)

## Differences from [bootstrap](https://getbootstrap.jp/)
1. High flexibility.
    - However, the amount of text is less in bootstrap than in tailwindcss.
2. Small file size.
    - Using Purge-css will increase performance.
      - example Tailwindcss under 10KB, bootstrap about 230KB
3. No JavaScript dependencies.
    - Works well with javascript frameworks
      - bootstrap's javascript is hidden and conflicts with javascript frameworks, making it difficult to use.
    - If you need Javascript, use [headlessUI](https://headlessui.dev/).
Reference [Tailwindcss course #2](https://www.youtube.com/watch?v=T_WIbcvB5GM)

## How to set up Tailwindcss
- [Use with frameworks](https://tailwindcss.com/docs/installation#integration-guides)
- [installing Tailwind CSS as a PostCSS plugin](https://tailwindcss.com/docs/installation#installing-tailwind-css-as-a-post-css-plugin)
    - What is [postcss](https://en.wikipedia.org/wiki/PostCSS)?
  - Example
    - `cd desktop`
    - `mkdir your-directory-name`
    - `cd your-directory`
    - `npm -init y`
      - Create the package.json file
      - Node.js is required.
    - `npm install -D tailwindcss@latest postcss@latest autoprefixer@latest`
      - Install Tailwind via npm
    - `npx tailwindcss init -p`
      - Create Tailwindcss config file
      - Create PostCSS config file
  - Include Tailwind in your CSS
    ```css
    /* ./your-css-folder/styles.css */
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
    ```
    - **!** If css validation is working, install the [PostCSS Language Support](https://marketplace.visualstudio.com/items?itemName=csstools.postcss) extension to vscode
  - Create `index.HTML` files
  - Add `<link rel="stylesheet" href="dist.css" />`
    - The built CSS will be used here.
  - Create `dist.css` file
    - [Tailwind CSS IntelliSense](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss)
  - Add Scripts ( package.json )
  ```diff
   "scripts": {
  -  "test": "echo \"Error: no test specified\" && exit 1"
  +  "build": "postcss styles.css -o dist.css"
  },
  ```
    - Meaning: Output ( -o ) the build styles.css file to dist.css
  - `npm uninstall postcss`
  - `npm i -D postcss-cli`
    - Make `npm run build` available
  - After executing the build command, the styling can be checked in the browser.
    - Paste the path of the HTML file into the url field of the browser.
Reference [Tailwindcss course #3](https://www.youtube.com/watch?v=mzRxqknA9Jg)

## Use Purge and Minify to build in production environment
- What is `autoprefixer`?
  - One that automatically adds a vendor prefix.
  - By adding vendorprevix, the same css can be applied to different browsers.
<br/>

- Example: `purge` css that is not used in index.html
  - tailwind.config.js
    - Can be specified as multiple arrays
    - Glob patterns are available
```diff
module.exports = {
+  purge: ["./index.html"],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {
    extend: {},
  },
  plugins: [],
}
```
  - package.json
```diff
"scripts": {
-  "build": "postcss styles.css -o dist.css"
+  "build": "NODE_ENV=production postcss styles.css -o dist.css"
},
```
  - `npm i -D cssnano`
    - Use `minify` to make it even lighter.
  - Add `cssnano: {},` to `postcss.config.js`.

```diff
module.exports = {
plugins: {
  tailwindcss: {},
  autoprefixer: {},
+  cssnano: {},
  },
}
```
  - Building the development environment
    - Add `"dev..."` to `package.json`.
```diff
"scripts": {
+  "dev": " postcss styles.css -o dist.css",
  "build": "NODE_ENV=production postcss styles.css -o dist.css"
},
```

 - Disable minify in the development environment.
  - Functionize `postcss.config.js`.
    - Enables reference to context.

```diff
module.exports = (ctx) => {
  return {
    plugins: {
      tailwindcss: {},
      autoprefixer: {},
      cssnano: ctx.env === "production" ? {} : false,
    },
  };
};
```


Reference [Tailwindcss course #4](https://www.youtube.com/watch?v=2MAJuoJcOw0)