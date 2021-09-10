<template>
    <alertBackground @closeAlert="closeAlert">
        <div class="content">
            <div class="list">
                <div class="unit">
                    <div class="title">
                        <span>币种</span>
                    </div>
                    <div class="input">
                        <input type="text" placeholder="添加内容" v-model="add_currency">
                    </div>
                    <div class="title">
                        <span>支撑</span>
                    </div>
                    <div class="input">
                        <input type="text" placeholder="添加内容">
                        <span>确定</span>
                        <span v-for="(item, index) in support_list" :key="index">
                            {{item}}
                        </span>
                    </div>
                    <div class="title">
                        <span>压力</span>
                    </div>
                    <div class="input">
                        <input type="text" placeholder="添加内容">
                        <span>确定</span>
                        <span v-for="(item, index) in support_list" :key="index">
                            {{item}}
                        </span>
                    </div>
                </div>
            </div>
            <div class="btn" @click="confirm">
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
    data() {
        return {
            add_currency: "",
            support_list: [],
            pressure_list: []
        }
    },
    methods: {
        closeAlert() {
            this.$emit("closeADDAlert")
        },
        confirm() {
            let get_params = {
                currency_name: this.add_currency,
                high_point_list: this.pressure_list,
                low_point_list: this.support_list,
                type: "HANGQING"
            }
            console.log("拿到的数据参数====>", get_params)
            this.$emit("addList", get_params)
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