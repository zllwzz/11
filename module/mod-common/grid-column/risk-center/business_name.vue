<template>
    <div class="common-ellipsis">
        <span v-for="(item) in rowData.business_name" :title="item" class="column-business-name" target="_blank">{{item}}</span><br>
    </div>
</template>

<script>
    /**
     * 业务名称
     */

    export default {
        props: {
            rowData: {
                type: Object,
                default: () => {}
            }
        }
    };
</script>

<style scoped>
    .column-business-name{
        color: #3e80da;
    }
</style>
