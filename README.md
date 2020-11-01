# nuxt-ckeditor-example

> Test code to integrate nuxt and ckeditor (rich text editor) with image upload support

## Initial setup
> Official guide to setting up Vue and CKEditor: https://ckeditor.com/docs/ckeditor5/latest/builds/guides/integration/frameworks/vuejs.html
- Followed CKEditor GUI to configure plugins using this guide: https://ckeditor.com/ckeditor-5/online-builder/
  - Copied the unzipped content into this project
  - Navigated into the copied `ckeditor5` folder and ran `npm install` followed by `npm run build`

## Port-over guide
- Copy-paste the `ckeditor5` folder
- Navigate into and and run `npm install` followed by `npm run build`
- Install package `npm install --save @ckeditor/ckeditor5-vue`
- Copy over all code in `pages/index.vue` into a component for the text editor. Address all `TODO` comments.

## Build Setup

```bash
# install dependencies
$ npm install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm run start

# generate static project
$ npm run generate
```
