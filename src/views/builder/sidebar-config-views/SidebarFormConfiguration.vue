<template>
    <div class="sidebar-form-configuration">
        <h5>{{$ml.get('MAIN_FORM_CONFIGURATION')}}</h5>

        <div :class="styles.FORM.FORM_GROUP">
            <label>{{$ml.get('FORM_HEADLINE')}}</label>
            <input type="text"
                   :class="styles.FORM.FORM_CONTROL"
                   v-model="formConfiguration.headline">
        </div>

        <div :class="styles.FORM.FORM_GROUP">
            <label>{{$ml.get('FORM_SUB_HEADLINE')}}</label>
            <input type="text"
                   :class="styles.FORM.FORM_CONTROL"
                   v-model="formConfiguration.subHeadline">
        </div>

        <div :class="styles.FORM.FORM_GROUP">
            <label>
                {{$ml.get('SHOW_FORM_HEADLINE')}}
                <input type="checkbox" v-model="formConfiguration.isShowHeadline">
            </label>
        </div>

        <div :class="styles.FORM.FORM_GROUP">
            <label>
                {{$ml.get('WRAPPER_FORM_OUTSIDE_OF_THE_FORM')}}
                <input type="checkbox" v-model="formConfiguration.renderFormTag">
            </label>
        </div>

        <div v-show="formConfiguration.renderFormTag">

            <div :class="styles.FORM.FORM_GROUP">
                <label>Form-TAG Action URL</label>
                <input type="text"
                       :class="styles.FORM.FORM_CONTROL"
                       v-model="formConfiguration.formActionURL">
            </div>

            <div :class="styles.FORM.FORM_GROUP">
                <label>Form-TAG Method</label>
                <input type="text"
                       :class="styles.FORM.FORM_CONTROL"
                       v-model="formConfiguration.formMethod">
            </div>

        </div>

        <div :class="styles.FORM.FORM_GROUP">
            <label>
                {{$ml.get('ENABLE_SERVER_SIDE_VALIDATION')}}
                <input type="checkbox" v-model="formConfiguration.enableServerSideValidation">
            </label>
        </div>

        <div v-show="formConfiguration.enableServerSideValidation">
            <div :class="styles.FORM.FORM_GROUP">
                <label>Validation URL API Endpoint</label>
                <input type="text"
                       :class="styles.FORM.FORM_CONTROL"
                       v-model="formConfiguration.serverSideValidationEndpoint">
            </div>
        </div>

        <div class="buttons">
            <button :class="styles.BUTTON.PRIMARY" @click="save(false)">
                {{$ml.get('SAVE')}}
            </button>
            <button :class="styles.BUTTON.INFO" @click="save(true)">
                {{$ml.get('SAVE_CLOSE')}}
            </button>
        </div>
    </div>
</template>

<script>
    import {STYLE_INJECTION_MIXIN} from "@/mixins/style-injection-mixin";
    import {SIDEBAR_BODY_MIXIN} from "@/mixins/sidebar-body-mixin";
    import {FORM_DEFAULT_DATA} from "@/configs/form";

    /**
     * @property {FORM_DEFAULT_DATA} formConfiguration
     */
    export default {
        name: "SidebarFormConfiguration",
        mixins: [STYLE_INJECTION_MIXIN, SIDEBAR_BODY_MIXIN],
        data: () => ({
            // OVERRIDE IT FROM `SIDEBAR_BODY_MIXIN`
            dataKey: "formConfiguration",
            formConfiguration: Object.assign({}, FORM_DEFAULT_DATA),
        }),

        methods: {
            /**
             * Pre-Form-Validation
             * @return {String}
             */
            preSaveValidation() {
                if (this.formConfiguration.enableServerSideValidation && !this.formConfiguration.serverSideValidationEndpoint) {
                    return "You must include the Server-side Validation Endpoint";
                }

                return ""
            }
        },

        created() {
            // retrieve the data from `GlobalSidebar` passed in
            this.formConfiguration = Object.assign({}, this.formConfiguration, this.dataPackage)
        }
    }
</script>