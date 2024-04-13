<script lang="ts">
import { renderSlot as _renderSlot, createElementVNode as _createElementVNode, openBlock as _openBlock, createElementBlock as _createElementBlock, createCommentVNode as _createCommentVNode, pushScopeId as _pushScopeId, popScopeId as _popScopeId } from "vue"
export default {
    name: 'MyDialog',
    data() {
        return {
            isVisible: false,
        };
    },
    methods: {
        open() {
            this.isVisible = true;
        },
        close() {
            this.isVisible = false;
        },
    },
};
</script>

<template>
    <div v-if="isVisible" class="dialog-backdrop">
        <div class="dialog">
            <slot></slot>
        </div>
    </div>
</template>

<style scoped>

</style>
