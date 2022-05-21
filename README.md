# Electron Forge & Create React App Typescript Template

Template for creating Electron apps using two popular frameworks with no extra dependencies:

-   [Create React App (with Typescript)](https://create-react-app.dev/)
-   [Electron Forge (WITHOUT Typescript)](https://www.electronforge.io/)

For a version of this template without Typescript visit [this other repo.](https://github.com/hakantakiri/cra-ef)

## Preface

Electron has become a popular tool for building desktop apps for Windows, macOS and some distributions of Linux. It allows the use of web technologies in the development process, hence, anyone can use their preferred web tools to have a better development experience, React falls in the category of a very preferred web technology.

As of 2022, great frameworks have made and easy entrypoint for new developers to these technologies. Create React App is a popular framework for building React projects offering helpful functionalities as hot reloading or basically non existing configuration or knowledge requirements for webpack or babel. On the other hand, Electron Forge framework achieves similar advantages for Electron.

This template is an attempt to provide a ready to use boilerplate for using both previously mention frameworks without any other dependency.

# How to use

## How to setup

1. Clone or download this repository to your local environment.
2. Install dependencies.

```
npm install
```

Now you are ready to create your app. Be careful if you move the location of the index.js file corresponding to React, or the main.js file corresponding to Electron. Check `package.json` to update their new locations.

## How to run project

1. For this you'll need to use two separate tabs or windows in your terminal. The first one for React and the second one for Electron.

-   1.1. For React:

```
npm start
```

-   1.2. For Electron:

```
npm run electron
```

This can be simplified using another dependencies such as `foreman`, but this template aims to avoid using any other dependency apart from what is already required inside the frameworks.

## How to build

In order to build a production ready version of your React project, and make an installer for your application using Electron Forge, execute the following command:

```
npm run build-all
```

Independent builds can be made using commands specified inside the `package.json` file.

Other npm commands corresponding to the Create React App framework and Electron Forge framework are available, you can checkout those reading the `package.json` file.

# Creating this template

## How to replicate this template from scratch

1. Create an empty project using `create-react-app` with Typescript. Check the [framework's docs](https://create-react-app.dev/docs/adding-typescript).
2. Install Electron as a development dependency.
3. Create a `main.js` inside the `src` folder. This will be our Electron entry point to manage the main electron processes. Copy here the following code and save file:

```js
const { app, BrowserWindow } = require("electron")
const path = require("path")
const url = require("url")

const createWindow = () => {
	// Create the browser window.
	const mainWindow = new BrowserWindow({
		width: 800,
		height: 600,
		webPreferences: {
			nodeIntegration: true,
			contextIsolation: false,
		},
	})

	// and load the index.html of the app.
	const startUrl =
		process.env.WEB_URL ||
		url.format({
			pathname: path.join(__dirname, "../build/index.html"),
			protocol: "file",
			slashes: true,
		})
	mainWindow.loadURL(startUrl)

	// Open the DevTools.
	mainWindow.webContents.openDevTools()
}

app.on("ready", createWindow)

app.on("window-all-closed", () => {
	if (process.platform !== "darwin") {
		app.quit()
	}
})

app.on("activate", () => {
	// On OS X it's common to re-create a window in the app when the
	// dock icon is clicked and there are no other windows open.
	if (BrowserWindow.getAllWindows().length === 0) {
		createWindow()
	}
})
```

4. Install Electron Forge using the "Importing Existing project" guide [here at it's website.](https://www.electronforge.io/import-existing-project)
5. Add the next block of json key-value pairs or replace them if any already exists at `package.json`. Don't change what is set to main and homepage in the following json data, but use your own description, author info and license.

```json
"main": "src/main.js",
"description": "Electron Forge & Create React App Template",
"author": {
    "name": "Your name",
    "email": "your@em.ail"
},
"license": "MIT",
"homepage": "./",
```

6. Replace all `scripts` object inside `package.json` for the following object:

```json
"scripts": {
    "start": "BROWSER=none react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "electron": "WEB_URL=http://localhost:3000 electron .",
    "electron-prd": "electron .",
    "electron-forge": "WEB_URL=http://localhost:3000 electron-forge start",
    "electron-forge-prd": "electron-forge start",
    "package": "electron-forge package",
    "make": "electron-forge make",
    "build-all": "npm run build; npm run make"
}
```

7. Add `/out` to your `.gitignore` file

```
# Electron Forge
/out
```

You now should have the same template.

## References

> Sepulveda, C. (2017). How to build an Electron app using create-react-app. No webpack configuration or “ejecting” necessary.
> https://medium.com/free-code-camp/building-an-electron-application-with-create-react-app-97945861647c

> Electron Forge. (2021). Importing Existing Project. https://www.electronforge.io/import-existing-project
