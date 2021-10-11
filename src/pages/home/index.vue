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
                    <span class="balance">[{{account_balance_amount(itemFour)}}]</span>
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
                                <span @click="modify(indexFour, indexSix, 'CONTRACT')">{{itemSix.total_amount}}【{{itemSix.usdt_total_amount}}】</span>
                            </div>
                        </div>
                    </template>
                </div>
            </div>
        </div>
        <div class="title account-name">
            <span>策略推荐</span>
        </div>
        <div class="content-list">
            <div class="content-unit">
                <span>1、1000USDT 2的7次幂</span>
                <span>2、总资金5%</span>
            </div>
        </div>
        <alertAmount 
        @inputContent="getInputContent" 
        @closeAlert="close"
        v-if="show_edit_alert"></alertAmount>
        <addAlert 
        v-if="show_add_alert" 
        @closeADDAlert="closeADD"
        @addList="addCurrencyInfo"></addAlert>
    </div>
</template>

<script>

import { division, addition, Subtraction, Multiplication } from "@/common-js/count-rules.js"
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
                    high_point_list: ["45400"],
                    low_point_list: ["43500"]
                },
                {
                    currency_name: "ETH",
                    high_point_list: ["3200"],
                    low_point_list: ["2950"]
                },
                {
                    currency_name: "DOT",
                    high_point_list: ['33.5'],
                    low_point_list: ['30.5']
                },
                {
                    currency_name: "LINK",
                    high_point_list: [''],
                    low_point_list: ['']
                },
                {
                    currency_name: "CELO",
                    high_point_list: [''],
                    low_point_list: ['']
                },
            ],
            // 当前账户列表
            currenct_account_list: [
                {
                    account_name: "总账户",
                    total_amount: '10211',
                    current_have_currency_list: [
                        {
                            currency_name: "ETH",
                            principal: {
                                amount: "1400",
                                unit: "USDT"
                            },
                            current_currency_amount: "0.49010038",
                            digit: 8
                        },
                        {
                            currency_name: "DOT",
                            principal: {
                                amount: "829.0836708",
                                unit: "USDT"
                            },
                            current_currency_amount: "24.6876",
                            digit: 8
                        },
                        {
                            currency_name: "FIL",
                            principal: {
                                amount: "750",
                                unit: "USDT"
                            },
                            current_currency_amount: "7.0984706",
                            digit: 4
                        },
                        {
                            currency_name: "HT",
                            principal: {
                                amount: "119.92838286",
                                unit: "USDT"
                            },
                            current_currency_amount: "16.11812926",
                            digit: 5
                        },
                        {
                            currency_name: "UNI",
                            principal: {
                                amount: "411.9826",
                                unit: "USDT"
                            },
                            current_currency_amount: "18.85",
                            digit: 4
                        },
                        {
                            currency_name: "MATIC",
                            principal: {
                                amount: "199.99637662",
                                unit: "USDT"
                            },
                            current_currency_amount: "144.74",
                            digit: 6
                        },
                        {
                            currency_name: "BGB",
                            principal: {
                                amount: "500",
                                unit: "USDT"
                            },
                            current_currency_amount: "7290.8997098",
                            digit: 5
                        },
                        {
                            currency_name: "HOO",
                            principal: {
                                amount: "1000",
                                unit: "USDT"
                            },
                            current_currency_amount: "2234.1",
                            digit: 5
                        },
                        {
                            currency_name: "FTT",
                            principal: {
                                amount: "1000",
                                unit: "USDT"
                            },
                            current_currency_amount: "10.60197",
                            digit: 5
                        },
                    ],
                    contract_list: [
                        {
                            currency_name: "USDT",
                            total_amount: "500",
                            usdt_total_amount: "500"
                        }
                    ]
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
         * 账户余额
         */
        holding_cost_price() {
            return function(tolal_price, buy_amount, get_digit=4) {
                return liandongDigits(division(tolal_price||0, buy_amount), get_digit)
            }
        },
        account_balance_amount() {
            return function(item) {
                let have_total_amount = item.total_amount
                let get_position_currency_list = item.current_have_currency_list
                let get_contract_currency_list = item.contract_list
                    let get_position_currency_list_totalAmount = 0
                    let get_contract_currency_list_totalAmount = 0
                    if(get_position_currency_list.length>0) {
                        for(let i=0;i<get_position_currency_list.length;i++) {
                            get_position_currency_list_totalAmount = addition(get_position_currency_list[i].principal.amount||0, get_position_currency_list_totalAmount)
                        }
                    }
                    if(get_contract_currency_list.length>0) {
                        for(let j=0;j<get_contract_currency_list.length;j++) {
                            get_contract_currency_list_totalAmount = addition(get_contract_currency_list[j].usdt_total_amount, get_contract_currency_list_totalAmount)
                        }
                    }
                    return `{
                        "仓本位": ${Multiplication(have_total_amount, 0.1)},
                        "当前仓位": ${addition(get_position_currency_list_totalAmount, get_contract_currency_list_totalAmount)},
                        "剩余仓位": ${Subtraction(have_total_amount, addition(get_position_currency_list_totalAmount, get_contract_currency_list_totalAmount))},
                        "总仓位": ${have_total_amount}
                    }`
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
         * 添加:
         * 关闭
         * 确定
         */
        closeADD() {
            this.show_add_alert = false
        },
        addCurrencyInfo(get_object) {
            this.current_point_list.push(get_object)
            this.closeADD()
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