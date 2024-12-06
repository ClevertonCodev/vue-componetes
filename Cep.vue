<template>
    <input 
        :name="$props.name"
        type="text" 
        :id="`field_${$props.name}`"
        v-model="input"
        :class="{ 'is-invalid': $props.hasError }"
        :maxlength="$props.maxLength"
        v-mask="'#####-###'">
</template>

<script>
export default {
    props: {
        value: {
            type: String
        },
        name: {
            type: String
        },
        hasError: {
            type: Boolean,
            default: false
        },
        maxLength: {
            type: Number,
            default: 255
        },
        isNumeric: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {
            input: this.$props.value
        }
    },
    watch: {
        input(input) {
            if (this.$props.isNumeric && input != input.replace(/[^0-9\.]/g, '')) {
                this.input = input.replace(/[^0-9\.]/g, '');
                return;
            }

            this.$emit('input', input);
        }
    },
}
</script>