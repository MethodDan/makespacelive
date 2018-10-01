# Overview

We host a variety of events at the [DoES Liverpool](https://doesliverpool.com) MakerSpace/Tech. Hub in Liverpool and have been thinking for some time of how we might live stream these.

Being a community of makers it seemed appropriate that we should look at what can be achieved with OpenSource / OpenHardware platforms for live streaming

This would then give us the options of

- streaming events at DoES Liverpool to the wider world (via Twitch, Facebook, Youtube Live, other)
- working with other groups to live stream their events to DoES Liverpool and elsewhere
- other use cases such as video conferencing, monitoring 3D print builds, building security etc.

Such a project may also help to foster links with other Makerspaces to create a "Federation of Makerspaces" where we interact with each other, are aware of each others' capabilities and events

The needs for such an infrastructure are

- very easy to live stream an event
- low cost hardware for live streaming
- easy to remotely access and update firmware on the hardware device
- has to look reasonably professional to viewers (minimal "buffering", options for cuts between multiple video sources.
- accessible to all on the internet
- potential for monetisation of events

## Device - Hardware

On the device side we are testing with the Raspberry Pi v3 which has Wifi and a GPU core for video encoding acceleration.
We are also testing out whether a Raspberry Pi Zero W is capable enough for livestreaming.

With this platform we can use either a PiCam or a webcam. We've been testing with a Logitech C270, Logitech C920, and PiCams.
Audio is supported from the Logitech webcams and we've successfully tested USB audio adaptors for use with PiCam-based systems

## Device - Firmware

We are using the [Resin.io](https://resin.io/how-it-works) cloud firmware management infrastructure to manage firmware updates and remote access to devices

The live streaming support on the client is based on [GStreamer1.0](https://gstreamer.freedesktop.org/) pipelines.

The Gstreamer pipeline is configured to stream audio/video as [H.264](https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC)/[FLV](https://en.wikipedia.org/wiki/Flash_Video) encoded [RTMP](https://en.wikipedia.org/wiki/Real-Time_Messaging_Protocol) streams to a configurable internet server

## Device - Enclosures

We're currently looking at a couple of Raspberry Pi Zero enclosure designs.

- PiZero design for PiCam [here](https://www.thingiverse.com/thing:1709013)
- Remixed design for 2xPiZero and PiCam for 360 video experiements [here](https://www.thingiverse.com/thing:3128603)

There are lots of others under various Creative Commons licenses which we are interested in hearing feedback on.

## Cloud RTMP - servers

We have been testing with the [Restream.io](https://restream.io) cloud platform which takes an RTMP stream and will then restream to other cloud endpoints such as [Twitch](https://www.twitch.tv), [Youtube](https://www.youtube.com/live), and [Facebook](https://live.fb.com)

The RTMP Stream URL and Stream Key are configurable and can be pointed to other endpoints easily

## Real time editing

We are currently working on VM images for 

- Local [Nginx](https://www.nginx.com) based RTMP restreaming and transcoding servers
- [OBS Studio](https://obsproject.com) VM image for real time a/v editing for live streams

# Maintainers & Contributers

- Alex J Lennon
- Dan Lynch
- Matthew Croughan
- Sarat Kumar

# Architecture

Architecture is currently very simple and consists of:

- [Dockerfile.template](./Dockerfile.template) which scripts creation of the Resin.io OS image (this can be recreated manually if you don't want to use Resin - see below)
- Support files from Resin.io [resin-io-connect](https://github.com/resin-io/resin-wifi-connect) project to support local Wifi AP configuration
- [stream.py](./stream.py) Python script which performs all streaming setup and runs the GStreamer1.0 pipeline

# Installation

## Standalone

[TBD]

## Resin.io Based

[TBD]

# Configuration

Configuration is achieved by setting environment variables prior to running `stream.py`

Currently supported variables are:

| Key                    | Description                              | Default Value            |
|------------------------|------------------------------------------| -------------------------|
| AV_STREAM_URL          | Target for RTMP live stream              | rtmp://10.0.31.212/live  |
| AV_STREAM_KEY          | Stream key if applcable                  |                          |
| AV_DISABLE_AUDIO       | Do not transmit audio                    | 0                        |
| AV_AUDIO_SAMPLING_RATE | Audio sample rate in Hz                  | 16000                    |
| AV_AUDIO_DEVICE        | ALSA audio hardware device               | 1                        |
| AV_AUDIO_BITRATE       | Encoded audio bitrate in kbps            | 128                      |
| AV_VIDEO_SOURCE        | Override the detected video source [TBD] |                          |
| AV_VIDEO_WIDTH         | Width of captured/encoded video          | 1280                     |
| AV_VIDEO_HEIGHT        | Height of captured/encoded video         | 720                      |
| AV_VIDEO_FRAMERATE     | Video frame rate in fps                  | 30                       |

An example setup script would be something like:

    export AV_STREAM_URL=my_stream_url
    export AV_STREAM_KEY=my_stream_key
    ./stream.py



