<template>
    <div :class="bemm()">
        <label :class="bemm('label')" v-if="label">
            {{ label }}
        </label>
        <input :class="bemm('control')" :placeholder="placeholder" type="checkbox" v-model="value" />
    </div>
</template>

<script lang="ts" setup>

import { useBemm } from 'bemm';
const bemm = useBemm('input-checkbox');

defineProps({
    label: {
        type: String,
        default: ""
    },
    placeholder: {
        type: String,
        default: ""
    }
})
const value = defineModel({ type: Boolean})
</script>

<style lang="scss">
.input-checkbox {
    display: flex;
    flex-direction: column;

    &+*{ margin-top: 1em;}

    &__label {
        padding: .5em;
        text-align: left;
    }

    &__control {
        padding: .5em;
        background-color: transparent;
        border: 1px solid currentColor;
        border-radius: 3px;
        font-size: 1em;
    }
}</style>