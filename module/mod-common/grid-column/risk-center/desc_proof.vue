<template>
    <div>
        <span v-if="proofUrl === ''">-</span>
        <span v-else class="proof-desc">举证图片：
            <svg class="icon proof-img" aria-hidden="true" @click="showImg">
                <use xlink:href="#icon-chakan-tupian"></use>
            </svg>
        </span>
    </div>
</template>

<script>
    /**
     * 描述column
     */

    export default {
        props: {
            proofUrl: {
                type: String,
                default: ''
            }
        },

        methods: {
            showImg () {

            }
        }
    };
</script>

<style scoped>
    .proof-desc{
        display: table;
    }
    .proof-img{
        vertical-align: top;
        font-size: 16px;
        cursor: pointer;
    }
</style>