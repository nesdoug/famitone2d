update - 3/11/2022
-now compatible with 0cc-Famitracker and Dn-Famitracker
-fixed a bug in text2data (undefined behavior) 
 that might have caused incorrect output


This is basically the same as the text2data from Shiru's website...

https://shiru.untergrund.net/code.shtml

and works with the famitone2 v 1.15 files provided there.

Changes:
-added song names to file output
-added command line switch -allin prevents removal of unused instruments
-added command line switch -Wno suppresses warnings about unsupported effects

Bugfixes:
-multiple D00 effects (different channels) incorrect pattern length
-Bxx below D00 effect (different channels) incorrect pattern length
-Bxx loop back causing wrong instrument inserted at loop point

example of new switches:
text2data filename.txt -ca65 -allin -Wno


Troubleshooting

My song is silent? Possible reasons...

-you are calling a song # too high. The first song is 0, not 1
LDA #song_number before calling FamiToneMusicPlay

-you forgot to initialize the music code, make sure you 
send the correct address of the music data to the init code
LDX #<music_data ;low
LDY #>music_data ;high
LDA #1 ;NTSC = 1, PAL = 0
JSR FamiToneInit

-you forgot to call FamiToneUpdate once per frame



Sound Fx aren't working? Possible reasons...

-you are calling a sfx # too high. The first sfx is 0, not 1

-you are calling a sfx channel with the wrong value
FT_SFX_STREAMS	= 1...4 
LDX #channel, LDA #which_sfx, JSR FamiToneSfxPlay
but X should have a value 0,15,30, or 45

-forgot to turn on sfx... FT_SFX_ENABLE shouldn't be commented out

-forgot to initialize sfx, you need to send a pointer to the sfx data 
LDX #<sfx_data ;low
LDY #>sfx_data ;high
JSR FamiToneSfxInit

-using the old nsf2data (earlier than v1.15) which has a different
header system to the data.
