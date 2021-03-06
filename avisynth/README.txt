TCannyMod - Canny edge detection filter for Avisynth2.6.0 / Avisynth+


TCannyMod is an Avisynth filter plugin rewritten from scratch to Avisynth2.6
based on tcanny written by Kevin Stone(a.k.a. tritical).


Syntax:

    TCannyMod(clip, int "mode", float "sigma", float "t_h", float "t_l",
              bool "sobel", int "chroma", float gmmax)

    info:
        Builds an edge map using canny edge detection.

    parameters:

        clip - planar 8bit formats only.

        mode - sets output format (default = 0)

            0 - thresholded edge map (255 for edge, 0 for non-edge)

            1 - gradient magnitude map.

            2 - edge pixel only gradient direction map (non-edge pixels set to 0)

            3 - gradient direction map

                Gradient direction are normarized to 31, 63, 127 and 255.
                   31 = horizontal
                   63 = 45' up
                  127 = vertical
                  255 = 45' down

            4 - Gaussian blured frame.

        sigma - standard deviation of gaussian blur.
                0 means not bluring before edge detection.
                (0 <= sigma <= 2.83, default = 1.5)

        t_h - high gradient magnitude threshold for hysteresis (default = 8.0)

        t_l - low gradient magnitude threshold for hysteresis (default = 1.0)

        sobel - use Sobel operator instead of [1, 0, -1] for edge detection. (default = false)

        chroma - processing of chroma (default = 0)

            0 - not processing.

            1 - full processing

            2 - copy from source.

            3 - fill with 0x80(128). output is grayscale.

            4 - fill with 0.

        gmmax - used for scaling gradient magnitude into [0,255] for mode=1 (default = 255)
                 gmmax is internally set to 1.0 if you set it to < 1.0.

        opt - specify which CPU optimization are used (default = auto.)

             0 - forth SSE2 + SSE routine.

             1 - forth SSE4.1 + SSE2 + SSE routine.

             others - use AVX2 + FMA3 + AVX routine.

 ------------------------------------------------------------------------

    GBlur2(clip, float "sigma", int "chroma")

    info:
        Gaussian blur filter. Just an alias of TCannyMod(mode=4).

    parameters:

        clip - same as TCannyMod.

        sigma - same as TCannyMod. (default = 0.5)

        chroma - same as TCannyMod. (default = 0)

        opt - same as TCannyMod. (default = auto)

 ------------------------------------------------------------------------

    EMask(clip, float "sigma", float "gmmax", int "chroma", bool "sobel", int "opt")

    info:
        Generate gradient magnitude edge map. Just an alias of TCannyMod(mode=1).

    parameters:

        clip - same as TCannyMod.

        sigma - same as TCannyMod. (default = 1.5)

        gmmax - same as TCannyMod. (default = 50.0)

        chroma - same as TCannyMod. (default = 1)

        sobel - same as TCannyMod. (default = false)

        opt - same as TCannyMod. (default = auto)

------------------------------------------------------------------------



Note:

    - TCannyMod requires appropriate memory alignments.
      Thus, if you want to crop the left side of your source clip before this filter,
      you have to set crop(align=true).

    - Probabry, this filter is able to work on Avisynth+'s "MT_NICE_FILTER" mode.(from v1.0.0)


Requirements:

    Avisynth2.6.0/Avisynth+r2005 or greater.
    Microsoft Visual C++ 2019 Redistributable Package
    Windows Vista sp2 / 7 sp1 / 8.1 / 10
    SSE2 capable CPU


Changelog:

    1.0.0 (20160326):
        - Almost rewrite.
        - VS2013 to VS2015.
        - Add AVX2(64bit only)/SSE4.1(both 32bit and 64bit) support.
        - Change direction values from 1,3,7,15 to 31,63,127,255.
        - Reduce waste processes.

    1.1.0 (20160328):
        - Add EMask().
        - Implement simd non-maximum-suppression.
        - a bit optimized gaussian-blur/hysteresis.

    1.1.1 (20160330):
        - Add AVX2 support for 32bit.

    1.2.0 (2016):
        - Set filter mode as MT_NICE_FILTER on Avisynth+ MT.
        - Use buffer pool on Avisynth+ MT.
        - Disable AVX2/FMA3/AVX code when /arch:AVX2 is not set.
        - Disable AVX2/FMA3/AVX code on Avisynth2.6.

    1.3.0 (20160705)
        - Update avisynth.h to Avisynth+MT r2005.

    1.3.1 (20200513)
        - Add support for v8 interface
        - Remove /arch:AVX2 requirement for AVX2/FMA3/AVX code

    1.3.2 (20200515)
        - Change GBlur function name to GBlur2


Source code:

    https://github.com/chikuzen/TCannyMod/


Author  Oka Motofumi (chikuzen.mo at gmail dot com)
