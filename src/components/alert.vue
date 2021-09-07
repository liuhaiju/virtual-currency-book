<template>
    <div class="alert-edit" @click.self="closeAlert">
        <div class="content">
            <div class="title">
                <span>请输入数量</span>
            </div>
            <div class="input">
                <input type="text" placeholder="输入内容" v-model="input_content">
            </div>
            <div class="btn" @click="confirmInput">
                <span>确认输入</span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            input_content: ""
        }
    },
    methods: {
        confirmInput() {
            this.$emit("inputContent", this.input_content)
        },
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
    .content {
        width: 300px;
        background-color: white;
    }
}
</style>