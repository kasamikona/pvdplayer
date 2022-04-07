# converting video formats (For Hasbro's VideoNow) I know the

## [#484](https://forum.videohelp.com/threads/123262/page17#post1149694) VideonowDude:

Here is the VideoNow Color file format:

Each audio track is taken as a byte sequence. The first track is special: I dont know why, but you have to copy it from a pre-recorded disc for the player to start up. After that, a track starts with N bytes of header, usually but not always zeros. N varies, I use 2500 in my encoder. N must be a multiple of 10. Then frames follow, each frame is 19600 bytes total, consisting of:

240 bytes of 0x81, 0xe3, 0xe3, 0xc7, 0xc7, 0x81, 0x81, 0xe3, 0xc7, 0xNN
120 bytes of 0x00, 0x00, 0x02, 0x01, 0x04, 0x02, 0x06, 0x03, 0xff, 0xNN etc (see a real disc for this pattern)
40 bytes of 9 0xFFs, one 0xNN (NN=audio byte, see later), some kind of sync
19200 Video of frame

Audio is every 10th byte, starting after the N bytes of header. It is stereo, 17640Hz, Stereo, 8-bit unsigned.

Video is 18 frames per second. There are 160 scanlines of 120 bytes per frame, with a nibble per 'pixel'. 'Pixels' are strange though, being RGBRGB for even scans, GBRGBR for odd ones (see good diagrams in earlier posts). If you take these as triangles, you get an effective resolution of 144 by 80.

The disc is run at exactly double speed. CD-Rs work great, CD-RWs fail badly. I haven't got a minidisc to work. The last track is never played, so is 4 seconds of black on the discs I looked at.
