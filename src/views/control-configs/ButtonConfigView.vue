<template>
    <div>
        <div :class="styles.FORM.FORM_GROUP">
            <label>{{$ml.get("BUTTON_TYPE")}}</label>
            <select type="text"
                    :class="styles.FORM.FORM_CONTROL"
                    v-model="control.buttonType"
            >
                <option v-for="item in buttonTypeList"
                        :key="item"
                        :value="item"
                        v-text="item">
                </option>
            </select>
        </div>

        <div :class="styles.FORM.FORM_GROUP">
            <label>{{$ml.get("BUTTON_CLASS")}}</label>
            <select type="text"
                    :class="styles.FORM.FORM_CONTROL"
                    v-model="control.buttonClass"
            >
                <option v-for="(className, name) in buttonClasses"
                        :key="className"
                        :value="className">
                    {{name}} ({{className}})
                </option>
            </select>
        </div>

        <div :class="styles.FORM.FORM_GROUP">
            <label>
                {{$ml.get("RUN_VALIDATION_BEFORE_EMITTING")}}
                <input type="checkbox" v-model="control.isRunValidation">
            </label>
        </div>

        <div :class="styles.FORM.FORM_GROUP">
            <label>
                {{$ml.get("EMITTING_CODE")}}
                <IconTooltip icon="informationOutline"
                             text="V-Form will $emit an Event with <strong>YOUR CODE</strong>" />
            </label>
            <input type="text"
                   :class="styles.FORM.FORM_CONTROL"
                   v-model="control.emitEventCode">
        </div>

        <div :class="styles.FORM.FORM_GROUP">
            <label>
                {{$ml.get("EMITTING_VALUE")}}
                <IconTooltip icon="informationOutline"
                             text="Specific Data-String when V-Form performs invoke $emit" />
            </label>
            <input type="text"
                   :class="styles.FORM.FORM_CONTROL"
                   v-model="control.emitEventData">
        </div>
    </div>
</template>

<script>
    import {CONTROL_SPECIAL_CONFIG_MIXIN} from "@/mixins/control-special-config-mixin";

    export default {
        name: "ButtonConfigView",
        mixins: [CONTROL_SPECIAL_CONFIG_MIXIN],

        computed: {
            buttonTypeList() {
                return ['button', 'submit', 'clear']
            },
            buttonClasses() {
                return this.styles.BUTTON
            }
        }
    }
</script>