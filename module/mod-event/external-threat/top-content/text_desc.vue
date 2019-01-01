<template>
    <article class="text-desc">
        <icon-num iconText="gaojiweixieshijian" name="最近遭受攻击" :num="textData.attackNum"></icon-num>
        <div class="right-text pull-left">
            <div>
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-zengchang"></use>
                </svg>
                <span>较上周增长5%</span>
            </div>
            <div>
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-heike"></use>
                </svg>
                <span>攻击源：{{textData.attackSource}}个，攻击手段：{{textData.attackType}}个</span>
            </div>
        </div>
    </article>
</template>

<script>
    /**
     * 文字描述
     */

    import IconNum from 'components/business/icon-num/index.vue';

    export default {
        components: {
            IconNum
        },
        props: {
            textData: {
                type: Object,
                default () {
                    return {};
                }
            }
        }
    };
</script>

<style scoped lang="less">
    .text-desc{
        padding: 30px;
        height: 124px;
    }

    .right-text{
        display: inline-block;
        margin-top: 8px;
        margin-left: 70px;
        line-height: 26px;
    }
</style>