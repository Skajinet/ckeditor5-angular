# CKEditor 5 component for Angular 2+

[![Join the chat at https://gitter.im/ckeditor/ckeditor5](https://badges.gitter.im/ckeditor/ckeditor5.svg)](https://gitter.im/ckeditor/ckeditor5?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![npm version](https://badge.fury.io/js/%40ckeditor%2Fckeditor5-angular.svg)](https://www.npmjs.com/package/@ckeditor/ckeditor5-angular)
[![Build Status](https://travis-ci.org/ckeditor/ckeditor5-angular.svg?branch=master)](https://travis-ci.org/ckeditor/ckeditor5-angular)
[![Coverage Status](https://coveralls.io/repos/github/ckeditor/ckeditor5-angular/badge.svg?branch=master)](https://coveralls.io/github/ckeditor/ckeditor5-angular?branch=master)
<br>
[![Dependency Status](https://david-dm.org/ckeditor/ckeditor5-angular/status.svg)](https://david-dm.org/ckeditor/ckeditor5-angular)
[![devDependency Status](https://david-dm.org/ckeditor/ckeditor5-angular/dev-status.svg)](https://david-dm.org/ckeditor/ckeditor5-angular?type=dev)

A simple component that simplifies the usage of [CKEditor 5](https://ckeditor.com/ckeditor-5/) in Angular 2+ projects.

## Usage

1. In your existing Angular project, install the CKEditor component:

   ```bash
   npm install --save @ckeditor/ckeditor5-angular
   ```

2. [Install](https://docs.ckeditor.com/ckeditor5/latest/builds/guides/integration/installation.html) one of available editor builds:
   * [build-classic](https://www.npmjs.com/package/@ckeditor/ckeditor5-build-classic)
   * [build-inline](https://www.npmjs.com/package/@ckeditor/ckeditor5-build-inline)
   * [build-balloon](https://www.npmjs.com/package/@ckeditor/ckeditor5-build-balloon)
   * [build-decoupled-document](https://www.npmjs.com/package/@ckeditor/ckeditor5-build-decoupled-document)

   or [create a custom one](https://docs.ckeditor.com/ckeditor5/latest/builds/guides/development/custom-builds.html)

   **Note:** The Angular integration doesn't work for now with an editor built from source. If you need to integrate a custom editor, please, follow these [steps](https://docs.ckeditor.com/ckeditor5/latest/builds/guides/development/custom-builds.html) and then import the `ckeditor.js` file when building an angular component.

   Let's pick the `ckeditor5-build-classic`:

   ```bash
   npm install --save @ckeditor/ckeditor5-build-classic
   ```

   **Note:** You may need to allow external JS in your project's `tsconfig.json` for the builds to work properly:

   ```json
   "compilerOptions": {
   	"allowJs": true
   }
   ```

3. Include the CKEditor module:

   ```ts
   import { CKEditorModule } from '@ckeditor/ckeditor5-angular';

   @NgModule( {
   	imports: [
   		...
   		CKEditorModule,
   		...
   	],
   	...
   } )
   ```

4. Import the editor build in your Angular component and assign it to a `public` property so it becomes accessible in the template:

   ```ts
   import * as ClassicEditorBuild from '@ckeditor/ckeditor5-build-classic';

   @Component( {
   	...
   } )
   export class MyComponent {
   	public Editor = ClassicEditorBuild;
   	...
   }

   ```

	You can import as many editor builds as you want.

5. Use the `<ckeditor>` tag in the template to run the editor

   ```html
   <ckeditor [editor]="Editor" data="<p>Hello world!</p>"></ckeditor>
   ```

## Integration with the `ngModel`

The component implements the [`ControlValueAccessor`](https://angular.io/api/forms/ControlValueAccessor) interface and works with the `ngModel`.

1. Create some model in your component to share with the editor:

   ```ts
   @Component( {
   	...
   } )
   export class MyComponent {
   	public model = {
   		editorData: '<p>Hello world!</p>'
   	};
   	...
   }
   ```

2. Use the model in the template to enable a 2–way data binding:

   ```html
   <ckeditor [(ngModel)]="model.editorData" [editor]="Editor"></ckeditor>
   ```

## Supported `@Inputs`

### `editor` (required)

The [Editor](https://docs.ckeditor.com/ckeditor5/latest/builds/guides/integration/basic-api.html) which provides the static [`create()`](https://docs.ckeditor.com/ckeditor5/latest/api/module_core_editor_editor-Editor.html#static-function-create) method to create an instance of the editor:

```html
<ckeditor [editor]="Editor"></ckeditor>
```

### `config`

The [configuration](https://docs.ckeditor.com/ckeditor5/latest/api/module_core_editor_editorconfig-EditorConfig.html) of the editor:

```html
<ckeditor [config]="{ toolbar: [ 'heading', '|', 'bold', 'italic' ] }" ...></ckeditor>
```

### `data`

The initial data of the editor. It can be a static value:

```html
<ckeditor data="<p>Hello world!</p>" ...></ckeditor>
```

or a shared parent component's property

```ts
@Component( {
	...
} )
export class MyComponent {
	public editorData = '<p>Hello world!</p>';
	...
}
```

```html
<ckeditor [data]="editorData" ...></ckeditor>
```

### `disabled`

Controls the editor's [read–only](https://docs.ckeditor.com/ckeditor5/latest/api/module_core_editor_editor-Editor.html#member-isReadOnly) state:

```ts
@Component( {
	...
} )
export class MyComponent {
	public isDisabled = false;
	...
	toggleDisabled() {
		this.isDisabled = !this.isDisabled
	}
}
```

```html
<ckeditor [disabled]="isDisabled" ...></ckeditor>

<button (click)="toggleDisabled()">
	{{ isDisabled ? 'Enable editor' : 'Disable editor' }}
</button>
```

## Supported `@Outputs`

### `ready`

Fires when the editor is ready. It corresponds with the [`editor#ready`](https://docs.ckeditor.com/ckeditor5/latest/api/module_core_editor_editor-Editor.html#event-ready)
event. Fires with the editor instance.

### `change`

Fires when the content of the editor has changed. It corresponds with the [`editor.model.document#change`](https://docs.ckeditor.com/ckeditor5/latest/api/module_engine_model_document-Document.html#event-change) event.
Fires with an object containing the editor and the CKEditor5 change event.

### `blur`

Fires when the editing view of the editor is blurred. It corresponds with the [`editor.editing.view.document#blur`](https://docs.ckeditor.com/ckeditor5/latest/api/module_engine_view_document-Document.html#event-event:blur) event.
Fires with an object containing the editor and the CKEditor5 blur event.

### `focus`

Fires when the editing view of the editor is focused. It corresponds with the [`editor.editing.view.document#focus`](https://docs.ckeditor.com/ckeditor5/latest/api/module_engine_view_document-Document.html#event-event:focus) event.
Fires with an object containing the editor and the CKEditor5 focus event.

## 🚧 Development 🚧

Having cloned the repository, to install necessary dependencies, run:

```bash
npm install
```

### The structure of the repository

This repository contains the following code:
* `./src/ckeditor` contains the CKEditor component,
* `./src/app` a demo application using the component.

**Note:** The [npm package](https://www.npmjs.com/package/@ckeditor/ckeditor5-angular) contains a [packaged component](#packaging-the-component) only.

### Testing the component (demo)

To open a demo application using the component, run:

```bash
npm run start
```

To test it in the production, use:

```bash
npm run start -- --prod
```

To run unit tests, use:

```bash
npm run test
```

To run e2e tests run:

```bash
npm run e2e
```

To run coverage tests run:

```bash
npm run coverage
```

Play with the application and make sure the component works properly.

### Packaging the component

This project uses the [ng-packagr](https://www.npmjs.com/package/ng-packagr) to create a package meeting the Angular Package Format specification. Calling

```bash
npm run build-package
```

creates a package in the `./dist` directory, which can be then published in npm.

#### Publishing the package

To publish the new package in the npm registry, run:

```bash
npm run publish
```

### Testing a package before releasing

Having generated a package, create a symlink to the `ckeditor5-angular/dist` package directory to test it in another (3rd–party) Angular project:

```bash
ln -s /path/to/ckeditor5-angular/dist node_modules/\@ckeditor/ckeditor5-angular
```

You may also need the following config in `angular.json` to include the symlinked component package without errors:

```json
{
	"project-name": {
		"architect": {
			"build": {
				"options": {
					"preserveSymlinks": true
				}
			}
		}
	}
}
```

## License

Licensed under the terms of [GNU General Public License Version 2 or later](http://www.gnu.org/licenses/gpl.html). For full details about the license, please check the LICENSE.md file.
