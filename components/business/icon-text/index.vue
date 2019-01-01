<template>
    <div class="icon-text-box">
        <svg class="icon" aria-hidden="true">
            <use :xlink:href="'#icon-' + iconName"></use>
        </svg>
        <span class="text-desc">
            <span>{{text}}</span>
            <slot></slot>
        </span>
    </div>
</template>

<script>
    /**
     * 字库和文字组合
     */

    export default {
        props: {
            iconName: {
                type: String,
                default: 'zengchang'
            },
            text: {
                type: String,
                default: ''
            }
        }
    };
</script>

<style scoped lang="less">
    .icon-text-box{
        line-height: 26px;
        .text-desc{
            font-size: 12px;
        }
    }
</style>