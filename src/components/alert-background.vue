<template>
    <div class="alert-edit" @click.self="closeAlert">
        <slot></slot>
    </div>
</template>

<script>
export default {
    methods: {
        closeAlert() {
            this.$emit("closeAlert")
        }
    }
}
</script>

<style lang="less" scoped>
.alert-edit {
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.7);
    position: fixed;
    left: 0px;
    top: 0px;
    right: 0px;
    bottom: 0px;
    display: flex;
    align-items: center;
    justify-content: center;
}
</style>