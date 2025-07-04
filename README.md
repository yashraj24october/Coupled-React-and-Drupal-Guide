# Coupled-React-and-Drupal-Guide
Complete Guide to get started with Coupled React and Drupal


In order to develop frontend of drupal site using reactjs as coupled system.


<h2>Steps</h2>

<ol>
  <li>Initialize project inside or near your Drupal theme folder using command: <br> <code>npm create vite@latest react-embedded --template react</code></li>
  <li><code>cd react-embedded</code></li>
  <li>run <code>npm install</code></li>
  <li><h4>Now Develop your components in src folder and call in App component.</h4></li>
  <li>Create a root element (div with id root) in your page--front.html.twig or any twig template where you want to render the component</li>
  <li>Now, Build you react app using <code>npm run build</code>, it will create a dist folder in react-embedded folder.<br>In your dist, you will get assets/index-xxxx.js and assets/index-xxxx.css.<br>
Everytime, you will make change in react component/file, you need to run npm run build.<br>
Everytime, a dynamic assets/index-xxxx.js and assets/index-xxxx.css will be called. <br/>
<h5>But, we want index.js and index.css file, so that we can attach it to library of theme so we get updated component in our drupal twig template.</h5>
<br>
For that, we need to make change in <code>vite.config file</code><br>
so, in vite.config file, we need to add this:
<code>
  export default defineConfig({
  build: {
    rollupOptions: {
      output: {
        entryFileNames: 'index.js',
        assetFileNames: 'index.css'
      }
    }
  }
});</code><br>
<h5>This ensures the output is always index.js and index.css</h5>

</li>
  <li>Now, create a react-assets folder in your theme folder. It will be  folder that will contain index.js and index.css file of react-embedded app.
  <li>Now,In your libraries.yml of theme, create a library: react_embedded_library as: <br>
<code>
  react_embedded_library:
  js:
    js/react-assets/index.js: { type: file, minified: true }
  css:
    theme:
      js/react-assets/index.css: {}
  dependencies:
    - core/drupal
</code>
<br>Then attach this library in theme info file.
</li>
<li>Now after getting the index.js and index.css file, we now need to add these updated files in react-assets folder of our theme.</li>
<li>Then <b>Drush cr</b>: to reflect changes</li>
</ol>


<h4>In this way, we can create coupled react and drupal application</h4>
