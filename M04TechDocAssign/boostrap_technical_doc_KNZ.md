# Setting Up Bootstrap in a Rails Application

## Loading Bootstrap from a CDN

> **Note**: If you're loading Bootstrap from a CDN, you don’t need to add it to your `Gemfile`.

### Step 1: Add Bootstrap CDN Links for CSS and JavaScript

Open the main view file, usually for a rails app it is in `app/views/layouts/application.html.erb`, then add the following lines within the `<head>` tag:

```html
<!-- Bootstrap CSS -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-whatever-integrity-value" crossorigin="anonymous">

<!-- Bootstrap JavaScript Bundle with Popper -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js" integrity="sha384-whatever-integrity-value" crossorigin="anonymous"></script>
```
### Step 2: Configure the Asset Manifest
In `app/assets/config/manifest.js`, add these lines or make sure these lines are included to link the asset directory:
```javascript
//= link_tree ../images
//= link_directory ../stylesheets .css
//= link_directory ../javascript .js
//= link_tree ../../../vendor/javascript .js
```
This will ensure that your images, stylesheets, and JavaScript files undergo proper precompilation.

### Step 3: Set Up Stimulus for JavaScript Controllers
Stimulus is a lightweight JavaScript framework that is part of the Hotwire suite. It allows you to add interactivity to your Rails applications.
1. In `app/assets/javascript/controllers/application.js`, add the following to set up Stimulus:
    ```javascript
    import { Application } from "@hotwired/stimulus";

    const application = Application.start();
    application.debug = false;
    window.Stimulus = application;

    export { application };
    ```
2. Create a simple controller to test if Stimulus works. In `app/assets/javascript/controllers/hello_controller.js`, add:
    ```javascript
    import { Controller } from "@hotwired/stimulus";

    export default class extends Controller {
        connect() {
            this.element.textContent = "Hello World!";
        }
    }
    ```
3. In `app/assets/javascript/controllers/index.js`, register the controllers with Stimulus:
    ```javascript
    import { application } from "controllers/application";
    import HelloController from "./hello_controller";

    application.register("hello", HelloController);
    ```
### Step 4: Configure Importmap
Edit the file in `config/importmap.rb` to load the Turbo and Stimulus libraries automatically:
```ruby
pin "@hotwired/turbo-rails", to: "turbo.min.js"
pin "@hotwired/stimulus", to: "stimulus.min.js"
pin "@hotwired/stimulus-loading", to: "stimulus-loading.js"
pin_all_from "app/javascript/controllers", under: "controllers"
```
### Step 5: Precompile
Ensure all assets are compiled for production correctly, run:
```bash
rails assets:precompile
```
### Integration Complete