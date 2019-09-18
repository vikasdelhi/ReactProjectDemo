# Availability built using React

## Coding Requirements

* Avoid using Jquery directly.
	* React has more performant ways to handle DOM manipulation and native support for ES6 operations.

* Avoid using Bootstrap.
  * The style model conflicts with Azure Devops.

* Performance
	* Always consider the impact adding libraries and complex components have.
	* Explore alternatives when a library or component has an impact on performance
	* Use chrome dev tools to measure performance
		* Performance tab > Start Profiling and Reload Page (Ctrl + Shift + E).
		* Compare current build (preferably on dev or production) to your current changes.
		* Test on at least 2 different machines.
		* Before each check-in save a copy of this profile (right click > save profile) and put it into the `Performance History` folder. Name the .json file `year-month-day_PR-pull request id.json`

* Indentation
  	* Use tabs.
	* Don't change indentation style. If you prefer a different indentation use your favorite styling via a personal `.editorconfig` file. (VSCode doesn't support `.editorconfig` directly.  `EditorConfig for VS Code` is a good extension)

* ES Lint
	* ES Lint issues must be addressed before checkins.
	* Allowed Ignore Linting exceptions:
		* `eslint-disable no-underscore-dangle` - only for code/packages we do not control. `eg: AuthContext._user....`
		* `eslint-disable global-require` - avoid if possible, but may be needed for some packages `eg: require(blahBlah...)`
		* `eslint-disable multiline-comment-style` - can conflict with some JSDoc  or other comments.  Use sparringly.
		* `eslint-disable` - only can be used in serviceWorker.js

## Coding Guidelines

There are a few basic things to keep in mind. I am intentionally writing only the important things that people tend to miss.

* Don't include a package unless absolutely needed (things like auth libraries, cosmosdb , etc)
	* If you still need a component from some library, only include that component in your library. `import { component } from 'some-library'` includes the entire library. Instead, use `import { Component } from 'some-library\component'`
	* Always check the bundle size before inclusion (500kB minified is a big package). [BundlePhobia](https://bundlephobia.com/)
	* Pay attention to the popularity and recent support/updates of a package when considering a package. Use [npm trends](https://www.npmtrends.com/) to compare package histories.

* Styles
	* Avoid inline styles where possible.
	* Avoid creating many smaller `.css` files, within reason,  to avoid conflicts.
	* Separate words in multi-word class names with a `-`.

* Data
	* Avoid using Redux or other stores.  Our use case is currently relatively simple.
	* API calls should generally occur in HOC and flow to smaller components via props.

* Browser Compatability
	* Test app on Edge, Chrome, and Edge Chromium before merging into develop.

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode using data from the development server.<br />
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.<br />
You will also see any lint errors in the console.

### `npm run start-local`

Runs the app in development mode using internal static data.

### `npm test`

Launches the test runner in the interactive watch mode.<br />
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.<br />
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.<br />
Your app is ready to be deployed!  Use `serve -s build` to host a local web server.

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run build-dev`
Builds the app for production using data from the development server. Files are located in the `build` folder. Use `serve -s build` to host a local web server.


### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (Webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Pipelines

Development and production builds are automatically staged and deployed to their Azure App Services after changes are made in their branches.<br />
Each App Service includes a staging slot that will be deployed to.  Upon successful deployment, the staging and production slots will be swapped for the development pipeline.

**Always run and view a build locally before merging into develop or master branch to preemptively address any build issues that arise**

### `Development`

[Build](https://) |
[Release](https://)

### `Production`

[Build](https://) |
[Release](https://)


## Overview of Code Flow
Updated: 8/9/19

+ `Tree`
	+ Data must be structured to allow the underlying table to match data to the columns object
		```javascript
		[
		 	{
		 		data: {
		 			'properties that are expected to be in the table columns',
		 		},
		 		childItems: [
					{
						data: {
							'properties that are expected to be in the table columns',
						},
						childItems: [{...}, ...]
							'other child properties'
					},
					...
				],
				'other parent properties',
				...
			},
			...
		]
		```
