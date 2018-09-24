# project
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>
    @font-face {
        font-family: 'icomoon';
        src:  url('fonts/icomoon.eot?9w70ab');
        src:  url('fonts/icomoon.eot?9w70ab#iefix') format('embedded-opentype'),
        url('fonts/icomoon.ttf?9w70ab') format('truetype'),
        url('fonts/icomoon.woff?9w70ab') format('woff'),
        url('fonts/icomoon.svg?9w70ab#icomoon') format('svg');
        font-weight: normal;
        font-style: normal;
    }

    *{
        margin: 0;
        padding:0;
    }
    .box{
        font-family: 'icomoon';

        width: 1200px;
        margin: 0 auto;
        background: black;
        border-radius:5px ;
        height: 675px;
        position: relative;
    }
    video{
        width: 100%;
        border-radius:5px ;
        position: absolute;
        display: none;
        z-index: 999;

    }

    .Control{
        width: 100%;
        height:34px;
        display: block;
        background: #d8d8d8;
        position: absolute;
        bottom:-33px;
        z-index: 999;
    }
    .Control .play{
        position: absolute;
        bottom: 0;
        width: 6.5%;
        height:100%;
        cursor: pointer;
        text-align: center;
        transition: 500ms;
    }

    .Control .play1::after{
          content: "\ea1c";
          font-size: 35px;
      }
    .Control .out::after{
        content: "\ea1d";
        font-size: 35px;

    }
    .Control .bar{
        width: 80%;
        height:100%;

        position: absolute;
        bottom: 0;
        left:6.5%;
    }
    .Control .bar .progress{
        width: 100%;
        height:60%;
        position: absolute;
        top:8px;
        background: darkgrey;
        border-radius: 10px;
        overflow: hidden;
    }
    .Control .bar  .progress .elapse{
        width: 0%;
        height:100%;
        position: absolute;
        top:0px;
        background: dimgray;
        border-radius: 10px;
        z-index: 999;
    }
    .Control .bar  .progress .bar1{
        width: 100%;
        height:100%;
        opacity:0;
        position: absolute;
        left:0;
        top:0;
        z-index:999;
        cursor: pointer;
    }

    .Control .barTime{
        width: 8%;
        height:100%;
        background: lightgrey;
        position: absolute;
        bottom: 0;
        left:86.5%;
        font-size: 8px;
        text-align: center;
        padding-top: 10px;
        box-sizing: border-box;
    }
    .Control .full{
        width: 5.5%;
        height:100%;
        background: lightslategrey;
        position: absolute;
        bottom: 0;
        right:0;
        text-align: center;
        padding-top: 8px;
        box-sizing: border-box;

    }
    .Control .fullon::after{
            content:"\e989";
        font-size: 20px;
        cursor: pointer;
        transition: 500ms;
    }
    .Control .fullon::after:hover{
        font-size: 25px;
    }
    .box .login{
        width: 100%;
        height: 100%;
        background: #d8d8d8;
        background-image: url("img/ploading.gif");
        background-repeat: no-repeat;
        background-position: 50% 50%;
        position: absolute;
        top:0;
        left:0;
    }
    video::-webkit-media-controls,
    video::-moz-media-controls,
    video::-webkit-media-controls-enclosure{
        display:none !important;
    }

    video::-webkit-media-controls-panel,
    video::-webkit-media-controls-panel-container,
    video::-webkit-media-controls-start-playback-button {
        display:none !important;
        -webkit-appearance: none;
    }
</style>
<body>
<div class="box">
    <div  class="login" id="login">

    </div>
    <video src="video/四月.mp4" id="video" > </video>
   <div class="Control">
       <div class="play play1" id="paly" title="播放">

       </div> <div class="bar" id="bar">
         <div class="progress" id="progress1">
             <div class="elapse">        <!--视频播放进度-->
             </div>
             <div class="bar1">

             </div>
         </div>

       </div>
       <div class="barTime" id="barTime">
          <span id="time1">00:00 /</span><span id="time2"> 00:00</span>
       </div>

       <div class="full fullon" id="full">

       </div>

   </div>
</div>

<script src="jquery/jquery.js"></script>

<script>


       var flag = true;
       var video = $('video')[0];

       video.controls = false;

       $(video).click(function () {
           if(flag){
               $("#paly").removeClass("play1").addClass("out");
               video.play();
           }else{
               $("#paly").removeClass("out").addClass("play1");
               video.pause();
           }
           flag = !flag;
       });         //点击视频就播放，在此点击就暂停同步到按钮

     $("#paly").click(function () {
         //如果是播放就暂停，如果是暂停就播放
          if(flag){
              $(this).removeClass("play1").addClass("out");
              video.play();
          }else{
              $(this).removeClass("out").addClass("play1");
              video.pause();
          }
         flag = !flag;

     });          //按钮点击视频就播放，在此点击就暂停

    $("#full").click(function () {
        video.requestFullScreen && video.requestFullScreen() ||
       video.webkitRequestFullScreen && video.webkitRequestFullScreen()||
        video.mozRequestFullScreen && video.mozRequestFullScreen() ||
        video.msRequestFullScreen && video.msRequestFullScreen();

    });            //全屏操作

    video.oncanplay = function () {

        setTimeout(function () {
            video.style.display = 'block';
            $('.login').css("display",'none');
            var total = video.duration;

            var hour = Math.floor(total/3600);   //计算小时
            hour = hour < 10 ? "0"+hour:hour;
            var minute = Math.floor(total%3600/60); //计算分钟
            minute = minute < 10 ? "0"+minute:minute;
            var second = Math.floor(total%60);   //计算秒
            second = second<10?"0"+second :second;

            $('#barTime #time2').text(" "+minute+":"+second);
        },1000)

    }         //视频加载完成后添加当前视频的时间

    video.ontimeupdate = function () {
        var current = video.currentTime;

        var hour = Math.floor(current/3600);   //计算小时
        hour = hour < 10 ? "0"+hour:hour;
        var minute = Math.floor(current%3600/60); //计算分钟
        minute = minute < 10 ? "0"+minute:minute;
        var second = Math.floor(current%60);   //计算秒
        second = second<10?"0"+second :second;
        $('#barTime #time1').text(minute+":"+second+" "+"/");

         var percent = current / video.duration;
         $(".elapse").css("width",percent * 100 +"%");



    }         //进度条


      


 $(".bar1").click(function (e) {
     var offset = e.offsetX;
     var percent = offset / $(this).width();
     var current = percent * video.duration;
     video.currentTime = current;
 })

 video.onended =function () {
     video.currentTime = 0;
     $("#paly").removeClass("out").addClass("play1");
 }


</script>
</body>
</html>
