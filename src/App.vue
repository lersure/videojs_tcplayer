<template>
  <div id="app">
    <div v-show="pushStream==false" class="box">

      <video id="video-box" class="video-js vjs-default-skin vjs-big-play-centered vjs-16-9 vjs-tech"
             controls
             preload="auto"
             webkit-playsinline="true"
             playsinline="true"
             x-webkit-airplay="allow"
             width='100%'
             ref='videoRef'
             x5-video-player-fullscreen="true"
             :poster="posterSrc">
        <source :src="videoSrc" type="application/x-mpegURL">
        <p class="vjs-no-js">不支持播放</p>
      </video>
    </div>


    <div v-show="pushStream==true">
      <div id="id-video"></div>
    </div>

  </div>
</template>

<script>

    // beforeDestroy(){
    //     const videoDom = this.$refs.videoRef;   //不能用document 获取节点
    //     videojs(videoDom).dispose();  //销毁video实例，避免出现节点不存在 但是flash一直在执行,也避免重新进入页面video未重新声明
    // }
    // allowsInlineMediaPlayback=YES
    //webview.allowsInlineMediaPlayback=YES
    var TcPlayer = require('../static/js/TcPlayer-module-2.2.2').TcPlayer

    export default {
        name: 'App',
        data() {
            return {
                pushStream: false,
                videoSrc: "http://1257142679.vod2.myqcloud.com/c9e1ad4evodcq1257142679/821e7e325285890800799662540/playlist.m3u8",
                posterSrc: 'https://mobisys-1300631855.cos.ap-chengdu.myqcloud.com/1584080738405',
                myPlayer: '',
                player: null
            }
        },

        mounted() {

         //   this.videoSrc = 'http://1257142679.vod2.myqcloud.com/c9e1ad4evodcq1257142679/821e7e325285890800799662540/playlist.m3u8'
            let that = this
            const myPlayerOptions = {
                bigPlayButton: true, // 是否显示播放按钮（这个播放按钮默认会再左上方，需用CSS设置居中）
                textTrackDisplay: true,
                posterImage: true,
                errorDisplay: true,
                controlBar: false,
                playbackRates: [0.5, 1, 1.5, 2],
                autoplay: false, // 不设置自动播放，若未true，则无法看到视频海报图片
                ControlBar: {
                    customControlSpacer: true
                }
            }
// 此处等待UI渲染进程结束，再执行js逻辑线程
            if (!that.pushStream) {
                setTimeout(() => {
                    that.myPlayer = videojs('video-box', myPlayerOptions)
                    that.myPlayer.src(that.videoSrc)
                    that.myPlayer.load(that.videoSrc)
                }, 500)
            }
            // setTimeout(() => {
            //     console.log("3秒后....")
            //     const videoDom = this.$refs.videoRef;
            //     videojs(videoDom).dispose();
            // that.pushStream = true
            // that.player = new TcPlayer('id-video', {
            //     "m3u8": 'http://1257142679.vod2.myqcloud.com/c9e1ad4evodcq1257142679/821e7e325285890800799662540/playlist.m3u8',
            //     "autoplay": false,  //iOS下safari浏览器，以及大部分移动端浏览器是不开放视频自动播放这个能力的
            //     "coverpic": {"style": "stretch", "src": ""},
            //     "controls": "system",
            //     "width": '100%',//视频的显示宽度，请尽量使用视频分辨率宽度
            //     "height": '100%',//视频的显示高度，请尽量使用视频分辨率高度
            //     "live": false,
            //     "wording": {
            //         2032: "请求视频失败，请检查网络",
            //         2048: "请求m3u8文件失败，可能是网络错误或者跨域问题"
            //     },
            //     "flash": true,
            //     "h5_flv": true,
            //     listener: function (msg) {
            //         //  console.log("type",msg.type)
            //         switch (msg.type) {
            //             case "error":
            //                 setInterval(() => {
            //                     that.player.load()
            //                 }, 2000)
            //                 break;
            //             case "play":
            //                 break;
            //             case "ended":
            //                 break;
            //             default:
            //                 break;
            //         }
            //     }
            // })
            //     window.tcplayer = that.player
            //     window.tcplayer.play()
            // }, 3000)

            //http://1257142679.vod2.myqcloud.com/c9e1ad4evodcq1257142679/821e7e325285890800799662540/playlist.m3u8


        },
        methods: {
            // beforeDestroy() {
            //     const videoDom = this.$refs.videoRef;   //不能用document 获取节点
            //     videojs(videoDom).dispose();  //销毁video实例，避免出现节点不存在 但是flash一直在执行,也避免重新进入页面video未重新声明
            // }
            videoClick(e) {
                console.log("点击屏幕", e)
            }
        }

    }

</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

  .vjs-big-play-button {
    left: 50% !important;
    top: 50% !important;
    margin-top: -1em;
    margin-left: -1.5em;
  }

  .video-js .vjs-big-play-button {
    border: unset !important;
    background-color: unset !important;
  }

  .video-js .vjs-big-play-button .vjs-icon-placeholder:before {
    font-size: 45px;
  }

  .vjs-poster {
    background-size: 100% 100% !important;
  }

  .box {
    margin-bottom: 20px;
  }

  .vjs-tech {
    pointer-events: auto;
  }

  /*.video-js .vjs-time-control{display:block;}*/
  /*.video-js .vjs-remaining-time{display: none;}*/
</style>
