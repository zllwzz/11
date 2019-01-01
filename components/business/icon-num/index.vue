<template>
    <div>
        <div class="left-icon pull-left">
            <svg class="icon" aria-hidden="true">
                <use :xlink:href="'#icon-' + iconText"></use>
            </svg>
        </div>
        <div class="left-text pull-left">
            <p class="left-text__name">{{name}}</p>
            <span class="left-text__num"><odometer-box :value="num"></odometer-box></span>{{unit}}
        </div>
    </div>
</template>

<script>
    /**
     * 字库图标和数字组合
     */

    import OdometerBox from 'components/common/odometer/index.vue';

    export default {
        components: {
            OdometerBox
        },
        props: {
            iconText: {
                type: String,
                default: 'gaojiweixieshijian'
            },
            name: {
                type: String,
                default: ''
            },
            num: {
                type: Number,
                default: 0
            },
            unit: {
                type: String,
                default: '次'
            }
        }
    };
</script>

<style scoped lang="less">
    .left-icon{
        position: relative;
        width: 64px;
        height: 64px;
        border-radius: 50%;
        background: #FEF6E9;
    }
    .left-icon .icon{
        position: absolute;
        top: 0;
        left: 0;
        bottom: 0;
        right: 0;
        margin: auto;
        font-size: 34px;
        color: #FF9900;
    }
    .left-text{
        display: inline-block;
        margin-left: 10px;
        .left-text__name{
            margin-bottom: 0;
        }
        .left-text__num{
            font-size: 36px;
            margin-right: 6px;
        }
    }
</style>