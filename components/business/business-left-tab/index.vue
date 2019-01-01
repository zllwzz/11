<template>
    <div class="box-upload" :style="uploadSize">
        <img class="img-container"  v-if="fileID"  :src="src"/>
        <span v-else>{{imgTips}}</span>
        <span class="delete-img" v-if="fileID" @click="deleteImg">删除</span>
        <div class="thumb" v-show="src" :style="uploadSize">
            <svg class="icon icon-thumb" aria-hidden="true">
                <use xlink:href="#icon-yemian"></use>
            </svg>
            <div class="progress" v-show="showProgress" :style="{'width': progressWidth + 'px', 'marginLeft': progressMargin + 'px'}">
                <div class="progress-bar progress-bar-info progress-bar-striped active"
                     role="progressbar"
                     aria-valuenow="50"
                     aria-valuemin="0"
                     aria-valuemax="100"
                     :style="{'width': progress + '%'}">
                </div>
            </div>
        </div>
        <div class="tip" v-show="!fileID" :style="uploadSize">
            <p class="upload-text">{{tip}}</p>
            <svg class="icon icon-tianjia" aria-hidden="true" >
                <use xlink:href="#icon-tianjia1"></use>
            </svg>
        </div>
        <div class="shade" :style="uploadSize" v-show="showProgress"></div>
        <div :id="uploadId"></div>
    </div>
</template>
<script>
    /**
     * @file 上传
     */

    import WebUploader from 'webuploader';
    import {uuid} from 'util/uuid';

    export default {
        data () {
            return {
                src: '',
                fileID: '',
                progress: 0,
                imgTips: '',
                showProgress: false,
                uploadId: uuid()
            };
        },
        props: {
            url: String,
            fileVal: {
                type: String,
                'default': 'file'
            },
            boxHeight: Number,
            boxWidth: Number,
            tip: {
                type: String,
                'default': '点击上传logo'
            }
        },

        computed: {
            progressWidth () {
                let vm = this;

                return vm.boxWidth * 3 / 4;
            },
            progressMargin () {
                let vm = this;

                return (vm.boxWidth - vm.progressWidth) / 2;
            },
            uploadSize () {
                let vm = this;

                return {
                    'height': vm.boxHeight + 'px',
                    'width': vm.boxWidth + 'px'
                };
            }
            
        },

        mounted () {
            let vm = this;

            vm.initUploader();
            vm.onFileAdd();
            vm.onUploadProgress();
        },

        methods: {
            initUploader () {
                let vm = this;

                vm.uploader = new WebUploader.Uploader({
                    auto: false,
                    swf: 'static/flash/uploader.swf',
                    fileVal: vm.fileVal,
                    server: vm.url,
                    pick: `#${vm.uploadId}`,
                    duplicate :true,
                    fileNumLimit: 1,
                    accept: {
                        title: 'Images',
                        extensions: 'gif,jpg,jpeg,bmp,png',
                        mimeTypes: 'image/gif,image/jpg,image/jpeg,image/bmp,image/png'
                    }

                });
            },
            onFileAdd () {
                let vm = this;

                vm.uploader.on('beforeFileQueued', (file) => {


                    vm.uploader.makeThumb(file, function (error, src) {
                        if (error) {
                            vm.imgTips = '不能预览';
                            return;
                        }
                        vm.src = src;
                    }, 246, 246);


                    vm.uploader.reset();

                    vm.fileID = file.id;
                });
            },
            ch