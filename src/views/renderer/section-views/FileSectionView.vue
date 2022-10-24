<template>
    <div class="file-section">
         <div class="headline-block p5" v-show="section.isShowHeadline">
             <h2 :class="section.headlineAdditionalClass" v-text="section.headline"></h2>
             <p :class="section.subHeadlineAdditionalClass" v-text="section.subHeadline"></p>
         </div>
         <div class="row md-layout control-list-container">
             <h2>Dosya</h2>
             <div class="input-group w-100 mt-2">
                 <div class="custom-file">
                     <input type="file" class="custom-file-input" id="inputGroupFile02" @change="chooseFile($event)">
                     <label class="custom-file-label mb-0" for="inputGroupFile02" aria-describedby="inputGroupFileAddon02">{{file.name || "Seçili dosya yok"}}</label>
                 </div>
                 <div class="input-group-append">
                     <button class="btn mcBgDark text-white px-5" type="button" id="inputGroupFileAddon04">Yükle</button>
                 </div>
             </div>
         </div>
     </div>
 </template>
 
 <script>
     import {RENDERER_SECTION_VIEW_MIXIN} from "@/mixins/renderer-section-view-mixin";
     import axios from 'axios'
     import {mapState} from 'vuex'
 
     /**
      * @property {Object} section
      * @property {Object} rows RowId - RowData
      * @property {Object} controls ControlId - ControlData
      * @property {Array} section.rows
      * @property {Array} section.controls
      * @property {String} currentStep
      */
     export default {
         name: "FileSectionView",
         mixins: [RENDERER_SECTION_VIEW_MIXIN],
         data () {
             return {
                file: {},
                customerFiles: []
             }
         },
         computed: {
           ...mapState([
               'baseUrl'
             ]),
         },
         methods: {
             chooseFile(event){
                 this.file = event.target.files[0]
             },
             getCloudApiTokens() {
                 axios({
                     method: 'GET',
                     url: '/getAllDefaultCloudApiTokens',
                     baseURL: this.baseUrl,
                 }).then(response => {
                 
                 if(response.data.content){ 
                     console.log(response.data.content.tokens);
                     this.getCustomerFiles()
                 }
                 }).catch(error => {
                     this.$notify({ 
                         group: 'foo',
                         type: 'error', 
                         title: this.$ml.get('ERROR'),
                         text: error
                     })
                 });
             },
             getCustomerFiles() {
                 if(this.customerFileIds.length>0){
                     axios({
                         method: 'GET',
                         url: '/api/listCustomerFiles',
                         baseURL: this.baseUrl,
                         params:{
                             customerFileIds:'['+this.customerFileIds.join(',')+']'
                         }
                     }).then(response => {
                         console.log(response.data.content.files);
                         this.customerFiles = response.data.content.files.reverse()
                     }).catch(error => {
                         this.$notify({
                             group: 'foo',
                             type: 'error',
                             title: this.$ml.get('ERROR'),
                             text: error
                         })
                     });
                 }
                 else{
                     this.customerFiles = []
                 }
             },
         },
 
         watch: {
            
         },
 
         mounted(){
             this.getCloudApiTokens()
         },
 
     }
 </script>
 
 <style scoped>
     .custom-file-label{
         padding-left:80px;
         color: #D1D1D7;
         font-size: 14px;
         font-weight: 500;
         line-height: 24px;
         overflow: hidden;
     }
     .custom-file-label::after{
         left: 0;
         right: auto;
         content: "Göz At";
         border: 1px solid rgba(0, 0, 0, 0.1);
         border-radius: 4px;
         background-color: #3767a0 !important;
         cursor: pointer !important;
         color: #fff !important;
     }
     .mcBgDark {
         background-color: #3767a0 !important;
     }
 </style>
 