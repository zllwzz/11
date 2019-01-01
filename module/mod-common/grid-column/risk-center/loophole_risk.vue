<template>
    <div class="tag-box">
        <span class="tag-btn tag-btn__red"
              v-for="(item, index) of rowData.label"
              :key="index">
            {{item}}
        </span>
    </div>
</template>

<script>
    /**
     * 漏洞风险
     */

    export default { // TODO 需要确定到底有多少个
        props: {
            rowData: {
                type: Object,
                default () {
                    return {};
                }
            }
        }
    };
</script>

<style scoped>
    .tag-btn{
        display: inline-block;
        padding: 2px 10px;
        font-size: 12px;
        line-height: 12px;
        margin-right: 5px;
    }
    .tag-btn:last-child{
        margin-right: 0;
    }
    .tag-btn__red{
        border: 1px solid #ffcccc;
        border-radius: 2px;
        color: #ee5555;
        background: #ffeded;
    }
</style>
