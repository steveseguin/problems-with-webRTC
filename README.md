* Increasing playout buffer delay does not improve quality at all or very little
    * This is a huge issue.
    * It does increase the delay, but not the quality

    
* H265 playback support in Chrome. 
    * Nvidia Jetson / iPhone can output H265 to WebRTC, but Chrome can't seem to play it back. 
    * SRT supports H265 but has higher latency and no NAT traversial
    * AV1 isn't adopted by embedded systems and most mobile phones yet
    * Would be a boon to IRL streamers to have improved compression rates of H265
        
* Cursor while screen recording
    * Screen capturing in Windows with browsers has the cursor show
    * the no cursor option in the screen capture settings does not hide the cursor
    * Forces users to have to use OBS to screen capture, which adds a lot of friction
        
* No application audio when window capturing in browser
    * Requires users to install to a virtual audio cable to capture window-specific audio sources
    * This adds friction and confusion to the experience
    * Capturing desktop audio then includes other sound effects
    * WHIP support in OBS might help here, but lacks flexible p2p support and data channels support
        
* Variable frame frames in screen sharing ; 57 /43 fps ; not 60.
    * Screen sharing in chrome doesn't seem to ever share 60-fps.
    * Window vs desktop vs tab results in max frame rates
        
* No USB audio on chrome Android or flutter
    * Chrome mobile and Flutter do not support USB audio sources

    
* No USB video at all in Flutter or mobile browser
    * USB video support (HDMI to USB, Webcams, etc) 
    * Forces users to buy very expensive hardware or use SRT/RTMP native apps instead
    * some proprietary native apps now have WHIP support; lack NAT traversal
        
* Switching to TURN fixes issues with bitrate with \~5% of users
    * Doesn't auto switch
    * No clear why this happens; peer to peer networking issues?
    * Seems to work for a second, then drops down to very low bitrates
        
* Bitrates get stuck at very low rates sometimes
    * reconnecting fixes it
    * reconnecting every minute isn't practical
        
* No TCP to TCP mode for p2p
    * UDP throttling issues on some networks.
    * No guaranteed delivery method with peer to peer other than with data-channels
    * If UDP is required, it would be nice to have a guaranteed delivery mode that uses larger buffers
        
* Can't encode once and reuse encoded stream
    * Unless using web-codecs, can't encode once and stream to multiple via the browser
    * low bitrate streams (\~500kbps) would be well suited for an inbrowser SFU
        
* Unless using Webcodecs, it's not feasible for one inbound encoded stream to be reused and forwarded to another user
    * This could open up the option of p2p2p
    * Transcoding lowers quality and uses a lot of CPU
        
* Changing buffer of video impacts audios buffer; not independent


* Can't lower buffer or limit size for low latency


* Changing play out buffer doesn't give control of underlying buffer ; just adds more. Makes play out quite variable


* Can't easily capture system audio on android (flutter); just mic audio (makes screen sharing pointless)


* Chrome gets stuck at 35 kbps from time to time; restart fixes it, but just for a few minutes.


* No way to save stream direct to file without transcoding (lose quality/ higher CPU)


* PCM has no FEC? Lots of clicking


* PCM doesn't support 48khz stereo just 32khz


* MP4 recording options


* FFMpeg.js requires shared array / strict same origin, making it inaccessible


* No transparency support; webcam, transmission, or playback. WebM supports it though. Vtubers and green screens need it.


* More open source background removal AI models needed; hard to compete with tech giants


* Changing tabs will smash any canvas or digital effects, killing frame rates, making effects while video streaming hard


* No control over amount of echo cancellation or noise removal


* Auto gain with mics yet Yetis fight for volume control with chrome AGC


* No way to increase maximum encoder quality target for high bitrate streaming


* Vp8/vp9/h264 changes color/saturation - AV1 better? Depends on browser for encoding and playback.


* No way to control hardware encoder or check if available; forcing openh264 works


* Hardware encoder in windows don't really reduce CPU load. Openh264 lower cpu


* Some of these work with webcodecs