<template>
    <div class="upload">
        <span v-show="title" class="pull-left upload-label">{{title}}</span>
        <input type="input"
               class="file-input"
               :placeholder="placeholder"
               v-model="filename"
               readonly
               :style="{width: inputWidth + 'px'}">
        <div :id="uploadId" class="upload-btn" ref="select-btn">{{btnText}}</div>
        <div class="progress"
             v-show="showProgress"
             :style="{
                'width': inputWidth + 'px',
                'margin': progressMargin
            }">
            <div class="progress-bar progress-bar-info progress-bar-striped active"
                 role="progressbar"
                 aria-valuenow="50"
                 aria-valuemin="0"
                 aria-valuemax="100"
                 :style="{'width': progress + '%'}">
            </div>
        </div>
    </div>
</template>
<script>
	/**
	 * 上传组件
	 */

	import WebUploader from 'webuploader';
	import {uuid} from 'util/uuid';
	import {CN} from 'const/number';

	export default {
		props: {
			propsUploadId: String,
			fileQueuedCallback: Function,
			url: String,
			gridId: String,
			btnText: {
				type: String,
				'default': '上传'
			},
			placeholder: {
				type: String,
				'default': '请选择文件'
			},
			acceptExt: {
				type: Array,
				'default': function () {
					return [];   //为空表示不限制上传文件类型
				}
			},
			limitSize: {
				type: [Number, String],
				'default': '',   //为''表示不限制大小
				validator: function (value) {
					return value === '' || value > 0;
				}
			},
			title: {
				type: String,
				'default': ''
			},
			inputWidth: {
				type: Number,
				'default': 260
			},
			progressMargin: {
				type: String,
				'default': '15px 0 0 74px'
			}
		},
		data () {
			return {
				filename: '',
				fileID: '',
				progress: 0,
				showProgress: false,
				uploadId: uuid()
			};
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
					server: vm.url,
					pick: `#${vm.propsUploadId || vm.uploadId}`,
					duplicate : true,
					fileNumLimit: 1
				});
			},
			onUploadProgress () {
				let vm = this;

				vm.uploader.on('uploadProgress', (file, percentage) => {
					vm.progress = Math.ceil(percentage * 100);
				});
			},
			onFileAdd () {
				let vm = this;

				vm.uploader.on('beforeFileQueued', (file) => {
					vm.uploader.reset();

					vm.fileSize = Math.ceil(file.size / CN.trillion);    //换算成M
					vm.fileID = file.id;
					vm.filename = file.name;
					vm.fileExt = file.source.ext;
				});

				vm.uploader.on('fileQueued', () => {
					vm.fileQueuedCallback && vm.fileQueuedCallback();
				});
			},
			checkExt () {
				let vm = this;

				if (vm.fileID) {
					if (!vm.acceptExt.length || vm.acceptExt.indexOf(vm.fileExt) > -1) {
						return true;
					}

					vm.$showWarn(`请上传${(vm.acceptExt.join(','))}格式的文件`);
					return false;
				}

				return true;
			},
			checkSize () {
				let vm = this;

				if (vm.fileID) {
					if (vm.limitSize === '' || vm.limitSize > vm.fileSize) {
						return true;
					}

					vm.$showWarn('文件大小超出限制');
					return false;
				}

				return true;
			},
			checkValid () {
				let vm = this;

				if (!vm.fileID) {
					vm.$showWarn('请选择文件');
					return false;
				}

				return vm.checkExt() && vm.checkSize();
			},
			offEventAndInit () {
				let vm = this,
					time;

				vm.uploader.off('uploadSuccess');
				vm.uploader.off('uploadError');

				vm.filename = '';
				vm.fileID = '';


				//当上传进度太快时，进度条会一闪而过，让进度条延迟消失
				time = setTimeout(() => {
					vm.showProgress = false;

					clearTimeout(time);
				}, 500);
			},
			upload (data) {
				let vm = this;

				vm.uploader.options = Obje