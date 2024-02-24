---
#**************************************
lang: en-AU
layout: page
social-share: true
comments: false

typora-copy-images-to: ../assets/img/${filename}
typora-root-url: ../
#**************************************

#*************************************
#fill this if you have renamed the page
redirect_from:
#*************************************

title: Listen to Stephen's Scanner
subtitle: Audio from Mt Sugarloaf
author: Stephen Schwetz
updated: 
cover-img: 
thumbnail-img: 
full-width: false

share-title: 
share-description: 
share-img: 

tags:
  -
---

<div class="outer-container">
    <div class="inner-container">
        <div>
            <!-- CHANGE THE SRC ADDRESS BELOW TO YOUR RDIO SCANNER INSTANCE -->
            <iframe src="https://radio.schwetz.au" width="410" height="608"></iframe>
        </div>

## Main screen

The main [Rdio Scanner](https://github.com/chuot/rdio-scanner) screen as three sections, the LED area, the display area and the controls area.

[![web app main screen](/assets/img/Using%20our%20Scanner/webapp-main.png)](https://github.com/chuot/rdio-scanner/blob/master/docs/images/webapp-main.png?raw=true)

### LED Area

![LED area](/assets/img/Using%20our%20Scanner/webapp-led.png)

The LED is illuminated when there is active audio and blinks if the audio is paused.

The colour is *green* by default but can be customised by the system or the talkgroup.

### Display Area

![display Area](/assets/img/Using%20our%20Scanner/webapp-display.png)

* First row
  * **09:26** - Current time
  * **Q: 27** - Number of audio files in the listening queue
* Second row
  * **SERAM** - System label
  * **Fire Dispatch** - Talkgroup tag
* Third row
  * **SG2** - Talkgroup label
  * **23:25** - The audio file recorded time
* Fourth row
  * **SERAM Regroupement 2** - Talkgroup full name
* Fifth row
  * **F: 770 506 250 Hz** - Call frequency on which the audio file was recorded. The name of the audio file will be displayed instead.
  * **TGID: 50002** - Talkgroup ID
* Sixth row
  * **E: 0** - Recorder's decoding errors
  * **S: 0** - Recorder's spike errors
  * **UID: 702099** - the unit ID or alias name
* History - The last five played audio files

> Note that you can double-click the display area to switch to full-screen display.

### Control area

![control Area](/assets/img/Using%20our%20Scanner/webapp-controls.png)

| Button                                                       | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![LIVE FEED](/assets/img/Using%20our%20Scanner/webapp-control-livefeed-partial.png) | When active, incoming audio will be played according to the active systems/talkgroups on the **SELECT TG** panel. The LED can also be *yellow* if playing audio from the archive while **LIVE FEED** is inactive, called **playback mode**. Disabling **LIVE FEED** will also stop audio playing and clear the listening queue. |
| ![HOLD SYS](/assets/img/Using%20our%20Scanner/webapp-control-holdsys.png) | Temporarily maintain the current system in live feed mode.   |
| ![HOLD TG](/assets/img/Using%20our%20Scanner/webapp-control-holdtg.png) | Temporarily maintain the current talkgroup in live feed mode. |
| ![REPLAY LAST](/assets/img/Using%20our%20Scanner/webapp-control-replay.png) | Replay the current audio from the beginning or the previous one if none is active. Press repeatedly to replay items displayed in the history section. For example, press three times to replay the third history item. |
| ![SKIP NEXT](/assets/img/Using%20our%20Scanner/webapp-control-skip.png) | Stop the audio currently playing and play the next one in the listening queue. This is useful when playing dull or encrypted sound. |
| ![AVOID](/assets/img/Using%20our%20Scanner/webapp-control-avoid.png) | Activate and deactivate the talkgroup from the current or previous audio. Pressing the **AVOID** button multiple times when on the same call will go through **AVOID** -> **AVOID 30M** -> **AVOID 60M** -> **AVOID 120M** where the last ones will be AVOID for a specific number of minutes. |
| ![SEARCH CALL](/assets/img/Using%20our%20Scanner/webapp-control-search.png) | Display the archived audio panel.                            |
| ![PAUSE](/assets/img/Using%20our%20Scanner/webapp-control-pause.png) | Stop playing queue audio. Useful if you have to answer the phone without losing what's queued up for playing. |
| ![SELECT TG](/assets/img/Using%20our%20Scanner/webapp-control-select.png) | Display the systems/talkgroups selection panel where you decide which audio you want to listen to in *LIVE FEED*mode, not in playback mode. |

## Select panel

![select panel](/assets/img/Using%20our%20Scanner/webapp-select.png)

Here you select which systems/talkgroups/groups you want to listen to while in *LIVE FEED* mode. Note that the selection panel has no effect if you play calls from the search panel.

### Groups section

![groups section](/assets/img/Using%20our%20Scanner/webapp-select-groups.png)

The first section concerns group selection. This section can be disabled from the configuration file, but is active by default. Each button has three states:

| Button                                                       | State                                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![ON](/assets/img/Using%20our%20Scanner/webapp-select-group-on.png) | All talkgroups, from any systems, that correspond to this group are actives. If you press it while it is active, it will make these talkgroups inactives. |
| ![OFF](/assets/img/Using%20our%20Scanner/webapp-select-group-off.png) | All talkgroups, from any systems, that correspond to this group are inactives. If you press it while it is inactive, it will make these talkgroups actives. |
| ![PARTIAL](/assets/img/Using%20our%20Scanner/webapp-select-group-partial.png) | Some talkgroups, from any systems, that correspond to this group are actives and some are inactives. If you press it while it is active, it will make all these talkgroups active. |
| ![ALL OFF](/assets/img/Using%20our%20Scanner/webapp-select-all-off.png) | Make every groups inactives, thereby disabling all talkgroups. |
| ![ALL ON](/assets/img/Using%20our%20Scanner/webapp-select-all-on.png) | Make every groups active, thereby enabling all talkgroups.   |

### Systems section

![systems section](/assets/img/Using%20our%20Scanner/webapp-select-system.png)

This is much like the group section, but for each system. There is also just two states for each button:

| Button                                                       | State                                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![ON](/assets/img/Using%20our%20Scanner/webapp-select-system-on.png) | The talkgroup from this system is active. If you press it while it is active, it will make the talkgroup inactive. |
| ![OFF](/assets/img/Using%20our%20Scanner/webapp-select-system-off.png) | The talkgroup from this system is inactive. If you press it while it is inactive, it will make the talkgroup active. Note that if the LED is flashing, it means that the talkgroup is avoided for a limited period. |
| ![ALL OFF](/assets/img/Using%20our%20Scanner/webapp-select-all-off.png) | Make inactive every talkgroups from this system.             |
| ![ALL ON](/assets/img/Using%20our%20Scanner/webapp-select-all-on.png) | Make active every talkgroups from this system.               |

## Search panel

[![search panel](/assets/img/Using%20our%20Scanner/webapp-search.png](https://github.com/chuot/rdio-scanner/blob/master/docs/images/webapp-search.png?raw=true)

### List section

![search list](/assets/img/Using%20our%20Scanner/webapp-search-list.png)

This section presents the list of archived audio files stored in the database. Depending on the *LIVE FEED* mode you are at, the replay function behaves differently:

| Mode                                                         | Function                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![LIVE FEED ON](/assets/img/Using%20our%20Scanner/webapp-control-livefeed-on.png) | while **LIVE FEED** is active, press the **PLAY** button to play this audio file. If another audio file is playing, it will be stopped and the one you just selected will be played instead. After playback finishes, the listening queue will resume as normal. |
| ![LIVE FEED PARTIAL](/assets/img/Using%20our%20Scanner/webapp-control-livefeed-partial.png) | This is the **playback mode** which can be activated only if **LIVE FEED** is inactive. Press the **PLAY** button to play this audio file. After playback is finishes, the next audio file from in list is played. While the audio is playing, pressing the **STOP** button will cancel the playback mode. |

At the bottom left of this section is the toggle switch which allows you to toggle between play buttons and download buttons. The later ones allows you to download locally the audio file.

At the bottom right of this section is the *paginator* to browse the whole database. This *paginator* is disabled while in playback mode.

> Note that playback mode always requires your web app to be online. It is called like that because it plays archived audio files from the database.

### Filters section

![filters section](/assets/img/Using%20our%20Scanner/webapp-search-filters.png)

This is the section where you filter the archived audio to a specific date, system, talkgroup, groups and tags. You can also change the sort order.

There is also a small slider that changes the **PLAY** buttons to **DOWNLOAD** buttons. This allows you to download audio files individually regardless of the playback mode you use.

> Note that if you change any filter while in playback mode, it will deactivate it.

  </div>
</div>