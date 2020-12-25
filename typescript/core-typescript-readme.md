# Some important facts

### There are two ways to install TypeScript with npm.
Either use
`npm install --global typescript`
or
`npm install --save-dev typescript`

If you use the `--global` option, then the TypeScript compiler tsc will be available in all projects on the same machine. It will also be available as a terminal command, but it will not be added to your `package.json` file. Therefore, if you share your code with others, TypeScript will not be installed when another person gets your code and runs `npm install`.
If you use `--save-dev`, TypeScript will be added to `package.json` and installed in your project’s `node_modules` folder (current size: 34.2 MB), **but it will not be available directly as a terminal command.**
You can still run it from the terminal as `./node_modules/.bin/tsc`, and you can still use tsc inside the `npm` `"scripts"` section of `package.json`.


### running TypeScript scripts
- If you would like to run a TypeScript file directly from the command prompt, the easiest way is to use `ts-node`:
`npm install --global ts-node`
After installing `ts-node`, run `ts-node X.ts` where `X.ts` is the name of a script you want to run.

- If you want to compile typescript to javascript do
`tsc x.ts` where x is the name of file
to executed js file run
`node x.js`

### Following scripts are created by me
1. "tsc": "tsc",
**usage -**  when typescript is not globally installed use this script to compile ts file
`npm run tsc X.ts` or `yarn tsc X.ts`
2. "ts-node": "ts-node"   
**usage -**  when ts-node is not globally installed use this script to execute ts file
`npm run ts-node X.ts` or `yarn ts-node X.ts`

### Some learnings
- if tsc command is giving compilation error for files in node_modules execute tsc with --lib
- ts-node does not work for RXJS.  