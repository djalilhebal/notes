# Understanding USB Audio
#USB #audio #codec?

- [What is Sample Rate? | iZotope Pro Audio Essentials - YouTube](https://www.youtube.com/watch?v=fZzMXdxbOes) `[saved]`
    * KAITO: Made the concept of "sample rate" sorta click for me...
    It's the same as "audio rate" (as ffmpeg calls it), right?

- [ ] LEARN: Is "bit depth" the same as "Bit Resolution"?
    - [x] WATCH: [Bit Depth | iZotope Pro Audio Essentials - YouTube](https://www.youtube.com/watch?v=ubCMI3Jq6e4) `[saved]`
    - [ ] CHECK: https://en.wikipedia.org/wiki/Audio_bit_depth 

---

Using #ffmpeg:
```sh
# See "Can ffmpeg convert audio to raw PCM? If so, how?" | Stack Overflow
# https://stackoverflow.com/a/47688758

ffmpeg -i praise.webm -acodec pcm_s16le -f s16le -ac 1 -ar 44100 -vn -y praise.s16le.44100hz.raw

ffplay praise.s16le.44100hz.raw -f s16le -ar 44100
> [s16le @ 0x7f8978000b80] Estimating duration from bitrate, this may be inaccurate
> Input #0, s16le, from 'praise.s16le.44100hz.raw': sq=    0B f=0/0   
>   Duration: 00:03:23.72, bitrate: 705 kb/s
>     Stream #0:0: Audio: pcm_s16le, 44100 Hz, 1 channels, s16, 705 kb/s
>   19.66 M-A: -0.000 fd=   0 aq=   92KB vq=    0KB sq=    0B f=0/0   
```

---

> **2.2.6 Supported Formats**  
> The following paragraphs list all currently supported Type I Audio Data Formats.
> 
> 2.2.6.1 PCM Format
> The PCM (Pulse Coded Modulation) format is the most commonly used audio format to represent audio
> data streams. The audio data is not compressed and uses a signed two’s-complement fixed point format. It
> is left-justified (the sign bit is the Msb) and data is padded with trailing zeros to fill the remaining unused
> bits of the subframe. The binary point is located to the right of the sign bit so that all values lie within the
> range [-1,+1).
> 
> 2.2.6.2 PCM8 Format
> [...]
> 
> **2.2.6.3 IEEE_FLOAT Format**  
> [...]
> 
> **2.2.6.4 ALaw Format and μLaw Format**  
> Starting from 12- or 16-bits linear PCM samples, simple compression down to 8-bits per sample (one byte
> per sample) can be achieved by using logarithmic companding. The compressed audio data uses 8 bits per
> sample (bBitsPerSample = 8). Data is signed fixed point, left-justified in the subframe, Msb first. The
> compressed range is [-128,128]. The difference between Alaw and μLaw compression lies in the formulae
> used to achieve the compression. Refer to the ITU G.711 standard for further details.
>
> -- https://usb.org/sites/default/files/frmts10.pdf


## Android

- **Audio format**:
> Audio data format: PCM 16 bit per sample. Guaranteed to be supported by devices.
>
> -- https://developer.android.com/reference/android/media/AudioFormat#ENCODING_PCM_16BIT

- AOA2 uses PCM 16-bit 44.1KHz audio.

---

Also see my comment: https://github.com/rom1v/usbaudio/issues/28#issuecomment-774116207

```sh
# Manually start the Audio Accessory mode for my connected Samsung Galaxy device (in MTP mode).
# (The USB device VendorID:ProductID were obtained using `lsusb`.)
usbaudio --no-play --device 04e8:6860
```

---

END.
