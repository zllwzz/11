<template>
    <section ref="threat">
        <text-desc :textData="textData"></text-desc>
        <div class="chart-desc">
            <left-card class="pull-left" :leftData="leftData"></left-card>
            <div class="arrow-point pull-left">
                <img class="arrow-img" src="./../img/arrow_point.png" alt="" />
            </div>
            <right-card class="pull-right" :rightData="rightData"></right-card>
        </div>
    </section>
</template>

<script>
    /**
     * 攻击趋势
     */

    import TextDesc from './text_desc.vue';
    import RightCard from './right_card.vue';
    import LeftCard from './left_card.vue';
    import { TIME_OPTIONS } from 'const/text';

    export default {
        components: {
            TextDesc,
            RightCard,
            LeftCard
        },

        data () {
            return {
                textData: {}, // 顶部文本描述
                leftData: {}, // 左边图表数据
                rightData: {} // 右边图表数据
            };
        },

        mounted () {
            let vm = this;

            vm.getCalculateData(TIME_OPTIONS[0].value);
        },

        methods: {
            getCalculateData (currTime) {
                let vm = this,
                    threat = vm.$refs.threat;

                if (!threat) {
                    return false;
                }

                vm.$ajax({
                    url: '/py/yd-webui/v1/risk/external-threat/calculate',
                    type: 'GET',
                    showLoadingDom: threat,
                    data: {
                        filter_time: currTime
                    },
                    successCallback (json) {
                        let data = json.data,
                            xData = [],
                            lineData = [],
                            barData = [[], [], [], []];

                        vm.textData = {
                            attackNum: data.total_attack_num,
                            attackSource: data.total_attack_source,
                            attackType: data.total_attack_type
                        };

                        vm.rightData = {
                            barData: data.top_attack,
                            otherNum: data.other_attack_num
                        };

                        data.attack_value.forEach(item => {
                            xData.push(item.attack_time); // x轴数据
                            lineData.push(item.attack_times); // 折线数据
                            barData[3].push(item.deadly_attack); // 致命
                            barData[2].push(item.high_attack); // 高
                            barData[1].push(item.middle_attack); // 中
                            barData[0].push(item.low_attack); // 低
                        });

                        vm.leftData = {
                            xData: xData,
                            lineData: lineData,
                            barData: barData
                        };
                    }
                });
            }
        }
    };
</script>

<style scoped>
    .chart-desc{
        height: 388px;
        margin-left: 30px;
        margin-right: 15px;
        border: 1px solid #EEEEEE;
        border-radius: 5px;
    }
    .arrow-point{
        position: relative;
        width: 9%;
        height: 100%;
        text-align: center;
    }
    .arrow-img{
        position: absolute;
        top: 50%;
        left: 40%;
        transform: translate(-50%, -50%);
        width: 121px;
        margin: auto;
    }
</style>
