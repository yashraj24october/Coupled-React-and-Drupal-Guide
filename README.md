# Coupled-React-and-Drupal-Guide
Complete Guide to get started with Coupled React and Drupal Web Application.


In order to develop frontend of drupal site using reactjs as coupled system.


<h2>Steps</h2>

<ol>
  <li>Initialize project inside or near your Drupal theme folder using command: <br> <code>npm create vite@latest react-embedded --template react</code></li>
  <li><code>cd react-embedded</code></li>
  <li>run <code>npm install</code></li>
  <li><h4>Now Develop your components in src folder and call in App component.</h4></li>
  <li>Create a root element (div with id root) in your page--front.html.twig or any twig template where you want to render the component</li>
  <li>Now, Build your react app using <code>npm run build</code>, it will create a dist folder in react-embedded folder.<br>In your dist, you will get assets/index-xxxx.js and assets/index-xxxx.css.<br>
Everytime, you will make change in react component/file, you need to run npm run build.<br>
Everytime, a dynamic dist/index-xxxx.js and dist/index-xxxx.css will be generated. <br/>
<h5>But, you need index.js and index.css file, and also dist folder inside custom theme folder instead of react-embedded folder so that you can attach it to library of theme so we get updated component in our drupal twig template.</h5>
<br>
For that, you need to make change in <code>vite.config file</code><br>
so, in vite.config file, you need to add this:<br/>
<code>
  
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig({
  plugins: [react()],
  base: '/themes/custom/crypto/dist/',

  build: {
    outDir: path.resolve(__dirname, '../dist'), 
    emptyOutDir: true,
    rollupOptions: {
      output: {
        entryFileNames: 'index.js',       
        assetFileNames: (chunkInfo) => {
          if (chunkInfo.name && chunkInfo.name.endsWith('.css')) {
            return 'index.css';            
          }
          return '[name][extname]';         // fallback
        },
      },
  },
},
});

</code><br>
<h5>This ensures the output is always index.js and index.css and output dist folder will always be in custom theme folder, so that you can easily attach it to libraries.yml of custom theme.</h5>

</li>
  <li>Now,In your libraries.yml of theme, create a library: react_embedded_library as: <br>
<code>
  react_embedded_library:
  js:
    dist/index.js: { type: file, minified: true }
  css:
    theme:
      dist/index.css: {}

</code>
<br>Then attach this library in theme info file.
</li>
<li>Then <b>Drush cr</b>: to reflect changes.</li>
</ol>
<br/>
<h3>Note: After every change in react component, to reflect change in your drupal site, you need to run <code>npm run build</code> command then <code>Drush cr</code> command.</h3>

<h4>In this way, you can create coupled react and drupal application.</h4>
