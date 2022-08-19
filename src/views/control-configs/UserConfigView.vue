<template>
    <div>
        <!-- <div :class="styles.FORM.FORM_GROUP">
            <label>
                Multiple Selection?
                <input type="checkbox" v-model="control.multiple">
            </label>
        </div> -->

        <div :class="styles.FORM.FORM_GROUP">
            <label>Option-Data-List Mode</label>
            <select :class="styles.FORM.FORM_CONTROL"
                    v-model="control.dataMode">

                <option v-for="item in listDataModes"
                        :key="item.val"
                        :value="item.val"
                        v-text="item.description">
                </option>

            </select>
            <!-- <v-select :options="options" label="title" v-model="control.dataMode"  ></v-select> -->
        </div>

        <div v-show="this.control.dataMode === listDataModes.api.val">
            <div :class="styles.FORM.FORM_GROUP">
                <label>
                    Rest-API URL
                    <icon-tooltip icon="informationOutline"
                                  text="Your API-Endpoint URL - GET Method. It must return an Array. Read more on Documentation" />
                </label>

                <input type="text"
                       placeholder="https://your-domain/rest-api/get-list/..."
                       :class="styles.FORM.FORM_CONTROL"
                       v-model="control.apiURL">
            </div>

            <div :class="styles.FORM.FORM_GROUP">
                <label>
                    Value Key of your Object
                    <icon-tooltip icon="informationOutline"
                                  text="[{'value': '',...}] => value" />
                </label>

                <input type="text"
                       :class="styles.FORM.FORM_CONTROL"
                       v-model="control.apiValueKey">
            </div>

            <div :class="styles.FORM.FORM_GROUP">
                <label>
                    Text Key of your Object
                    <icon-tooltip icon="informationOutline"
                                  text="[{'text': '',...}] => text" />
                </label>

                <input type="text"
                       :class="styles.FORM.FORM_CONTROL"
                       v-model="control.apiTextKey">
            </div>
        </div>

    </div>
</template>

<script>
    import {CONTROL_SPECIAL_CONFIG_MIXIN} from "@/mixins/control-special-config-mixin";
    import {DROPDOWN_DATA_MODES} from "@/configs/control-config-enum";
    import ListItem from "@/libraries/list-item.class";

    export default {
        name: "UserConfigView",
        mixins: [CONTROL_SPECIAL_CONFIG_MIXIN],
        data: () => ({
            listDataModes : {
                api: {
                    val: 'api',
                    description: "API - List Items from your own API",
                },
            },
            selected: "api",
            options:[
  {
    "userId": 1,
    "id": 1,
    "title": "sunt "
  },
  {
    "userId": 1,
    "id": 2,
    "title": "qui est"
  },
  {
    "userId": 1,
    "id": 3,
    "title": "ea molestias"
  }]
        }),

        methods: {
            /**
             * Add new List-Item for the Current Radio/Checkbox
             */
            addListItem() {
                this.control.items.push(
                    new ListItem('', '')
                )
            },

            /**
             * Remove list-Item by Index of the Array
             */
            removeListItem(index) {
                this.control.items.splice(index, 1)
            }
        },

        computed: {
            /**
             * Dropdown Data Modes List
             */
            // listDataModes: () => DROPDOWN_DATA_MODES,
        }
    }
</script>