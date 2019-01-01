<template>
    <section class="process-wrap" :class="isVertical ? 'process-vertical' : 'process-level'">
        <div class="time-line"></div>
        <ul class="time-line-wrap">
            <li class="time-line-item"
                :class="itemClass(item, index)"
                v-for="(item, index) of dutyInfo.info"
                :key="item.title">
                <div class="item-desc">
                    <div class="process-title" :class="{'process-color-red': passCurrIndex(index)}">{{item.title}}</div>
                    <!--专家信息 -->
                    <div class="process-expert"
                         v-if="item.duty_step === DUTY_STEP.adult && item.detail.expert !== ''">（{{item.detail.expert}}）</div>
                    <!--发现篡改url-->
                    <div class="process-url common-ellipsis"
                         v-if="index === 0 && item.detail.url !== ''"
                         :title="item.detail.url">{{item.detail.url}}</div>
                    <!--查看替身-->
                    <div class="process-substitute" v-if="item.duty_step === DUTY_STEP.replace">已准备替身信息：
                        <br v-if="isVertical">{{item.detail.catch_time}}缓存页面，
                        <span class="common-color-blue">查看</span></div>
                    <div class="process-time" v-if="!passCurrIndex(index)">{{item.happen_time}}</div>
                </div>
            </li>
        </ul>
    </section>
</template>

<script>
    /**
     * 值守过程
     */

    import { DUTY_STEP } from 'const/home';

    export default {
        props: {
            dutyInfo: {
                type: Object,
                default () {
                    return {};
                }
            },
            cache: {
                type: Object,
                default () {
                    return {};
                }
            },
            isVertical: {
                type: Boolean,
                default: true
            }
        },

        data () {
            return { // TODO 缓存页面地址确认
                DUTY_STEP,
                currIndex: null
            };
        },

        methods: {
            itemClass (item, index) {
                let vm = this;

                if (item.duty_step === vm.dutyInfo.curr_step) {
                    vm.currIndex = index;
                }

                return {
                    'is-active': item.duty_step === vm.dutyInfo.curr_step,
                    'item-other': index > vm.currIndex
                };

            },

            passCurrIndex (index) {
                let vm = this;

                return vm.currIndex <= index;
            }
        }
    };
</script>

<style scoped lang="less">
    .process-wrap{
        position: relative;
    }

    .time-line {
        background: #DDDDDD;
    }

    .time-line-wrap {
        width: 100%;
    }

    .time-line-item {
        display: flex;
        position: relative;
        margin-bottom: 15px;
        align-items: center;
        &::before{
            content: '';
            display: inline-block;
            position: absolute;
            border-radius: 50%;
            width: 14px;
            height: 14px;
            background: #4A90E2;
            border: 2px solid #F7F8F9;
        }
        .item-desc{
            max-width: 270px;
        }

        .process-url{
            font-size: 12px;
            color: #4A90E2;
        }

        .process-time,
        .process-substitute{
            font-size: 12px;
            color: #BBBBBB;
        }
        .process-color-red{
            color: #EE5555;
        }
    }
    .time-line-item.is-active{
        &::before {
            left: -5px;
            background: #EE5555;
            border: 2px solid #F7F8F9;
        }

        &::after {
            content: '';
            display: inline-block;
            position: absolute;
            width: 18px;
            height: 18px;
            border-radius: 50%;