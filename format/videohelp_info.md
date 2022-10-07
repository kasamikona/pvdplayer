## [Thread](https://forum.videohelp.com/threads/123262-converting-video-formats-(For-Hasbro-s-VideoNow)-I-know-the)  converting video formats (For Hasbro's VideoNow) I know the

## [#467](https://forum.videohelp.com/threads/123262-converting-video-formats-(For-Hasbro-s-VideoNow)-I-know-the/page16#post1137756) GregSmith:
max,

you are very far along. i think i know something that may help you.

lcd displays are actually collections of dots - red, green, and blue dots. They are arranged in a pattern like this[^gmissing] ...
[^gmissing]: Diagrams show every other row shifted right and starting with B .. , really they are shifted left and start with G B ..

```
R G B R G B R G B R G B
.B R G B R G B R G B R G
R G B R G B R G B R G B
```

do you see how the pixels make up little triangles of RGB? That is why you are getting those strange patterns in your image. you will have to arithmetically combine the pixels so that three pixels are combined into one on the screen. let me see if i can draw this ...

```
column
..0   1   2   3   4   5

R   G   B   R   G   B
.\ / \ / \ / \ / \ / \
..00  01  02  03  04  05 - row 0
..|   |   |   |   |   |
..B   R   G   B   R   G
..|   |   |   |   |   |
..10  11  12  13  14  15 - row 1
./ \ / \ / \ / \ / \ /
R   G   B   R   G   B
.\ / \ / \ / \ / \ / \
..20  21  22  23  24  25 - row 2
..|   |   |   |   |   |
..B   R   G   B   R   G
..|   |   |   |   |   |
..30  31  32  33  34  35 - row 3
./ \ / \ / \ / \ / \ /
R   G   B   R   G   B
```

The pixels on your screen are numbered 00, 01, 02, etc...  
So, to create pixel 00 you would combine the RGB elements that are indicated by the lines.

I hope this makes some sense.

greg

---

## [#484](https://forum.videohelp.com/threads/123262-converting-video-formats-(For-Hasbro-s-VideoNow)-I-know-the/page17#post1149694) VideonowDude:

Here is the VideoNow Color file format:

Each audio track is taken as a byte sequence. The first track is special: I dont know why, but you have to copy it from a pre-recorded disc for the player to start up. After that, a track starts with N bytes of header, usually but not always zeros. N varies, I use 2500 in my encoder. N must be a multiple of 10. Then frames follow, each frame is 19600 bytes total, consisting of:

240 bytes of 0x81, 0xe3, 0xe3, 0xc7, 0xc7, 0x81, 0x81, 0xe3, 0xc7, 0xNN  
120 bytes of 0x00, 0x00, 0x02, 0x01, 0x04, 0x02, 0x06, 0x03, 0xff, 0xNN etc (see a real disc for this pattern)  
40 bytes of 9 0xFFs, one 0xNN (NN=audio byte, see later), some kind of sync  
19200 Video of frame[^videoofframe]
[^videoofframe]: *bytes of video frame data

Audio is every 10th byte, starting after the N bytes of header. It is stereo, 17640Hz, Stereo[^stereomono], 8-bit unsigned.
[^stereomono]: Found to be 35280Hz mono, explains lack of any obvious left-right sync mechanism.

Video is 18 frames per second. There are 160 scanlines of 120 bytes per frame, with a nibble per 'pixel'. 'Pixels' are strange though, being RGBRGB for even scans, GBRGBR for odd ones (see good diagrams in earlier posts). If you take these as triangles, you get an effective resolution of 144 by 80.[^pixelres]
[^pixelres]: Pixels are laid out in a 216x160 matrix, with every other row shifted by half a pixel. Interpreting groups of 3 as RGB triplets gives 144x80.

The disc is run at exactly double speed. CD-Rs work great, CD-RWs fail badly. I haven't got a minidisc to work. The last track is never played, so is 4 seconds of black on the discs I looked at.
