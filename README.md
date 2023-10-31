# Problems with webRTC
A collection of issues limiting webRTC's adoption, specifically with a focus on video production

I'd like to note that some of these issues may have been resolved in more recent Chromium updates; some retesting is needed to confirm
Other issues are possibly perhaps  due to ignorance or error on my part, but I'm listing them anyways
    
* Switching to TURN fixes issues with bitrate with ~5% of users
    * Doesn't auto switch
    * Not clear why this happens; peer to peer Internet routingf issues; UDP throttling?
    * Seems to work for a second, then drops down to very low bitrates|
    * Adds a lot of cost to professional deployments where quality is important to paying customers
      
* Bitrates get stuck at very low rates sometimes
    * reconnecting fixes it; potentially restart-ice works
    * reconnecting every minute isn't practical
    * can get stuck after seconds or minutes
    * not easy to replicate
    * It's happening less these days than was common before 2023
        
* Cursor while screen recording
    * Screen capturing in Windows with browsers has the cursor show
    * the no cursor option in the screen capture settings does not hide the cursor
    * Forces users to have to use OBS to screen capture, which adds a lot of friction

* No application audio when window capturing in browser
    * Requires users to install to a virtual audio cable to capture window-specific audio sources
    * This adds friction and confusion to the experience
    * Capturing desktop audio then includes other sound effects
    * WHIP support in OBS might help here, but lacks flexible p2p support and data channels support
    
* Can't easily capture system audio on android or iOS (flutter); just mic audio (makes screen sharing pointless)

* Can't lock the resolution at a fixed resolution
    * content-hint a partial fix, but not fully
   
* Variable frame frames in screen sharing ; 57 /43 fps ; not 60.
    * Screen sharing in chrome doesn't seem to ever share 60-fps.
    * Window vs desktop vs tab results in different max frame rates
    
* No USB audio on Chrome Android or flutter
    * Chrome mobile and Flutter do not support USB audio sources;
    * Forces custom native apps that are not accessible to most developers
    * Firefox supports USB audio on mobile though?
    * Chrome has started to support USB, but its very flakey and buggy and often doesn't work
    * Chrome's USB audio support degrades the audio-quality; adds aec/denoise and can't be disabled?
 
* Firefox.
    * Firefox is a problem.
    * It can't keep up with features yet users still persist with it
    * Forcing high bitrates and controlling video settings on Firefox is limited

* No USB video at all in Flutter or mobile browser
    * USB video support (HDMI to USB, Webcams, etc) 
    * Forces users to buy very expensive hardware or use SRT/RTMP native apps instead
    * some proprietary native apps now have WHIP support; lack NAT traversal

* Changing tabs will smash any canvas or digital effects, killing frame rates, making effects while video streaming hard
    * work around is using a high precision audio timer, but it's not great and results in mismatched draw rates 

* No TCP to TCP mode for p2p
    * UDP throttling issues on some networks.
    * No guaranteed delivery method with peer to peer other than with data-channels
    * If UDP is required, it would be nice to have a guaranteed delivery mode that uses larger buffers

* H265 playback support in Chrome. 
    * Nvidia Jetson / iPhone can output H265 to WebRTC, but Chrome can't seem to play it back?
    * SRT supports H265 but has higher latency and no NAT traversial
    * AV1 isn't adopted by embedded systems and most mobile phones yet
    * Would be a boon to IRL streamers to have improved compression rates of H265

* Degoogled / security browsers / PFsense,etc
    * P2P / IP leaking is blocked, forcing TURN
    * built-in browser VPNs
    * UDP blocking
    * Challenging to detect without troubleshooting or pre-testing ahead of time
    * https://vdo.ninja/check for example tries to solve this; mainly an issue for p2p use cases

* Can't encode once and reuse encoded stream
    * Unless using web-codecs, can't encode once and stream to multiple via the browser
    * low bitrate streams (\~500kbps) would be well suited for an inbrowser SFU
    * maybe you can do this with insertable streams?

* Insertable stream support lacking with SFU deployments
    * e2e encryption, etc.
      

* Not sure how to include the playback URL of WHEP source when publishing with WHIP
    * Is this in the standard somewhere?
    * Currently I include "whep" in the WHIP answer's header response with the WHEP playback location
    * Having something like this being standard would be very helpful

* Cant change encoding presets in Browser like you can in OBS
    * increasing color depth would be nice
    * slower encoding presets to achieve better efficiency would be nice
    * maximum quality allowed is limited, blocking access to high bitrates

* Triggering keyframes is pretty hacky at the moment and not reliable

* No way to save stream direct to file without transcoding (lose quality/ higher CPU)
        
* Unless using Webcodecs, it's not feasible for one inbound encoded stream to be reused and forwarded to another user
    * This could open up the option of p2p2p
    * Transcoding lowers quality and uses a lot of CPU
       
* iOS devices force webKIT (safari) on pretty much all webviews and browsers, so there is no Chromium / Firefox option in reality

* Chrome gets stuck at 35 kbps from time to time; restart fixes it, but just for a few minutes.
   * this issue used to happen a lot more often than it does now

* Increasing playout buffer delay does not improve quality at all or very little
    * This is a huge issue. (or was?)
    * It does increase the delay, but not the quality
    * It's hard to reproduce synthentically, and it might have been improved actually in recent versions of Chrome?
    * Changes to chrome lately have resulted in a near zero jitter buffer? I'm a bit confused now

    Test setup for the above (assuming you also are employing lag + drop + out of order packet network distortion)
    ```
    publish: https://dev.versus.cam/evarate123/?view=57NXtzg&relay
    Play with 5s buffer: https://vdo.ninja/alpha/?view=57NXtzg&buffer=5000&relay
    Play with 100ms buffer: https://vdo.ninja/alpha/?view=57NXtzg&buffer=100&relay
    Play with no buffer: https://vdo.ninja/alpha/?view=57NXtzg&relay
    ```

* Changing play out buffer doesn't give control of underlying buffer ; just adds more. Makes play out quite variable
   * Changing buffer of video impacts audios buffer; not independent
   * Can't lower buffer or limit size for low latency
   * I need to retest this, as its been a while. Increasing the playout hint delay isn't increasing the jitter buffer anymore, but the video still is delayed?

* PCM has no FEC? Lots of clicking? 
   *  I might just not have this working right.

* PCM doesn't support 48khz stereo; just 32khz.
   *  48khz is supported with mono
   *  minor issue really

* MP4 recording options missing
   * webm support with OPUS, maybe PCM, but most users can't convert
   
* Video recordings (webM) of webRTC tracks are typically variable framerate/resolution; unusable by most video editors

* FFMpeg.js requires shared array / strict same origin, making it inaccessible
   * can't easily use it in browser to do remuxing into different pages or audio conversion

* Safari doesn't announce when a webrtc connection drops / ends / closes, so the peer continues to hold the connection open until there is a manual time out due to lack of ping or something.  (Haven't tested this in several months tho)

* No transparency support; webcam, transmission, or playback. WebM supports it though. Vtubers and green screens need it.
   * can't playback webRTC with alpha channels
   * can't bring in video sources with transparent sources
   * webcodecs don't yet support alpha channels
   
* 5.1 / 7.1 multichannel audio can't be published via the browser
   * stereo channel only with getUserMedia
   * 5.1 channel output mixing is hard to control; each 5.1 playback device handles it differently
   * browsers themselves handle this all wrnog
   * Multi channel audio capture doesn't seem to work in Chrome ?
   * 
* Multi channel audio playback up mixes into non-discrete channels with most multi channel audio output sources

* Surround sound headsets (logitech/corsair) cause Chrome to not work with webrtc audio playback
* 
* PCM stereo 2.0 audio only supports 32khz; not 44.1 or 48. `&audiocodec=pcm`

* Higher than 192khz sample rate on a playback device may break webaudio's mic
    * ie: you want a volume control in browser for the mic source
    * Firefox doesn't support re-sampling in webaudio source nodes; complicating this


* 32-bit audio playback devices browsers to not playback webRTC audio sometimes
    * This might be fixed now?   

* 110db resonance audio distortion peaks appear with PCM / OPUS audio via Chrome; less so with
    * This is reported by a user who is pretty picky, but noted distoration in the pcm/opus receieved

* No ASIO audio support by the browser

* More open source background removal AI models needed; hard to compete with tech giants
    * sound like background blur is coming, but green screen and digital backgarounds are needed

* No control over amount of echo cancellation or noise removal
    * some users want control over this. meh

* Auto gain with mics yet Yetis fight for volume control with chrome AGC
    * long standing issues with USB Yeti mics and a few other software-controlled gain on mics
    * windows users

* No way to increase maximum encoder quality target for high bitrate streaming
   * quality seems stuck at around medium-quality

* Vp8/vp9/h264 changes color/saturation - AV1 better? Depends on browser for encoding and playback.

* No way to control hardware encoder or check if available; forcing openh264 works
    * currently i'm guessing what the rules are here; under 360p software, but only if x is why and not z

* Hardware encoder in windows don't really reduce CPU load. Openh264 lower cpu than the hardware encoder
    * it might say the hardware encoder is being used, but the CPU load for webrtc is still high

* can't open a window at a specific resolution in Chrome; it opens to only 'roughly' the right resolution. Makes screen sharing at 720p fixed size not feasible.

* some users have reported the desire for B-frame support
