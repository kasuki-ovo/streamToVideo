<template>
	<div class="video-wrap">
		<div class="input-wrap">
			<input v-model="url" placeholder="请输入视频地址, 支持rtsp/rtmp格式"></input>
			<button @click="onPlay">播放</button>
		</div>
	  <video
	    muted="muted"
	    controls
	    width="100%"
	    height="600"
	    ref="video"
	  ></video>
	</div>
</template>

<script>
	import flvjs from 'flv.js';
	export default {
	  data() {
	    return {
	      flvPlayer: null,
	      url: 'rtmp://ns8.indexforce.com/home/mystream' // 这个是外网链接，速度比较慢，可以换cctv央视网: https://cctvwbndbd.a.bdydns.com/cctvwbnd/cctv1_2/index.m3u8?BR=single，m3u8格式视频：https://d2zihajmogu5jn.cloudfront.net/bipbop-advanced/bipbop_16x9_variant.m3u8
	    };
	  },
	  beforeDestroy() {
	    this.destoryVideo();
	  },
	  methods: {
			// 播放
			onPlay(){
				if(this.url && this.isPushUrl(this.url)){
					this.play()
				}
			},
			// 判断地址是否正确
			isPushUrl(url) {
			  const RegExp = /^((rtmp|rtsp|https):\/\/)(\S+\/)+\S+$/;
			  return RegExp.test(url);
			},
	    // 创建video
	    createVideo() {
	      if (flvjs.isSupported()) {
	        const videoElement = this.$refs.video;
	        this.flvPlayer = flvjs.createPlayer(
	          {
	            type: 'flv',
	            // isLive: false,
	            // hasAudio: false,
	            url: 'ws://localhost:8888/' + this.url
	          },
	          {
	            cors: true, // 是否跨域
	            // enableWorker: true, // 是否多线程工作
	            enableStashBuffer: false, // 是否启用缓存
	            // stashInitialSize: 128, // 缓存大小(kb)  默认384kb
	            autoCleanupSourceBuffer: true, // 是否自动清理缓存
	            fixAudioTimestampGap: false //false才会音视频同步
	          }
	        );
	        this.flvPlayer.attachMediaElement(videoElement);
	        this.flvPlayer.load();
	        this.flvPlayer.play();
	        // 报错重连
	        this.flvPlayer.on(flvjs.Events.ERROR, (errType, errDetail) => {
	          console.log('errorType:', errType);
	          console.log('errorDetail:', errDetail);
	          this.play()
	        });
	      }
	    },
	    // 销毁video
	    destoryVideo() {
				if(this.flvPlayer){
					this.flvPlayer.pause();
					this.flvPlayer.unload();
					this.flvPlayer.detachMediaElement();
					this.flvPlayer.destroy();
					this.flvPlayer = null;
				}
	    },
			// 重播/播放
			play(){
				if (this.flvPlayer) {
				  this.destoryVideo();
				}
				this.createVideo();
			}
	  }
	};
</script>

<style scoped>
	.video-wrap{
		width: 850px;
		margin: 0 auto;
		padding: 20px;
		border: 1px solid #ccc;
	}
	video{
		background-color: #000;
	}
	.input-wrap{
		margin-bottom: 10px;
		display: flex;
		align-items: center;
	}
	input{
		height: 32px;
		line-height: 32px;
		border: 1px solid #ccc;
		padding: 0 10px;
		flex: 1;
	}
	input:focus-visible{
		outline: none;
	}
	button{
		height: 32px;
		width: 60px;
		margin-left: 10px;
	}
</style>