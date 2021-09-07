<template>
    <div class="home">
        <div class="title">
            <span>相关点位</span>
        </div>
        <div class="list" v-for="(item,index) in current_point_list" :key="index">
            <div class="unit">
                <span class="name">{{item.currency_name}}</span>
                <span class="point-about">
                    <i class="point-low" v-for="(itemTwo, indexTWo) in item.low_point_list" :key="indexTWo">{{itemTwo}}</i>
                    <i class="point-high" v-for="(itemThree, indexThree) in item.high_point_list" :key="`list3${indexThree}`">{{itemThree}}</i>
                </span>
            </div>
        </div>
        <div class="add-style" @click="addEvent('DIANWEI')">
            <span>+</span>
        </div>
        <div class="title account-name">
            <span>账户</span>
        </div>
        <div class="list" v-for="(itemFour,indexFour) in currenct_account_list" :key="`list4${indexFour}`">
            <div class="unit account-unit">
                <div class="account-about">
                    <span class="name account-name">{{itemFour.account_name}}</span>
                    <span class="amount">{{itemFour.total_amount}}</span>
                </div>
                <div class="list-content">
                    <div class="table-title">
                        <span class="currency">币种</span>
                        <span>购买金额</span>
                        <span>购买数量</span>
                        <span>购买价格</span>
                    </div>
                    <div class="list" v-for="(itemFive,indexFive) in itemFour.current_have_currency_list" :key="`list5${indexFive}`">
                        <div class="unit unit-flex">
                            <span class="name">{{itemFive.currency_name}}</span>
                            <span class="current-amount">
                                <i @click="modify(indexFour, indexFive, 'AMOUNT')">{{itemFive.principal.amount||'-'}}({{itemFive.principal.unit}})</i>
                                <i class="amount" @click="modify(indexFour, indexFive, 'QUALITY')">{{itemFive.current_currency_amount}}</i>
                                <i class="price">[{{holding_cost_price(itemFive.principal.amount, itemFive.current_currency_amount, itemFive.digit)}}]</i>
                            </span>
                        </div>
                    </div>
                    <div class="add-account-unit" @click="addEvent('ACCOUNT_CURRENCY')">
                        <span>+</span>
                    </div>
                    <template v-if="itemFour.contract_list.length > 0">
                        <div class="name contract-title">
                            <span>合约</span>
                        </div>
                        <div class="list" v-for="(itemSix, indexSix) in itemFour.contract_list" :key="indexSix">
                            <div class="unit">
                                <span class="name">{{itemSix.currency_name}}</span>
                                <span @click="modify(indexFour, indexSix, 'CONTRACT')">{{itemSix.total_amount}}</span>
                            </div>
                        </div>
                    </template>
                </div>
            </div>
        </div>
        <alertAmount 
        @inputContent="getInputContent" 
        @closeAlert="close"
        v-if="show_edit_alert"></alertAmount>
        <addAlert v-if="show_add_alert" @closeADDAlert="closeADD"></addAlert>
    </div>
</template>

<script>

import { division } from "@/common-js/count-rules.js"
import { liandongDigits } from "@/common-js/check-rule.js"

import alertAmount from "@/components/alert.vue"
import addAlert from "@/components/add-alert.vue"

export default {
    components: {
        alertAmount,
        addAlert
    },
    data() {
        return {
            // 当前点位列表
            current_point_list: [
                {
                    currency_name: "BTC",
                    high_point_list: ["52000", "54000"],
                    low_point_list: ["50000"]
                },
                {
                    currency_name: "ETH",
                    high_point_list: ["4000"],
                    low_point_list: ["3700"]
                },
                {
                    currency_name: "LTC",
                    high_point_list: ["238"],
                    low_point_list: ["210"]
                },
                {
                    currency_name: "DOT",
                    high_point_list: ["34.7", "35"],
                    low_point_list: ["30", "32"]
                }
            ],
            // 当前账户列表
            currenct_account_list: [
                {
                    account_name: "BIXIN",
                    total_amount: 11715,
                    current_have_currency_list: [
                        {
                            currency_name: "BTC",
                            principal: {
                                amount: "2245",
                                unit: "USDT"
                            },
                            current_currency_amount: "0.04901788",
                            digit: 4
                        },
                        {
                            currency_name: "ETH",
                            principal: {
                                amount: "850",
                                unit: "USDT"
                            },
                            current_currency_amount: "0.27872768",
                            digit: 4
                        },
                        {
                            currency_name: "LTC",
                            principal: {
                                amount: "1366",
                                unit: "USDT"
                            },
                            current_currency_amount: "10.02353204",
                            digit: 4
                        }
                    ],
                    contract_list: [
                        {
                            currency_name: "BTC",
                            total_amount: "0.00307263"
                        },
                        {
                            currency_name: "ETH",
                            total_amount: "0.0475"
                        }
                    ]
                },
                {
                    account_name: "HOO",
                    total_amount: 500,
                    current_have_currency_list: [
                        {
                            currency_name: "HOO",
                            principal: {
                                amount: 500,
                                unit: "USDT"
                            },
                            current_currency_amount: 881,
                            digit: 4
                        }
                    ],
                    contract_list: []
                },
                {
                    account_name: "火币",
                    total_amount: "1700",
                    current_have_currency_list: [
                        {
                            currency_name: "ICP",
                            principal: {
                                amount: "230",
                                unit: "USDT"
                            },
                            current_currency_amount: "2.11576",
                            digit: 4
                        },
                        {
                            currency_name: "NFT",
                            principal: {
                                amount: "100",
                                unit: "USDT"
                            },
                            current_currency_amount: "41666666.666",
                            digit: 11
                        },
                        {
                            currency_name: "HT",
                            principal: {
                                amount: "100",
                                unit: "USDT"
                            },
                            current_currency_amount: "7.3485734",
                            digit: 4
                        },
                        {
                            currency_name: "FIL",
                            principal: {
                                amount: "",
                                unit: "USDT"
                            },
                            current_currency_amount: "5.9244234",
                            digit: 4
                        },
                        {
                            currency_name: "DOT",
                            principal: {
                                amount: "",
                                unit: "USDT"
                            },
                            current_currency_amount: "12.8364462",
                            digit: 4
                        },
                        {
                            currency_name: "UNI",
                            principal: {
                                amount: "",
                                unit: "USDT"
                            },
                            current_currency_amount: "8.06794661",
                            digit: 4
                        },
                    ],
                    contract_list: []
                },
                {
                    account_name: "币安",
                    total_amount: "1000",
                    current_have_currency_list: [
                        {
                            currency_name: "TLM",
                            principal: {
                                amount: "250",
                                unit: "USDT"
                            },
                            current_currency_amount: "748.251",
                            digit: 4
                        },
                        {
                            currency_name: "DOGE",
                            principal: {
                                amount: "100",
                                unit: "USDT"
                            },
                            current_currency_amount: "321.678",
                            digit: 8
                        },
                        {
                            currency_name: "SAND",
                            principal: {
                                amount: "100",
                                unit: "USDT"
                            },
                            current_currency_amount: "97.902",
                            digit: 4
                        }
                    ],
                    contract_list: []
                }
            ],
            /**
             * 页面相关参数:
             * 父下标
             * 子下标
             * 当前改变的key: AMOUNT QUALITY CONTRACT
             * 显示编辑弹窗
             * 显示添加弹窗
             */
            father_index: "",
            son_index: "",
            current_change_key: "",
            show_edit_alert: false,
            show_add_alert: false
        }
    },
    computed: {
        /**
         * 页面参数：
         * 持仓成本价
         */
        holding_cost_price() {
            return function(tolal_price, buy_amount, get_digit=4) {
                return liandongDigits(division(tolal_price||0, buy_amount), get_digit)
            }
        }
    },
    methods: {
        /**
         * 点击事件：
         * 修改
         * 添加事件
         */
        modify(father_index, son_index, key) {
            this.father_index = father_index
            this.son_index = son_index
            this.current_change_key = key
            this.show_edit_alert = true
        },
        addEvent(get_key) {
            if(get_key === 'DIANWEI') {
                console.log("添加新币种点位========>")
            } else {
                console.log("添加新账户资产========>")
            }
            this.show_add_alert = true
        },
        /**
         * 修改金额组件
         */
        getInputContent(value) {
            let get_key = this.current_change_key
            let get_father_index = this.father_index
            let get_son_index = this.son_index
            if(get_key === "AMOUNT") {
                this.currenct_account_list[this.father_index].current_have_currency_list[this.son_index].principal.amount = value
            } else if(get_key === "QUALITY") {
                this.currenct_account_list[this.father_index].current_have_currency_list[this.son_index].current_currency_amount = value
            } else if(get_key === "CONTRACT") {
                this.currenct_account_list[get_father_index].contract_list[get_son_index].total_amount = value
            }
            this.close()
        },
        close() {
            this.show_edit_alert = false
        },
        /**
         * 添加
         */
        closeADD() {
            this.show_add_alert = false
        }
    }
}
</script>

<style lang="less" scoped>
.home {
    width: 100%;
    height: 100%;
    overflow: auto;
}
.title {
    span {
        color: black;
        font-size: 18px;
        font-weight: 650;
    }
}
.list {
    .unit {
        // display: flex;
        // align-items: flex-start;
        .name {
            width: 100px;
        }
        .point-about {
            display: flex;
            i {
                width: 50px;
            }
        }
        .account-about {
            width: 120px;
            span.account-name {
                color: black;
                font-size: 18px;
            }
        }
        .list-content {
            .table-title {
                display: flex;
                span {
                    width: 100px;
                }
            }
        }
        .current-amount {
            color: orange;
            display: flex;
            i {
                width: 100px;
                word-wrap: break-word;
            }
            .price {
                color: green;
            }
        }
    }
    .unit-flex {
            display: flex;
    }
    .account-unit {
        margin-top: 20px;
    }
}
.account-name {
    margin-top: 30px;
}
.point-high {
    color: green;
    margin-right: 4px;
}
.point-low {
    color: red;
    margin-right: 10px;
}
.contract-title {
    span {
        color: blue;
    }
}
</style>