
## Common
### How secure is the CDNBye service?
CDNBye transfers data between peers using WebRTC data channel, 
which is secured by SCTP protocols and TLS encryption. Communication 
with the backend(tracker and signaling server) is done via HTTPS and 
secured WebSocket.

### Is it a must to pay for P2P services of CDNBye?
Currently, CDNBye is being commercialized, but still offers free services to small streaming websites.The charge model is as follows.
- P2P service will be no longer provided for those who do not bind domain names.
- A daily free 5GB of P2P traffic will be provided after the domain name is bound in the console (https://oms.cdnbye.com). Please note that it will be reset the next day.
- A daily free 20GB of P2P traffic will be provided after signing in at the console. Please note that it will be cleared the next day.
- P2P service will be stopped if the free quota is exceeded. If you want to continue to use it, you need to buy a traffic package.
- iOS and android SDK will not charge a fee for use of the service.

### How to buy a traffic package?
Log on to https://oms.cdnbye.com and click the `Traffic Package` on the left side. At present, Alipay and Paypal are supported to pay for the order. Please note that Alipay pays in RMB and Paypal in US dollars.

### How to deduct the traffic after buying the traffic package if multiple domain names/AppId are bound?
If you bind multiple domain names in one account, those which open P2P services share traffic packets together. You can save on packet consumption by shutting down P2P services of one or several domain names in `P2P Config`.

### Will the shutdown of P2P service affect the normal play of video?
No. Videos will still play normally without P2P streaming.

### Why does the official signalling service seem unstable at peak times？ 
It is not recommended to use the default signaling service. You can deploy your own signaling services or use third-party free signaling services. Please refer to [here](/en/signaling.md)

### Are there any requirements for websites/APP accessing P2P services? 
CDNBye does not provide P2P services for illegal content such as pornography.Please ensure that your website/APP complies with legal norms.

### Does CDNBye interfere with DRM(Digital Right Management)?
No. P2P transmission is activated only after the user is authorized 
by the publisher’s server. Importantly, decryption key isn't transferred 
through the P2P network. The segments shared between peers are the same as 
what peers receive from the CDN. Also, content is never uploaded or stored 
 in our server.
 
### Will CDNBye bring a delay to a user's stream?
No. The first Segment is always downloaded from CDN. Additionally, 
if a video segment cannot be loaded from peers within the timeout period, 
our scheduling algorithm will switch to CDN mode in time. That means CDNBye
will never bring a delay to a video stream.

## Web SDK
### How does Web SDK work?
CDNBye implements WebRTC datachannel to scale live/vod video streaming 
by p2p network using bittorrent-like protocol. The built-in 
scheduling algorithm determines whether a peer should load the next 
segment from p2p network or the publisher’s alternative delivery system.
hlsjs-p2p-engine is a hls.js wrapper provided by CDNBye for streaming HLS. 
Today CDNBye supports the vast majority of HTML5 players including Hls.js, 
JWPlayer, Video.js, Clappr, Flowplayer and TCPlayer.

### What happens if a user‘s browser doesn't support WebRTC?
If that happens, the user will seamlessly fallback to normal HTTP request 
without any side effect.

### Is it possible to run CDNBye on iOS in browsers?
No, web P2P solutions request Apple Media Source Extensions supported on iOS. This is the limitation of all existing P2P video streaming solutions.

## Native SDK
### Is Android TV supported?
Yes, Android TV is a supported platform for the SDK. CDNBye Android SDK works with any Android player although we recommend ExoPlayer which we've found to give good playback results.

### Is Apple TV supported?
No, Apple TV is not supported currently.

### Resolve some streams fail with HTTP response code 302 in exoplayer
It's working as intended that ExoPlayer [doesn't follow cross-protocol redirects]((https://github.com/google/ExoPlayer/issues/1190)) by default. You can enable following of cross-protocol redirects in ExoPlayer by passing `allowCrossProtocolRedirects=true` to the `DefaultHttpDataSourceFactory` constructor:
```java
DataSource.Factory dataSourceFactory =
    new DefaultHttpDataSourceFactory(
            Util.getUserAgent(this, "p2p-engine"),
            DefaultHttpDataSource.DEFAULT_CONNECT_TIMEOUT_MILLIS,
            DefaultHttpDataSource.DEFAULT_READ_TIMEOUT_MILLIS,
            true   /* allowCrossProtocolRedirects */
    );
```
For details, please see official [demo](https://github.com/cdnbye/android-p2p-engine).