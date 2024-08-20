# M4L Note Repeat

This is a Max for Live device for Ableton Live. It allows you to map a key that will repeat the last note played, letting you play repetitions of that note very quickly by alternating between the normal key and the repeat key.

The purpose is to play note repetitions much smoother and with no gaps from having to lift and press the key again. It also allows for fast repetitions which would otherwise be more difficult on a keyboard.

In short, it's a performance MIDI device meant to make guitar-like note repetitions easier to perform on the keyboard.

## Context

This is part of a couple devices I wrote in the summer of 2021 when I first learned Max. 3 years later, I thought "hey, maybe I should upload these." Someone might find them useful, and they're an interesting showcase on my GitHub profile.

So they all have some properties in common:

1. They're a little old and a little late.
2. They're my first foray into Max, so they might not be the most well-designed devices.
3. The commits are added after the devices are already finished based on previous versions of the devices instead of being atomic commits making incremental changes.

## Purpose

As mentioned, this is part of a semi-collection of performance MIDI devices with the common purpose being to make guitar-like playing easier to accomplish on the keyboard, specifically note repetition and chord changes with no gaps.

Guitarist "just" strum a string again. Keyboardists have to lift the key in order to press it down again. We can use a sustain pedal, but then when we need to change notes, the pedal timing needs to be super precise not to slur the notes or miss one. Additionally, and this might just be a me problem, it's incredibly hard to do fast repetitions on piano in the style of tremolo picking.

This present us with two projects.

1. Modified sustain pedal that lets us change notes without slurring.
2. Map key to repeat last played note so we can tremolo pick same note by alternating hands.

This project is the latter.

For the former project, see [M4L Sustain Pedal](https://github.com/MedicodiBiscotti/m4l-sustain-pedal).

I have one other M4L project that isn't really related to guitar playing. It lets you convert CC from one controller to another (e.g. modulation -> soft pedal), [M4L CC Converter](https://github.com/MedicodiBiscotti/m4l-cc-converter).

## Known bugs

This device proved fiendishly complicated for a first project. There are lots of things to consider when taking natural playing style into account, specifically that people play legato with overlapping notes. Historically there were two big bugs, both of which were fixed with logic that I don't fully understand anymore. A third still remains.

- [x] 1. Lifting either "real" or "repeat" key would send note off message, cutting off the other one if playing had overlap between keys.
- [x] 2. Holding "repeat" key and changing "real" key would change the internally stored note value, preventing proper note off when releasing "repeat".
- [ ] 3. Hold one key (e.g. C), then another (D), then "repeat", then release first key. First key will not send note off message.

Bug 3 is a variation of bug 2 introduced as result of fixing the other bugs. The way it works is it compares whether the note value of the last pressed "real" and "repeat" key are the same. If they are the same, it shouldn't send note off. However, a combination like described above will produce a situation where those values are the same, but the released key was different, so it will erroneously not send a note off message.

This situation is very rare to come up naturally in normal playing, and I don't know how to fix it while still retaining the fixes to the previous bugs. For reference, I remembered there was still an edge case bug, but it took me mashing keys for it to show up so I could figure out what it was again. It's not likely to be that big of an issue.
