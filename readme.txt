Quick reproduction of issue reported at https://github.com/Zeugma440/atldotnet/issues/240

My Machine:

```
Processor	Intel(R) Core(TM) i7-1065G7 CPU @ 1.30GHz   1.50 GHz
Installed RAM	32.0 GB (31.6 GB usable)
System type	64-bit operating system, x64-based processor

Windows 10 Pro
Version	22H2
OS build	19045.3803
```

## The problem

I have an AIF file (that is seemingly corrupt), but ATL is "locking up" when we encounter it. The call to var track = new Track(path); will sit for hours before ultimately having to be killed; it never returns in my testing.
Environment

    Found in 4.33, but also reproducible with caec6e6
    Windows 10

## Details

I believe that the issue is around position 928534 in the audio file. In file AIFF.cs read( goes and calls into seekNextChunkHeader and the returned headers start to return a negative length. I believe that this effectively puts the read( method in an endless loop.