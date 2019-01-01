<template>
    <el-popover placement="bottom"  trigger="hover">
        <ul class="hover-wrap">
            <li class="hover-item"> ODay漏洞名字：{{zero.name}}</li>
            <li class="hover-item"> 爆发日期：{{zero.happen_time}}</li>
            <li class="hover-item"> 危险等级：{{RISK_RANK[zero.risk_level]}}</li>
            <li class="hover-item"> 漏洞影响：{{zero.effect}}</li>
            <li class="hover-item"> 处置结果：{{zero.result}}</li>
        </ul>
        <div  class="reference-con" slot="reference">
            <div class="label-item-wrap">
                <span class="label-item" :class="'label-'+ DEFENCE_STATUS[zero.status]">{{zero.name}}</span>
                <svg class="icon circle-icon-yundun" v-if="zero.status === DEFENCE_STATUS.happen">
                    <use xlink:href="#icon-jichufanghu"></use>
                </svg>
            </div>
        </div>
    </el-popover>
</template>

<script>
    /**
     *  漏洞标签
     */


    import {Popover} from 'element-ui';
    import {RISK_RANK, DEFENCE_STATUS} from 'const/text';

    export default {
        components: {
            ElPopover: Popover
        },

        props: {
            status: {
                type: String,
                default: 'happened'
            },

            zero: {
                type: Object,
                default: () => {}
            }
        },

        data () {
            return {
                RISK_RANK,
                DEFENCE_STATUS
            };
        }
    };
</script>

<style scoped lang="less">
    .reference-con {
        display: inline-table;
    }

    .label-item-wrap {
        display: inline-block;
        position: relative;
        margin-top: 10px;
        margin-right: 10px;
    }
    .label-item {
        display: inline-block;
        font-size: 12px;
        text-align: center;
        padding: 6px 10px;
        border-radius: 4px;
    }

    .hover-wrap{
        .hover-item {
            line-height: 30px;
        }
    }

    .label-happened {
        background: #FFEEEE;
        border: 1px solid #FF3333;
        color: #EE5555;
    }

    .label-not {
        background: #F7FBFF;
        border: 1px solid #4A90E2;
        color: #4A90E2;
    }

    .circle-icon-yundun {
        position: absolute;
        top: -10px;
        right: -7px;
        color: #22B07B;
        font-size: 16px;
        z-index: 2;
    }

</style>
