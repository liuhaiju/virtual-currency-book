<template>
    <alertBackground @closeAlert="closeAlert">
        <div class="content">
            <div class="list">
                <div class="unit">
                    <div class="title">
                        <span>币种</span>
                    </div>
                    <div class="input">
                        <input type="text" placeholder="添加内容">
                    </div>
                    <div class="title">
                        <span>支撑</span>
                    </div>
                    <div class="input">
                        <input type="text" placeholder="添加内容">
                        <span>确定</span>
                    </div>
                    <div class="title">
                        <span>压力</span>
                    </div>
                    <div class="input">
                        <input type="text" placeholder="添加内容">
                        <span>确定</span>
                    </div>
                </div>
            </div>
            <div class="btn">
                <span>按钮</span>
            </div>
        </div>
    </alertBackground>
</template>

<script>
import alertBackground from "./alert-background.vue"

export default {
    components: {
        alertBackground
    },
    methods: {
        closeAlert() {
            this.$emit("closeADDAlert")
        }
    }
}
</script>

<style lang="less" scoped>
.content {
    background-color: white;
    width: 300px;
    height: 300px;
}
</style>