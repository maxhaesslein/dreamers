# The Dreamers v.4

Live audioreactive visuals on a Raspberry Pi 4, written in Python3 and OpenCV. Currently a work in progress.

See [here](https://www.maxhaesslein.de/visual/objects/the-dreamers/) for a video of the previous version 3. This will be updated soon.

This is a personal project and may be a bit finicky to set up. Your best bet is a Raspberry Pi 4 with Raspberry Pi OS Lite 32-bit. You need to install some dependencies with:

## Installation

Install required dependencies with:

```bash
sudo apt install python3-opencv python3-pyaudio python3-aubio xserver-xorg xinit
```

Then download or clone this repo, extract it into `~/dreamers` and add videofiles in a `footage/` subfolder. Then start everything by calling the `start` file.

The videofiles should be h.264 encoded. The sweetspot is 1280x720px at 24/25/30 fps on a Raspberry Pi 4, without heavy effects. Don't mix resolutions or framerates.

## Microphone

You also need to make sure that you have a microphone connected and set up correctly. You may need to add a `~/.asoundrc` file, mine has this content for a cheap usb mic:

```
pcm.!default {
  type asym
  capture.pcm "mic"
}
pcm.mic {
  type plug
  slave {
    pcm "hw:1,0"
  }
}
```

## Config options

You also need to check all the options at the beginning of the `dreamers` file and change them accordingly.

You can also add custom font files (.ttf) into the `fonts/` subfolder and then set it in the `options` variable.

## starting over SSH

To allow starting over SSH, you may want to add this to your */etc/X11/Xwrapper.config*:

```
allowed_users=anybody # was: console
```

then you can call the *start* file over SSH

## Autostart

To autostart, copy the `dreamers.service` file into `~/.config/systemd/user/dreamers.service` and then activate it with:

```
sudo systemctl daemon-reload
systemctl --user enable dreamers.service
journalctl --user -u dreamers.service
```
