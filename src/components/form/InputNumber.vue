<template>
    <div :class="bemm()">
        <label :class="bemm('label')" v-if="label">
            {{ label }}
        </label>
        <input :class="bemm('control')" :placeholder="placeholder" type="number" v-model="value" />
    </div>
</template>

<script lang="ts" setup>

import { useBemm } from 'bemm';
const bemm = useBemm('input-number');

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
const value = defineModel({ type: Number })
</script>

<style lang="scss">
.input-number {
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
        border: 1px solid var(--primary);
        border-radius: 3px;
        font-size: 1em;
    }
}</style>