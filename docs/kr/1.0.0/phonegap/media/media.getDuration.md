media.getDuration
=================

오디오 파일의 지속 시간을 반환한다.

    media.getDuration();


설명
-----------

`media.getDuration` 함수는 오디오 파일의 초단위 지속 시간을 반환하는 동기 함수이다. 만약 지속 시간을 모르면, -1이 반환된다.

지원하는 플랫폼
-------------------

- Android
- iOS
    
빠른 예제
-------------

        // Audio player
        //
        var my_media = new Media(src, onSuccess, onError);

        // 지속 시간을 얻는다.
        var counter = 0;
        var timerDur = setInterval(function() {
            counter = counter + 100;
            if (counter > 2000) {
                clearInterval(timerDur);
            }
            var dur = my_media.getDuration();
            if (dur > 0) {
                clearInterval(timerDur);
                document.getElementById('audio_duration').innerHTML = (dur) + " sec";
            }
       }, 100);


전체 예제
------------

        <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
                              "http://www.w3.org/TR/html4/strict.dtd">
        <html>
          <head>
            <title>Media Example</title>
        
            <script type="text/javascript" charset="utf-8" src="phonegap.js"></script>
            <script type="text/javascript" charset="utf-8">
        
            // PhoneGap이 로드되기를 기다린다
            //
            document.addEventListener("deviceready", onDeviceReady, false);
        
            // PhoneGap이 준비되면 호출된다
            //
            function onDeviceReady() {
                playAudio("http://audio.ibeat.org/content/p1rj1s/p1rj1s_-_rockGuitar.mp3");
            }
        
            // Audio player
            //
            var my_media = null;
            var mediaTimer = null;
        
            // 오디오를 재생한다.
            //
            function playAudio(src) {
                // src로부터 Media 객체를 생성한다.
                my_media = new Media(src, onSuccess, onError);
        
                // 오디오를 재생한다.
                my_media.play();
        
                // 매 초마다 my_media 위치를 갱신한다.
                if (mediaTimer == null) {
                    mediaTimer = setInterval(function() {
                        // my_media 위치를 얻는다.
                        my_media.getCurrentPosition(
                            // 성공 콜백
                            function(position) {
                                if (position > -1) {
                                    setAudioPosition((position) + " sec");
                                }
                            },
                            // 에러 콜백
                            function(e) {
                                console.log("Error getting pos=" + e);
                                setAudioPosition("Error: " + e);
                            }
                        );
                    }, 1000);
                }
            }
        
            // 오디오를 일시정지한다.
            // 
            function pauseAudio() {
                if (my_media) {
                    my_media.pause();
                }
            }
        
            // 오디오를 멈춘다.
            // 
            function stopAudio() {
                if (my_media) {
                    my_media.stop();
                }
                clearInterval(mediaTimer);
                mediaTimer = null;
            }
        
            // onSuccess 콜백
            //
            function onSuccess() {
                console.log("playAudio():Audio Success");
            }
        
            // onError 콜백
            //
            function onError(error) {
                alert('code: '    + error.code    + '\n' + 
                      'message: ' + error.message + '\n');
            }
        
            // 오디오 위치를 설정한다.
            // 
            function setAudioPosition(position) {
                document.getElementById('audio_position').innerHTML = position;
            }
        
            </script>
          </head>
          <body>
            <a href="#" class="btn large" onclick="playAudio('http://audio.ibeat.org/content/p1rj1s/p1rj1s_-_rockGuitar.mp3');">Play Audio</a>
            <a href="#" class="btn large" onclick="pauseAudio();">Pause Playing Audio</a>
            <a href="#" class="btn large" onclick="stopAudio();">Stop Playing Audio</a>
            <p id="audio_position"></p>
          </body>
        </html>
