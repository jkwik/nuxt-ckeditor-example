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
  axios.withCredentials = true;

  let ClassicEditor
  let CKEditor

  if (process.client) {
    ClassicEditor = require('~/ckeditor5/build/ckeditor')
    CKEditor = require('@ckeditor/ckeditor5-vue')
  } else {
    CKEditor = { component : {template:'<div></div>'}}
  }

  // TODO: Remove below authentication, this is for testing purposes
  axios.post('https://apd-be-prod.smrxbtsdpc.us-west-1.elasticbeanstalk.com/query', {
    query: `mutation LoginAdmin {
      loginUser(input: {
        email: "kwik@wisc.edu"
        password: "fakepassword"
      }) {
        id
        first_name
        last_name
        email
        role
        access_token
      }
    }`
  }, {
    withCredentials: true,
  }).then(result => {
    console.log(result.data)
  }).catch(error => {
    console.log(error)
  })

  export default {
    components : {
      ckeditor: CKEditor.component
    },
    data() {
      return {
          editor: ClassicEditor,
          // TODO: For sending emails this needs to be empty.
          // For product descriptions this needs to be the existing product description.
          editorData: '<p>Content of the editor.</p>',
          editorConfig: {
            simpleUpload: {
              uploadUrl: 'https://apd-be-prod.smrxbtsdpc.us-west-1.elasticbeanstalk.com/asset',
              withCredentials: true,
            },
            toolbar: {
              items: [
                'heading',
                '|',
                'bold',
                'italic',
                'link',
                'bulletedList',
                'numberedList',
                '|',
                'indent',
                'outdent',
                '|',
                'imageUpload',
                'blockQuote',
                'insertTable',
                'mediaEmbed',
                'undo',
                'redo',
                'alignment',
                'fontBackgroundColor',
                'fontColor',
                'fontSize',
                'fontFamily',
                'highlight',
                'horizontalLine',
                'imageInsert',
                'strikethrough',
                'specialCharacters',
                'underline'
              ]
            },
            language: 'en',
            image: {
              toolbar: [
                'imageTextAlternative',
                'imageStyle:full',
                'imageStyle:side'
              ]
            },
            table: {
              contentToolbar: [
                'tableColumn',
                'tableRow',
                'mergeTableCells',
                'tableCellProperties',
                'tableProperties'
              ]
            },
          },
      };
    },
    methods: {
      submit() {
        console.log(this.editorData)
        // TODO: Below mutation is just for creating product, obviously, must add
        // "sendEmail" mutation if this editor is being used for sending emails
        axios.post('https://apd-be-prod.smrxbtsdpc.us-west-1.elasticbeanstalk.com/query', {
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
