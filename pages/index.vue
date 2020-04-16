<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <v-flex
      xs12
      sm8
      md6
    >
      <ckeditor :editor="editor" v-model="editorData" :config="editorConfig"></ckeditor>
      <v-btn class="btn-submit" @click="submit">Submit To Backend</v-btn>
      <h1 class="content-header">Rich Text Rendered Below (using v-html tag)</h1>
      <div v-html="editorData" class="content"></div>
    </v-flex>
  </v-layout>
</template>

<script>
  import axios from 'axios';

  let ClassicEditor
  let CKEditor

  if (process.client) {
    ClassicEditor = require('@ckeditor/ckeditor5-build-classic')
    CKEditor = require('@ckeditor/ckeditor5-vue')
  } else {
    CKEditor = { component : {template:'<div></div>'}}
  }

  export default {
    components : { 
      ckeditor: CKEditor.component
    },
    data() {
      return {
          editor: ClassicEditor,
          editorData: '<p>Content of the editor.</p>',
          editorConfig: {
            // TODO: Decide on whether we should support image uploads through creating a custom adapter
            // https://ckeditor.com/docs/ckeditor5/latest/framework/guides/deep-dive/upload-adapter.html
            // or adding images as base64 encoded
            // https://ckeditor.com/docs/ckeditor5/latest/features/image-upload/base64-upload-adapter.html
          },
      };
    },
    methods: {
      submit() {
        console.log(this.editorData)
        axios.post('http://localhost:8080/query', {
          query: `mutation CreateProduct {
            createProduct(input: {
              name: "Test Product With Rich Text"
              description: "${this.editorData}"
              category: Helmet
              inventory: [
                {
                  quantity: 1
                  price: 10
                }
              ],
              brand_id: 4
              active: false
            }) {
              description
            }
          }`
        }).then(result => {
          console.log(result)
        }).catch(error => {
          console.log(error)
        })
      }
    }
  }
</script>

<style lang="css">
  .btn-submit {
    margin-top: 20px;
  }
  .content-header {
    margin-top: 50px;
  }
  .content {
    margin-top:30px;
  }
</style>
