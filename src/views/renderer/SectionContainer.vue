<template>
    <div class="section-container">
        <component
            :is="sectionViewComponent"
            :section="section"
            :rows="rows"
            :controls="controls"
            :key="section.uniqueId"
            :value-container="valueContainer"
            :validation-errors="validationErrors"
            :read-only="readOnly"
            :current-step="currentStep"
            @asd="asdasd"
        />
    </div>
</template>

<script>
    import {SECTION_TYPES} from "@/configs/section";
    import FormBuilderBusiness from "@/mixins/form-builder-mixins";

    export default {
        name: "SectionContainer",
        mixins: FormBuilderBusiness,
        props: {
            section: Object,
            rows: Object,
            controls: Object,
            valueContainer: Object,
            validationErrors: Object,
            readOnly: Boolean,
            currentStep: String
        },

        computed: {
            sectionViewComponent() {
                return SECTION_TYPES[this.section.type].rendererView
            }
        },

        methods: {
            asdasd() {
                // console.log("----------------");
                console.log(this.controls);
                this.$emit("asd")
            }
        },

        created() {
            // console.log(this.currentStep);
        }
    }
</script>

<style scoped>
</style>