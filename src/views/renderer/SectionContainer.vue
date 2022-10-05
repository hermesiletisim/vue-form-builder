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
        },

        computed: {
            sectionViewComponent() {
                return SECTION_TYPES[this.section.type].rendererView
            }
        },

        methods: {
            asdasd() {
                // console.log("----------------");
                // console.log(this.controls);
                this.$emit("asd")
            }
        }
    }
</script>

<style scoped>
</style>