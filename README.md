# ATV-GTG-for-x86_64
Android and Google TV ported from arm to x86 and x64 devices

all files needed can be found here: 
set up:
first flash the image using rufus. the device used to flash can be usb or hard drive. When booting into the TVOS you must always have it attatched to the system

the install as it stands would not store the user data. all changes would be lost on reboot.
to fix this download a data.zip file for the corresponding size. then unzip and paste into the system partition. This will store the data
issue: normally the system partition dosnetl let files bigger than 4 gb so to get data bigger than this in there you need to archive and paste each one into the partition then unzip inside the device.

if you intend on conneciting to HDMI and getting an audio output out of the device you are connected to you must follow the steps.
First in the TeraBox link you will find a folver at >ATV-GTG-for-x86_64 >audio. copy the audio folver onto another storage device. 
Boot into your device and give root permissions to the FX file manager and Smartpack kernel manager.
install the APK and analyse your device. scroll across to output, where you should find your output. You need to know which HDMI rail your output will be at. There should be a number by your HDMI output (0, 1 2)
this number corresponds to the script you will need.
Copy that script and the test.wav file onto your filesystem. put it somewhere acessible.
Using FX file manager, run the script with the corresponding hdmi output with root permissions (Execute with root). This script detects which PCM your putput is coming out of. WHen it has detected it should play a sound out of your output device. Make a ot of that PCM and the HDMI rail that you had sucess with.

change this line in the script you just executed:

setprop hal.audio.out.hdmi pcmC0D7p

the first number after setprop hal.audio.out.hdmi is the HDMI rail you are using. you shouldnt need to change this, but if you do it should correspond to the output of you HDMI you found when you installed the APK and navigated to output or sound.
The seccond sumber is the PCM that your sound comes out of. You should have made a note of it above. For example if your PCM 5 works and your HDMI is 2 your line should look like:

setprop hal.audio.out.hdmi pcmC2D5p

OPTIONAL:
when you know which pcm your outpuc comes out of, to speed up the script you can replace the for loop with the number you know works:

example: if PCM 5 works, you can change this:

for pcm in 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20; do
    echo "🔊 Testando PCM $pcm..." | tee -a "$LOG"
	
	if aplay -D plughw:0,$pcm "$AUDIO"; then
        echo "✅ PCM $pcm OK" | tee -a "$LOG"
    else
        echo "❌ PCM $pcm FALHOU" | tee -a "$LOG"
    fi
    
    echo "------------------------------" >> "$LOG"
    sleep 2
done

to this:

for pcm in 5; do
    echo "🔊 Testando PCM $pcm..." | tee -a "$LOG"
	
	if aplay -D plughw:0,$pcm "$AUDIO"; then
        echo "✅ PCM $pcm OK" | tee -a "$LOG"
    else
        echo "❌ PCM $pcm FALHOU" | tee -a "$LOG"
    fi
    
    echo "------------------------------" >> "$LOG"
    sleep 2
done

This just speeds up the Script. 

And you are done. your Android or Google TV device running on a x86 or x64 device should now be setup.

