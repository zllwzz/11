<template>
    <article class="right-card">
        <div class="top5-title">业务遭受攻击情况Top5</div>
        <div class="top5-bar" ref="bar"></div>
        <div class="top5-desc">其他业务遭受攻击总和：{{rightData.otherNum}}次</div>
    </article>
</template>

<script>
    /**
     * 右边图表
     */

    import barChart from 'components/common/echarts/bar/index';

    export default {
        props: {
            rightData: {
                type: Object,
                default () {
                    return {};
                }
            }
        },
        watch: {
            rightData: {
                deep: true,
                handler () {
                    let vm = this;

                    vm.initBar();
                }
            }
        },

        methods: {
            initBar () {
                const TOP5 = 5; // 表示最多显示5个
                let vm = this,
                    bar = vm.$refs.bar,
                    seriesData = [],
                    option,
                    data = vm.rightData.barData;

                data.forEach((rs, idx) => {
                    let obj = {
                        name: rs.event_name,
                        type: 'bar',
                        data: [rs.event_num],
                        barWidth: 13,
                        barGap: '315%',
                        label: {
                            normal: {
                                show: true,
                                color: '#7e8397',
                                position: ['0%', -18],
                                formatter (params) {
                                    return params.seriesName;
                                }
                            }
                        },
                        tooltip: {
                            show: true,
                            trigger: 'item',
                            transitionDuration: 0,
                            formatter (params) {
                                return `${params.seriesName}：${params.data}次`;
                            },
                            textStyle: {
                                color: '#fff',
                                fontSize: 12
                            },
                            backgroundColor: 'rgba(0, 0, 0, 0.5)'
                        },
                        itemStyle: {
                            normal: {
                                color: new echarts.graphic.LinearGradient(
                                    0, 0, 1, 0,
                                    [
                                        {offset: 0, color: '#FFE45F '},
                                        {offset: 1, color: '#FFC431'}
                                    ]
                                )
                            }
                        },
                        markPoint: {
                            symbolSize: 1,
                            label: {
                                normal: {
                                    color: '#FFAA00',
                                    formatter: '{c}次',
                                    position: 'right'
                                }
                            },
                            data: [
                                {type: 'max'}
                            ]
                        }
                    };

                    if (idx === 0) {
                        obj.barGap = '250%';
                    }

                    seriesData.push(obj);
                });

                // 需要补空的情况和数量，避免少于5个时看起来自动居中
                let needAddEmpty = TOP5 - data.length;
                if (needAddEmpty > 0 && needAddEmpty !== TOP5) {
                    for (let index = needAddEmpty; index > 0; index--) {
                        seriesData.push({
                            name: '',
                            type: 'bar',
                            barWidth: 13
                        });
                    }
                }
            }
        }
    }
     