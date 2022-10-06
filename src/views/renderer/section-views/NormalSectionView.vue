<template>
    <div class="normal-section">
        <div class="headline-block" v-show="section.isShowHeadline">
            <h2 :class="section.headlineAdditionalClass" v-text="section.headline"></h2>
            <p :class="section.subHeadlineAdditionalClass" v-text="section.subHeadline"></p>
        </div>

        <div :class="containerClasses">
            <ControlView
                v-for="controlId in section.controls"
                :key="controlId"
                :control="controls[controlId]"
                :parent-id="section.uniqueId"
                :value-container="valueContainer"
                :validation-errors="validationErrors"
                :read-only="readOnly"
                :current-step="currentStep"
                @asd="asdasd"
            />
        </div>
    </div>
</template>

<script>
    import {RENDERER_SECTION_VIEW_MIXIN} from "@/mixins/renderer-section-view-mixin";

    /**
     * @property {Object} section
     * @property {Object} rows RowId - RowData
     * @property {Object} controls ControlId - ControlData
     * @property {Array} section.rows
     * @property {Array} section.controls
     * @property {String} currentStep
     */
    export default {
        name: "NormalSectionView",
        mixins: [RENDERER_SECTION_VIEW_MIXIN],
        data: () => ({

        }),
        methods: {
            asdasd() {

                // for(let key in this.controls){
                //     if(key == y){
                //         this.controls[key].permission = x
                //     }
                // }
                // console.log(this.controls);
                this.$emit("asd")
            }
        },

        created() {
            // console.log("normal section ", this.currentStep);
        }
    }
</script>
