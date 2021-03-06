# FOMS [East] March 2019

## Monday 3/4

### Position Statement
Rob Walch - JW Player, Video Player

- Cross Browser/Device Compatibility
    - MSE
    - Metadata / TextTracks
- Video Analytics
- Web Application Performance

### Notes

#### iOS HLS hacks
- parse M3U8 with performance.getEntriesByName to sync with Safari playback
- "playImmediately(atRate rate: Float)" API https://developer.apple.com/documentation/avfoundation/avplayer/1643480-playimmediately
- iOS supports `video.getStartDate() + video.currentTine`
    - `video.getStartDate()` is invalid (`getTime() = NaN`) when PDT is not defined for first (all?) segment(s)

#### iOS Radar Issues to File
- MSE on iOS (memory efficient)
- TextTrack metatdata for manifest tags
    - Program Date Time
    - DateRange
    - X-Cue Out/In

#### HLS.js TODO
- Make HEAD (instead of GET) manifest requests, until you see a change, then:
    - request all the content
    - predict the next update based on when the header changed (etag)
- Eject the back-buffer 30 seconds back (configurable, defaults to Infinty)
- Increase the forward buffer length only when "waiting" events fire

### MSE [Track 1, Session 1]
- Feature
    - Chrome appendBuffer in a Worker
    - Buffer eviction / TimeRange change event notification
    - Media memory limit getter / notifications (be aware of cap)
    - Specify where all the keyframes are (couldn't you keep track of this?)
    - Ability to play-through gaps
        - Gap jumping
        - Frame dropping on append issues...
        - Change algorithm to only play through ranges with union of AV tracks
            - Video tag `buffered` exposes coalesced ranges based on config
            - SourceBuffered `buffered` is the truth
    - `appendStream` pipe ReadableStream into SourceBuffer

### Player Challenges [Track 1, Session 2]
- Supporting IE11 / Windows 7 / Flash: End support 2020/1/1
- Applications (pages) have their own service workers
    - A player library like shaka must have hooks / APIs to setup background fetch to work with your app
- Native players need offline storage. Web players do not.

### Low Latency
- Apple has no stance on the LHLS standard
- John Bartos gave a great demo comparing LHLS to LL-DASH with latency target of 1-5 seconds

### QoE
- Questions we need to answer
    - easy
        - was my video visible* ?
        - Did background tab affect performance?
        - Spanial? observer
    - hard
        - * was my video visible in an iframe
            - IntersectionObserver is tricky (force an update?)
        - why is the player / page slow?
            - resources, long task
        - get the framerate
            - `video.getVideoPlaybackQuality().totalVideoFrames / video.played.end(0)`
            - use frame accurate seeking to go to the next frame and check
        
- Conviva and Mux involved in CTA spec (Adobe has stepped in too)
- TODO: Create issues on repo for properties and events that would be hard to track
- TODO: Create a problem statement framerate (expose simple video fps) (*, 24, 29.*, 30, 48, 60, variable)

### Tools
- Media Lighthouse

### Video Element Extensibility
- ServiceWorker

### Media Stitching/Switching
- What am I playing and what can I stitch in

### Media Capabilities
- ...

### Live
- End-card standards (before/after event, event timings, single segment start card stitching)
- Timing Src https://webtiming.github.io/timingsrc/
