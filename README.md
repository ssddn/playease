﻿# playease.js 2.0

> [[domain] http://studease.cn](http://studease.cn/playease.html)  
> [[source] https://github.com/studease/playease](https://github.com/studease/playease)  
> [[中文] http://blog.csdn.net/icysky1989/article/details/75094205](http://blog.csdn.net/icysky1989/article/details/75094205)  
> 公众号：STUDEASE  
> QQ群：528109813  
> Skype: live:670292548  

This is a HTML5 Video Player for modern media streaming.  

## Roadmap

- Modules  
  - [x] SRC (original html5 media resources, eg. Ogg, Mpeg4, WebM, etc.)  
  - [x] FLV (http[s]/ws[s])  
  - [ ] FMP4 (http[s]/ws[s])  
  - [ ] DASH (LL-CMAF)  
  - [ ] HLS (LL-CMAF)  
  - [ ] RTC  
  - [ ] ~~Flash~~  

- Plugins  
  - [x] Poster  
  - [x] v2.1.00 - Danmu  
  - [x] Display  
  - [ ] AD  
  - [ ] Share  
  - [x] Logo  
  - [x] Controlbar  
  - [x] ContextMenu  
  - [ ] Playlist  

- Others  
  - [x] Synchronization of audio and video while the remote dropped some frames.  
  - [ ] Breakpoint download for http-flv playback (Send a HEAD request at first).  
  - [x] v2.1.43 - Buffer before starting to play.  
  - [x] v2.1.07 - Carry api.id while dispatching events.  
  - [ ] Reduce latency smoothly, due to cumulative ack of tcp.  
  - [x] v2.1.10 - Remove media segments within a specific time range to reduce memory usage.  
  - [x] v2.1.20 - Capture the current frame, and save as a picture.  

## Solutions

- [x] [Live Broadcast Solution](http://studease.cn/solutions.html).  
- [x] [Live Broadcast Solution on Mobile](http://studease.cn/solutions-mobile.html).  
- [x] [Monitoring Example](http://studease.cn/cms/index.html?n=9).  

## Example

Use the SDK directly:

```js
var utils = playease.utils,
    events = playease.events,
    Event = events.Event,
    IOEvent = events.IOEvent,
    MediaEvent = events.MediaEvent,
    index = 0;

var api = playease();
api.addEventListener(Event.READY, console.log);
api.addEventListener(Event.PLAY, console.log);
api.addEventListener(IOEvent.LOADSTART, console.log);
api.addEventListener(Event.WAITING, console.log);
api.addEventListener(IOEvent.STALLED, console.log);
api.addEventListener(IOEvent.ABORT, console.log);
api.addEventListener(IOEvent.TIMEOUT, console.log);
api.addEventListener(Event.DURATIONCHANGE, console.log);
api.addEventListener(Event.LOADEDMETADATA, console.log);
api.addEventListener(Event.LOADEDDATA, console.log);
api.addEventListener(IOEvent.PROGRESS, console.log);
api.addEventListener(Event.CANPLAY, console.log);
api.addEventListener(Event.PLAYING, console.log);
api.addEventListener(Event.CANPLAYTHROUGH, console.log);
api.addEventListener(IOEvent.SUSPEND, console.log);
api.addEventListener(Event.PAUSE, console.log);
api.addEventListener(Event.SEEKING, console.log);
api.addEventListener(Event.SEEKED, console.log);
api.addEventListener(Event.SWITCHING, console.log);
api.addEventListener(Event.SWITCHED, console.log);
api.addEventListener(Event.RATECHANGE, console.log);
api.addEventListener(Event.TIMEUPDATE, console.log);
api.addEventListener(Event.VOLUMECHANGE, console.log);
api.addEventListener(IOEvent.LOAD, console.log);
api.addEventListener(MediaEvent.INFOCHANGE, console.log);
api.addEventListener(MediaEvent.STATSUPDATE, console.log);
api.addEventListener(MediaEvent.SCREENSHOT, onScreenshot);
api.addEventListener(Event.ENDED, console.log);
api.addEventListener(Event.ERROR, console.error);
api.setup(container, {
    file: 'http://127.0.0.1/vod/sample.flv',
});

function onScreenshot(e) {
    var arr = e.data.image.split(',');
    var ret = arr[0].match(/^data:(image\/(.+));base64$/);
    if (ret === null) {
        console.error('The string did not match the expected pattern.');
        return;
    }

    var link = document.createElement('a');
    link.href = e.data.image;
    link.download = 'screenshot-' + utils.padStart(index++, 3, '0') + '.' + ret[2];
    link.click();
}
```

Use the built-in extendible UI framework:

```js
var UIEvent = playease.events.UIEvent;
var ui = playease.ui();
ui.addEventListener(UIEvent.SHOOTING, console.log);
ui.addEventListener(UIEvent.FULLPAGE, console.log);
ui.addEventListener(UIEvent.FULLSCREEN, console.log);
ui.addEventListener(UIEvent.RESIZE, console.log);
ui.setup(container, config);
```

## Add Callback

```js
api.onready = function(e) {
    // do something
};
```

Or:

```js
api.addEventListener('ready', onReady);

function onReady(e) {
    // do something
}
```

## API

### API of SDK

| Method | Arguments | Description |
| :--- | :--- | :--- |
| setup | container, config | Setup the SDK with the given configuration. |
| play | url = '', options = null | Plays the specified media file or live stream, or the current item of the sources if the url doesn't provided. |
| pause |  | Pauses playing. Calling this method does nothing if the video is already paused. |
| seek | offset | Seeks the keyframe (also called an I-frame in the video industry) closest to the specified location. |
| stop |  | Stops playing, sets the time property to 0. |
| reload |  | Releases all the resources, reloads the media file or live stream. |
| capture | width, height, mime | Captures the current frame, dispatches an screenshot event, and returns the image. |
| muted | status | Mutes or unmutes the audio/video elements, if status is a boolean. Otherwise, returns the current status. |
| volume | f | Sets volume, which in the range of 0 to 1, if f is a number. Otherwise, returns the current volume. |
| definition | index | Switches to the specified definition, if index is a number. Otherwise, returns the current definition. |
| element |  | Gets the current rendering element, such as video, flash, canvas, etc. |
| duration |  | Gets the media duration. |
| state |  | Gets the player state. |

### API of UI

Note: All of the SDK methods are also supported by UI.

| Method | Arguments | Description |
| :--- | :--- | :--- |
| setup | container, config | Setup the UI with the given configuration. |
| danmu | enable | Enables or disables the danmu plugin, if enable is a boolean. Otherwise, returns the current status. |
| shoot | text, data = null | Shoots the text with the data binded to. |
| displayAD | element | Displays the AD element. |
| removeAD |  | Removes the AD element. |
| fullpage | status | Requests or exits fullpage, if status is a boolean. Otherwise, returns the current status. |
| fullscreen | status | Requests or exits fullscreen, if status is a boolean. Otherwise, returns the current status. |
| resize |  | Resizes the player to fit to the parent node. However, if aspectratio is set, such as '16:9', the height will be calculated. |

## Events

The SDK supports Event and IOEvent. All of the SDK events will be forward to UI.

### Event

| Type | Properties | Meaning |
| :--- | :--- | :--- |
| READY |  | The ready event occurs when a module is ready. |
| PLAY |  | The play event occurs when it has been started or is no longer paused. |
| WAITING |  | The waiting event occurs when it stops because it needs to buffer the next frame. |
| DURATIONCHANGE | duration | The durationchange event occurs when the duration data is changed. |
| LOADEDMETADATA | metadata | The loadedmetadata event occurs when metadata has been loaded. |
| LOADEDDATA |  | The loadeddata event occurs when data for the current frame is loaded, but not enough data to play next frame. |
| CANPLAY |  | The canplay event occurs when the browser can start playing (when it has buffered enough to begin). |
| PLAYING |  | The playing event occurs when it is playing after having been paused or stopped for buffering. |
| CANPLAYTHROUGH |  | The canplaythrough event occurs when the browser estimates it can play through the specified media without having to stop for buffering. |
| PAUSE | timestamp | The pause event occurs when it is paused either by the user or programmatically. |
| SEEKING | timestamp | The seeking event occurs when the user starts moving/skipping to a new position. |
| SEEKED | timestamp | The seeked event occurs when the user is finished moving/skipping to a new position. |
| SWITCHING | index | The switching event occurs when the user starts switching to a different definition. |
| SWITCHED | index | The switching event occurs when the user is finished switching to a different definition. |
| RATECHANGE| rate | The ratechange event occurs when the playing speed is changed, which is invoked by the playbackRate method. |
| TIMEUPDATE | timestamp, buffered | The timeupdate event occurs when the playing position has changed. |
| VOLUMECHANGE | volume | The volumechange event occurs each time the volume has been changed. |
| ENDED |  | The ended event occurs when the media has reached the end. |
| ERROR | code, message | The error event occurs when an error occurred. |

### IOEvent

| Type | Properties | Meaning |
| :--- | :--- | :--- |
| LOADSTART |  | The loadstart event occurs when the browser starts loading. |
| STALLED |  | The stalled event occurs when the browser is trying to get media data, but data is not available. |
| ABORT |  | The abort event occurs when the loading is aborted. |
| TIMEOUT |  | The timeout event occurs when progression is terminated due to preset time expiring. |
| PROGRESS | loaded, total, buffer | The progress event occurs when the browser is downloading. |
| SUSPEND |  | The suspend event occurs when the browser is intentionally not getting media data. |
| LOAD |  | The load event occurs when an object has been loaded. |
| LOADEND |  | The loadend event occurs when a request has completed, whether successfully (after load) or unsuccessfully (after abort or error). |

### UIEvent

| Type | Properties | Meaning |
| :--- | :--- | :--- |
| SHOOTING | text, data | The shooting event occurs when a damnu message is shot. |
| FULLPAGE | status | The fullpage event occurs when the fullpage status is changed. |
| FULLSCREEN | status | The fullscreen event occurs when the fullscreen status is changed. |
| RESIZE | width, height | The resize event occurs when the UI is resized. |

### MediaEvent

| Type | Properties | Meaning |
| :--- | :--- | :--- |
| INFOCHANGE | info | The infochange event occurs when the meida info is changed. |
| STATSUPDATE | stats | The statsupdate event occurs when the media stats is changed. |
| SCREENSHOT | image | The screenshot event occurs when an image is captured. |

## Configuration

### Properties for SDK

```js
{
    airplay: 'allow',
    autoplay: false,
    dynamic: false,          // dynamic streaming
    bufferLength: 0.3,       // sec.
    file: '',
    lowlatency: true,        // ll-dash, ll-hls, ll-flv/fmp4 (auto reduce latency due to cumulative ack of tcp)
    maxBufferLength: 1.2,    // sec.
    maxPlaybackLength: 10,   // sec. for live mode only
    mode: 'live',            // live, vod
    module: '',              // SRC, FLV, FMP4, DASH, HLS, RTC, Flash
    muted: false,
    objectfit: 'contain',    // 'fill', 'contain', 'cover', 'none', 'scale-down'
    playsinline: true,
    preload: 'none',         // none, metadata, auto
    smooth: false,           // smooth switching
    volume: 0.8,
    loader: {
        name: 'auto',
        mode: 'cors',        // cors, no-cors, same-origin
        credentials: 'omit', // omit, include, same-origin
    },
    sources: [{              // ignored if "file" is presented
        file: '',
        module: '',
        label: '',
        default: false,
        mime: 'application/vnd.apple.mpegurl',
        thumbnails: '',
        textTracks: [{
            kind: '',
            file: '',
            default: true,
        }],
    }],
}
```

### Extension for UI

```js
{
    aspectratio: '',         // 16:9 etc.
    skin: 'classic',
    plugins: [{
        kind: 'Poster',
        file: 'images/poster.png',
        objectfit: 'fill',   // 'fill', 'contain', 'cover', 'none', 'scale-down'
        visibility: true,
    }, {
        kind: 'Danmu',
        speed: 100,
        lineHeight: 32,
        enable: true,
        visibility: true,
    }, {
        kind: 'Display',
        layout: '[Button:play=][Button:waiting=][Label:error=][Panel:info=][Panel:stats=]',
        ondoubleclick: 'fullscreen', // 'fullpage', 'fullscreen'
        visibility: true,
    }, {
        kind: 'AD',
        visibility: true,
    }, {
        kind: 'Share',
        visibility: true,
    }, {
        kind: 'Logo',
        file: 'image/logo.png',
        link: 'http://studease.cn/playease',
        target: '_blank',
        style: 'margin: 3% 5%; width: 36px; height: 36px; top: 0px; right: 0px;',
        visibility: true,
    }, {
        kind: 'Controlbar',
        layout: '[Slider:timebar=Preview]|[Button:play=Play][Button:pause=Pause][Button:reload=Reload][Button:stop=Stop][Label:quote=Live broadcast][Label:time=00:00/00:00]||[Button:report=Report][Button:capture=Capture][Button:mute=Mute][Button:unmute=Unmute][Slider:volumebar=80][Select:definition=Definition][Button:danmuoff=Danmu Off][Button:danmuon=Danmu On][Button:fullpage=Fullpage][Button:exitfullpage=Exit Fullpage][Button:fullscreen=Fullscreen][Button:exitfullscreen=Exit Fullscreen]',
        autohide: false,
        visibility: true,
    }, {
        kind: 'ContextMenu',
        visibility: true,
        items: [{
            mode: '',        // '', 'featured', 'disable'
            icon: 'image/github.png',
            text: 'github.com',
            shortcut: '',
            handler: function () { window.open('https://github.com/studease/playease'); },
        }],
    }]
};
```

## License

BSD 3-Clause License ([NOTICE](https://github.com/studease/playease/blob/master/NOTICE))
