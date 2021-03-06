<template>
    <div class="shopcart" :class="{'highlight': totalCount>0}">
        <div class="shopcart-wrapper">
            <div class="content-left">
                <div class="logo-wrapper" :class="{'highlight': totalCount>0}" @click="toggleList">
                    <span class="icon-shopping_cart logo" :class="{'highlight': totalCount>0}"></span>
                    <i class="num" v-show="totalCount">{{totalCount}}</i>
                </div>
                <div class="desc-wrapper">
                    <p class="total-price" v-show="totalPrice">${{totalPrice}}</p>
                    <p class="tip" v-show="totalPrice" :class="{'highlight': totalCount>0}">{{poiInfo.shipping_fee_tip}}</p>
                </div>
            </div>
            <div class="content-right" :class="{'highlight': totalCount>0}">
                {{payStr}}
            </div>
            <div class="shopcart-list" v-show="listShow" :class="{'show': listShow}">
                <div class="list-top" v-if="poiInfo.discounts2">
                    {{poiInfo.discounts2[0].info}}
                </div>
                <div class="list-header">
                    <h3 class="title">購物清單</h3>
                    <div class="empty" @click="emptyFn">
                        <img src="./ash_bin.png" />
                        <span>清空購物車</span>
                    </div>
                </div>
                <div class="list-content" ref="listContent">
                    <ul>
                        <li class="food" v-for="food in selectFoods" :key="food">
                            <div class="desc-wrapper">
                                <div class="desc-left">
                                    <p class="name">{{food.name}}</p>
                                    <p class="unit" v-show="!food.description">{{food.unit}}</p>
                                    <p class="description" v-show="food.description">{{food.description}}</p>
                                </div>
                                <div class="desc-right">
                                    <span class="price">${{food.min_price}}</span>
                                </div>
                            </div>
                            <div class="cartcontrol-wrapper">
                                <CartControl :food="food" />
                            </div>
                        </li>
                    </ul>
                </div>
                <div class="list-bottom"></div>
            </div>
        </div>
        <div class="shopcart-mask" v-show="listShow" @click="hideMask"></div> 
    </div>
</template>

<script>
import CartControl from '../Cartcontrol/CartControl'
import BScroll from 'better-scroll'

export default {
    data() {
        return {
            fold: true,
            shopScroll: {}
        }
    },
    components: {
        CartControl
    },
    created() {
        // console.log(this.poiInfo);
    },
    props: {
        selectFoods: {
            type: Array
        },
        poiInfo: {
            type: Object,
            default() {}
        }
    },
    methods: {
        toggleList() {
            // Check if total count is 0
            if(!this.totalCount) {
                return;
            }
            this.fold = !this.fold;
        },
        emptyFn() {
            this.selectFoods.forEach((food) => {
                food.count = 0; 
            })
        },
        hideMask() {
            this.fold = true;
        }
    },
    computed: {
        totalCount() {
            let num = 0;
            this.selectFoods.forEach((food) => {
                num += food.count;
            })
            return num;
        },
        totalPrice() {
            let total = 0;
            this.selectFoods.forEach((food) => {
                total += food.min_price * food.count;
            })
            return total
        },
        payStr() {
            if(this.totalCount > 0) {
                return "帳單結算"
            } else {
                return this.poiInfo.min_price_tip;
            }
        },
        listShow() {
            if(!this.totalCount) {
                return false;
            }
            let show = !this.fold;

            // BScroll related
            if(show) {
                this.$nextTick(() => {
                    if(this.shopScroll) {
                        this.shopScroll = new BScroll(this.$refs.listContent, {
                            click: true
                        });
                    } else {
                        this.shopScroll.refresh();
                    }
                });
            }

            return show;
        }
    },
    watch: {
        listShow: {
            deep: true,
            handler: function () {
                if(!this.totalCount) {
                    this.fold = true;
                }
            }
        }
    }
}
</script>

<style>
    @import url("Shopcart.css");
</style>