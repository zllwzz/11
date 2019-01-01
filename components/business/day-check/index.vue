<template>
    <section class="loop-wrap" :style="`height: ${height}px`">
        <div class="label-info-content">
            <loop-label v-for="(item, index) in zeroDay" :key="index" :zero="item"></loop-label>
        </div>
        <div class="label-info-wrap">
            <div class="has-happen">
                <span class="label-square has-happen-label"></span>
                已发生
            </div>
            <div class="has-happen not-happen">
                <span class="label-square not-happen-label"></span>
                未发生
            </div>
            <div class="has-happen not-happen">
                <svg class="icon circle-icon-yundun">
                    <use xlink:href="#icon-jichufanghu"></use>
                </svg>
                云盾已防护
            </div>
        </div>
    </section>
</template>

<script>
    /**
     *  0Day漏洞检测
     */

    import LoopLabel from './loop-label/index.vue';

    export default {
        components: {
            LoopLabel
        },

        props: {
            zeroDay: {
                type: Array,
                default: () => []
            },
            height: {
                type: Number,
                default: 257
            }
        }
    };
</script>

<style scoped lang="less">
    .loop-wrap {
        display: flex;
        justify-content: center;
        position: relative;
        height: 257px;
        padding: 20px 0 55px 0;
        text-align: center;

        .label-info-content{
            align-self: center
        }

        .label-info-wrap {
            display: flex;
            justify-content: space-around;
            position: absolute;
            bottom: 22px;

            .has-happen {
                display: inline-block;
                margin-right: 34px;
            }
            .has-happen:last-child {
                margin-right: 0;
            }

            .label-square {
                display: inline-block;
                width: 14px;
                height: 14px;
                border-radius: 2px;
                vertical-align: middle;
            }

            .has-happen-label {
                background: #FFEEEE;
                border: 1px solid #FF3333;

            }

            .not-happen-label {
                background: #F7FBFF;
                border: 1px solid #4A90E2;
            }

            .circle-icon-yundun {
                font-size: 16px;
                color: #22B07B;
            }
        }
    }
</style>
