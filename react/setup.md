### setup react with typescript and with scss module
- using create react app's `--template typescript` flag create the app
- create react app setups up webpack to use css/scss module out of the box
- to use scss module install sass npm package
- for typescript to reconize the css/scss module create typings.d.ts file and declare module `declare module '*.module.scss';`
- to import module scss use `import styles from './abc.module.scss'`
- to import global style use `import './abc.module.scss'`


### setup bootstrap
- add npm package  `bootstrap` and its dependency `@popperjs/core`
- either add bootstrap min css or scss in index.ts or in the global css file which is imported in index.ts
- to import as css file use `import 'bootstrap/dist/css/bootstrap.min.css'`
- to import as scss file use `@import '~bootstrap/scss/bootstrap';`