<template>
    <span class="common-tag" :class="[color ? 'tag-color__' + color : '']">
        <slot></slot>
    </span>
</template>

<script>
    /**
     * 事件等级
     */

    export default {
        props: {
            color: {
                type: String,
                'default': ''
            }
        }
    };
</script>

<style scoped>
    .common-tag{
        text-align: center;
    }

    .tag-color__green {
        color: #0EB35B;
    }

    .tag-color__red{
        color: #EE5555;
    }

    .tag-color__blue{
        color: #3e80da;
    }
    .tag-color__grey{
        color: #C5C5C5;
    }
    .tag-color__deadly{
        color: #C33333;
    }
    .tag-color__high{
        color: #EE5555;
    }
    .tag-color__middle{
        color: #FF9800;
    }
    .tag-color__low{
        color: #FFC431;
    }
</style>