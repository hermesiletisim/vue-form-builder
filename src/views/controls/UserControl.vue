<template>
    <div class="person-input">
        <!-- <select :id="control.uniqueId"
            :class="controlFieldClass"
            :name="control.name || control.uniqueId"
            @input="updateValue($event.target.value)"
            :multiple="this.control.multiple"
        >
            <option disabled
                    selected
                    v-text="control.placeholderText"
                    v-if="control.placeholderText"
            ></option>

            <option v-for="optionObj in listOptions"
                    :key="optionObj._id"
                    :value="optionObj.value"
                    v-text="concatNames(optionObj.n, optionObj.sn)"
                    :selected="value === optionObj.value"
            ></option>
        </select> -->
        <!-- <v-select :options="listOptions" label="n" @keyup.native="getContacts($event.target.value)"></v-select> -->
        <!-- <v-select :id="control.uniqueId" :name="control.name || control.uniqueId" :multiple="this.control.multiple" :options="listOptions" :reduce="item => item.n">
            <template slot="option" slot-scope="option">
                {{ option.n }} {{option.sn}} 
            </template>
            <template slot="selected-option" slot-scope="option">
                {{ option.n }} {{option.sn}}
            </template>
        </v-select> -->
        <input :id="control.uniqueId"
           :type="control.typeAttribute"
           :class="controlFieldClass"
           :value="value"
           v-model="fullName"
           :name="control.name || control.uniqueId"
           :placeholder="control.placeholderText"
           @input="getContacts($event.target.value)"
        />
        <ul class="new-dropdown border-0 p-0 autocomplete-results" v-show='listOptions.length>0'>
            <li class="autocomplete-result" v-if="listOptions.length>0" v-for='(result, i) in listOptions' :key="i" @click="setResult(result)">{{result.n}} {{result.sn}}</li>
            <li class="autocomplete-result" v-if="listOptions.length<1">NOT FOUND</li>
        </ul>
    </div>
    
    
</template>

<script>
    import {CONTROL_FIELD_EXTEND_MIXIN} from "@/mixins/control-field-extend-mixin";
    import {DROPDOWN_DATA_MODES} from "@/configs/control-config-enum";
    import ListItem from "@/libraries/list-item.class";
    import axios from 'axios'
    import {mapState} from 'vuex'
    

    /**
     * Dropdown Control.
     * I've been thinking all day, all night, should I use some library (select2, choices.js,...)
     * But, after some researched via https://bundlephobia.com/ , I decided to use Native Select instead
     * In order to save some KBs, the bundle is kinda bigger now @@
     * @property {ListItem[]} listOptions
     */
    export default {
        name: "UserControl",
        mixins: [CONTROL_FIELD_EXTEND_MIXIN],
        data: () => ({
            listOptions: [],
            dataMode: "",
            apiURL: "",
            fullName:""
        }),

        watch: {
            control: {
                deep: true,
                handler(controlObj) {
                    // we cached it. If nothing change => no reload
                    if (
                        // single check
                        controlObj.dataMode !== this.dataMode ||
                        controlObj.apiURL !== this.apiURL ||

                        // list check
                        (
                            controlObj.dataMode === DROPDOWN_DATA_MODES.list.val &&
                            controlObj.items.length !== this.listOptions.length
                        )
                    ) {
                        return this.retrieveOptionLists()
                    }
                },
            },
        },
        computed: {
          ...mapState([
              'baseUrl'
            ]),
        },

        methods: {
            getContacts(keyWord){
                this.fullName = keyWord
               
                var dataObj={
                    search:keyWord
                }
                
                axios({
                    method:'POST',
                    url:'/api/quickFilterContact',
                    baseURL: this.baseUrl,
                    data: dataObj,
                    withCredentials:true
                }).then((res)=>{
                    return res
                }).then((res)=>{
                    if(!res.data.status){
                         
                    }
                    if(res.data.status.code==0){
                        this.listOptions = res.data.content.contacts
                    }
                })
                
                                 
            },
            setResult(res){
                const result = this.listOptions.filter(item => item._id.$oid == res._id.$oid)

                if(result[0].sn !== undefined){
                    this.fullName = result[0].n + " " + result[0].sn
                }
                else{
                    this.fullName = result[0].n
                }

                this.updateValue(this.fullName)

                this.listOptions = []
                
            },
            concatNames(name, surname){
                var fullName = name;

                if(surname !== undefined){
                    fullName = name + " " + surname
                }
                
                return fullName;
            },
            retrieveOptionLists() {
                this.dataMode = this.control.dataMode;
                this.apiURL = this.control.apiURL;

                // pick up option data
                if (this.control.dataMode === DROPDOWN_DATA_MODES.list.val) {
                    this.listOptions = this.control.items;
                    return
                }

                // our code goes down here? => REST-API
                if (!this.control.apiURL) {
                    throw new TypeError("[Dropdown] In order to use REST-API for Dropdown. You must set your Rest-API Endpoint.");
                }

                // is it http/https?
                if (this.control.apiURL.indexOf("http://") < 0 && this.control.apiURL.indexOf("https://") < 0) {
                    throw new TypeError("[Dropdown] Rest-API Endpoint must be valid http/https URL.");
                }

                // ok retrieve now
                fetch(this.control.apiURL, {
                    method: "GET"
                }).then(result => result.json())
                  .then(this.afterRestAPICallDataSuccessfully)
                  .catch(this.restAPICallErrorHandling);
            },

            /**
             * [FOR-RestAPI-Dropdown][EVENT]
             * Appending data after success
             * @param {[]} result
             */
            afterRestAPICallDataSuccessfully(result) {
                if (!Array.isArray(result)) {
                    throw new TypeError(`[DROPDOWN-${this.control.name}] Wrong API-data format.`);
                }

                // clear list
                this.listOptions = [];

                // traversal all list and add it into the list
                result.forEach(optionObj => {
                    this.listOptions.push(
                        new ListItem(
                            optionObj[this.valueKey],
                            optionObj[this.textKey]
                        )
                    );
                });
            },

            /**
             * [FOR-RestAPI-Dropdown][EVENT]
             * Show error for dropdown.
             */
            restAPICallErrorHandling(e) {
                console.error(`[DROPDOWN-Control-${this.control.uniqueId}] Request API to get data failed.`, e);
            },
        },

        computed: {
            /**
             * [API-Only] Get Text Key
             * @returns {string}
             */
            textKey() {
                return this.control.apiTextKey || "text"
            },

            /**
             * [API-Only] Get Text Key
             * @returns {string}
             */
            valueKey() {
                return this.control.apiValueKey || "value"
            },
        },
        
        watch: {
            fullName: function() {
                if(this.fullName == ""){
                    this.listOptions = []
                }
            }
        },

        created() {
            this.retrieveOptionLists()
        }
    }
</script>

<style scoped>

</style>