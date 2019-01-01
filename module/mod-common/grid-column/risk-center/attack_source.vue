<template>
    <div :title="titleText">
        {{contentText}}
    </div>
</template>

<script>
    /**
     * 攻击源
     */

    export default {
        props: {
            rowData: {
                type: Object,
                default () {
                    return {};
                }
            }
        },

        computed: {
            titleText () {
                let vm = this;

                return vm.rowData.src_ip_set.map(item => {
                    return `${item.ip}（${item.country}）`;
                }).join('、');
            },

            contentText () {
                let vm = this,
                    source = vm.rowData.src_ip_set,
                    len = vm.rowData.src_ip_set.length,
                    text = `${source[0].ip}（${source[0].country}）`;

                if (len > 1) {
                    text += `等${len}个`;
                }
                return text;
            }
        }
    };
</script>

<style scoped>

</style>
