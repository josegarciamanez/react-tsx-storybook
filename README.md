This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Instalation

### `create-react-app hello-tsx-storybook --typescript`

### `yarn add --dev @storybook/react @types/storybook__react`

### `yarn add --dev babel-loader @babel/core`

### `yarn add --dev awesome-typescript-loader react-docgen-typescript-loader react-docgen-typescript-webpack-plugin`

### `mkdir .storybook`

### `cd .storybook`

`touch addons.js config.js tsconfig.json webpack.config.js`

For a basic Storybook configuration, the only thing you need to do is tell Storybook where to find stories and include the path inside .storybook/config.js. In our current scenario, we are going to stories inside the src/components directory (just a personal preference).

`import { configure } from "@storybook/react" const req = require.context("../src/components", true, /.stories.tsx$/) function loadStories() { req.keys().forEach(req) } configure(loadStories, module)`
`yarn add node-sass`

Note, that, prior to this step, components directory inside src did not exist. You will have to create that manually. The above snippet of code is to create a pattern such that all the stories match a particular glob. code is Next file that needs to be configured is tsconfig.json inside the storybook directory. This file is going to be responsible to compile stories from TypeScript to JavaScript. Add the following to this file.

### `{

    "compilerOptions": {
    	"baseUrl": "./",
    	"allowSyntheticDefaultImports": true,
    	"module": "es2015",
    	"target": "es5",
    	"lib": ["es6", "dom"],
    	"sourceMap": true,
    	"allowJs": false,
    	"jsx": "react",
    	"moduleResolution": "node",
    	"rootDir": "../",
    	"outDir": "dist",
    	"noImplicitReturns": true,
    	"noImplicitThis": true,
    	"noImplicitAny": true,
    	"strictNullChecks": true,
    	"declaration": true
    },
    "include": ["src/**/*"],
    "exclude": [
    	"node_modules",
    	"build",
    	"dist",
    	"scripts",
    	"acceptance-tests",
    	"webpack",
    	"jest",
    	"src/setupTests.ts",
    	"**/*/*.test.ts",
    	"examples"
    ]

}`

Lastly, add the following code inside the file .storybook/webpack.config.js.

### `module.exports = ({ config }) => {

    config.module.rules.push({
    	test: /\.(ts|tsx)$/,
    	use: [
    		{
    			loader: require.resolve("awesome-typescript-loader"),
    			options: {
    				presets: [["react-app", { flow: false, typescript: true }]],
    				configFileName: "./.storybook/tsconfig.json"
    			}
    		},
    		{
    			loader: require.resolve("react-docgen-typescript-loader")
    		}
    	]
    })
    config.resolve.extensions.push(".ts", ".tsx")
    return config

}`

To complete the setup, open package.json file and add the script to run the storybook interface.

### `"storybook": "start-storybook -p 4000 -c .storybook"`

### `yarn start`
