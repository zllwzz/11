<template>
    <div>
        <div class="mod-setting" :class="tagClass" v-if="tagVisibal">
            <span class="common-tag-square tag-color">基础防护</span>
            <span v-show="securityConfig.emergency_disposal" class="common-tag-square tag-color">应急处置</span>
            <span v-show="securityConfig.data_safe" class="common-tag-square tag-color">数据安全</span>
            <span v-show="securityConfig.sensitive_all_lock" class="common-tag-square tag-color">敏感时期锁定</span>
        </div>
        <span v-else>-</span>
    </div>
</template>

<script>
    /**
     *  安全配置
     */

    import { ACCESS_STATUS } from 'const/setting';

    export default {
        props: {
            rowData: {
                type: Object,
                default: () => {}
            }
        },

        data () {
            return {
                ACCESS_STATUS
            };
        },
        computed: {
            securityConfig () {
                let vm = this;

                return vm.rowData.safe_config;
            },

            tagVisibal () {
                let vm = this;

                return vm.rowData.status === ACCESS_STATUS.haveAccess || vm.rowData.status === ACCESS_STATUS.waitAccess;
            },

            tagClass () {
                let vm = this;

                return {
                    'tag-gray': vm.rowData.status === ACCESS_STATUS.waitAccess
                };
            }
        }
    };
</script>

<style scoped lang="less">
    .tag-color{
        margin-right: 5px;
        border: 1px solid #bfe5ff;
        color: #4d8dd9;
        background: #e8f5ff;
    }
    .tag-gray{
        .tag-color{
            border: 1px solid #ddd;
            color: #ddd;
            background: #fff;
        }
    }
</style>
