# ames
anki media extractor script(ames): Update anki cards with desktop audio and screenshots on GNU/linux

Ames automates the process of adding screenshots and desktop audio clips to your latest added Anki card; making immersion mining smoother and more efficient.

## Changes

I needed to beam up the most recent audio file from a directory to Anki, as I was not able to record the audio for a virtual machine in VirtualBox with FFMPEG, for whatever reason. Instead, I created a shared folder, installed ShareX on the Windows 10 VM, recorded audio from the guest following [this guide](https://animecards.site/setupsharex/), and then saved it to the shared folder so my host could access it. This was fine, but I had to copy the file every time to Anki, which took time, and I also couldn't use a .opus container because Anki seemingly didn't support it. I had to use .ogg instead, which is older and less efficient. 

ames has the handy ability to force Anki to use .opus through AnkiConnect. `ames -g` takes the `GRABDIR` parameter in the config to decide where to grab the latest `.opus`, `.ogg`, or `mp3` file from. By default, it's set to `$HOME/Downloads`, and the config contains an example of changing it to be `$HOME/Documents`.

I'm not sure what other use cases this could have, but if you needed to beam the latest audio file in a directory to Anki, this is the ames fork for you.

## Requirements
+ A bash interpreter
+ Anki and [AnkiConnect](https://ankiweb.net/shared/info/2055492159). *Note that Anki must be running*.
+ `pulseaudio` and `pactl`: detecting and recording from audio monitors
+ `ffmpeg`: encoding desktop audio
+ `maim`: screenshots
+ `xdotool`: detecting active windows
+ `libnotify`: sending notifications


## Installation
### General
1. Download the ames.sh script somewhere safe
2. Edit the script and change the first two lines to match the names of your Anki model image and audio fields.
3. Bind the following commands to any key in your DE, WM, sxhkd, xbindkeysrc, etc.
    * `sh ~/path/to/ames.sh -r`: press once to start recording, and again to stop and export the audio clip to your latest-created Anki card.
    * `sh ~/path/to/ames.sh -s`: prompt for an interactive screenshot selection
    * `sh ~/path/to/ames.sh -a`: repeat the previous screenshot selection. If there is no previous selection, default to -s.
    * `sh ~/path/to/ames.sh -w`: screenshot the currently active window (requires xdotool)

### Arch users
1. Install the `ames` package from the AUR
2. Copy the default config: `mkdir -p ~/.config/ames/ && cp /usr/share/ames/config ~/.config/ames/config`
3. Edit the config file however you like, but make sure your Anki image and audio fields are correct.
4. Bind the same commands however you want, but now the `ames` command should be in your PATH; so you can bind, for example, `ames -s` instead of `sh ~/path/to/ames.sh -s`
    
## Notes
+ You may also define config options in `~/.config/ames/config`. These must be bash variable declarations, with no spaces like in the script.
+ ames tries to pick the right output monitor automatically. If this doesn't work for you, you can first list monitor sinks with `pactl list | grep -A2 '^Source #'` and then redefine the `OUTPUT_MONITOR` variable in the script or a config file with the name of the correct sink.
+ By default, images are scaled to a height of 300px
+ Prefix your ames command with `LANG=ja` for japanese notifications to achieve *maximum immersion*
  
