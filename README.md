# SEG-Y Resample

A browser tool that interpolates post-stack SEG-Y files to a finer sample interval (for example 2 ms to 1 ms) and crops them to a time window. Files are read and processed locally in the browser and are not uploaded.

Live page: https://hbedle-subsurface.github.io/segy-resample/

## Use

1. Load one or more SEG-Y files. The table lists trace count, first CDP, samples per trace, sample interval, sample format and the dataset line from the textual header, and reports whether the files share the same geometry.
2. Choose the output sample interval and the start and end times.
3. Resample. Files are processed one after another in a background worker.
4. Check the output table, the section displays and the trace plots, then download each file.

A 2D line of 19,393 traces with 5,001 samples (about 390 MB) resamples from 2 ms to 1 ms over 0 to 2 s in under 10 seconds in a current desktop browser.

## Method

Each trace is Fourier transformed, the spectrum is extended with zeros above the Nyquist frequency of the input sampling, and the inverse transform gives the trace at the finer sample interval (band-limited, or sinc, interpolation). The input samples are reproduced in the output, and no frequency content above the input Nyquist frequency is created. Only the part of each trace needed for the chosen window, plus a 0.5 s cosine-tapered margin, is transformed.

## Output

- 32-bit IEEE float samples (format code 5), in the byte order of the input file.
- Binary header: sample interval, samples per trace and format code updated.
- Trace headers: copied from the input, with samples per trace and sample interval updated. When the start time is not zero, the delay recording time is increased by the start time. CDP numbers, coordinates and all other values are unchanged.
- Textual header: copied unchanged, so any sample interval or format it states refers to the original file.

Supported input: fixed-length SEG-Y (revision 0, 1 or 2) with IBM float, IEEE float, or 8-, 16- or 32-bit integer samples, big- or little-endian, with or without extended textual headers.

## License

Heather Bedle, University of Oklahoma. Licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).
