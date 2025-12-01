# KOHCTPYKTOP
Remake of KOHCTPYKTOP, Zachtronics. WIP

## Done
* Drawing
* Copying&Cutting
* Save&Load (Only support vanilla save format. It's others' responsibility to support vanilla in their remakes, not me to support theirs)
* Different size of board (vanilla format also save size<s> but fail to read different sizes </s>)
* Simulation
* Level format

## Todo
* Specification
* Bottom display detach

# FAQ

## Why make another while there are already some remakes?

I don't like their operations. I put N-silicon, P-silicon, Metal, Via, Erase silicon and Erase Metal on (no|Ctrl|Shift) + (Left|Right) mouse, which feels at least better

## Why does an empty solution on lvl 1 result `40.084%(39%)`?

The original game floors the percentage multiple times. The first score is what you'll get if there's no rounding at all, and the second is what you'll get in game.

## Why squish image as base64 in the HTML?

Due to [security reason](https://developer.mozilla.org/en-US/docs/Web/HTML/How_to/CORS_enabled_image) and that this project is expected to run locally, it's easier to provide base64 in HTML to prove to browser that I have read access to the image file.

## Why is it NOT true that vanilla don't support save size?

Import this in Lvl 1: (Not good idea to place in FAQ though)

    eNrt2lsOgjAQBVCZ2x/24BLcgEtx/xsxEUui7cxAaavArZGfY6EvRkIn3MJ1fAzj
    fRCjdODgfNKzBUFySH6SHrr1iEwml3K4/NYb971OyEXmW43F5axWZjgsZJVjyXMs
    gFI7unZyk2N9aBM6OdTl8LqAwrGBeX53DzBZW4ob2L5LZJ9s3/5Szk7o+VzgXcNq
    a3YiH58DyGTy//A+wiqZTCYzrK58heDU3vQSgFNOJpPP97S6YPOJ+1NkMpnhdSUv
    zBXgUyqZTD5tlG2dAeA0rUuCgJALWeY5NlnKEgTESRAQJUHga7vdWA5qgsDsvTMA
    bJYjJghIqwQBESnIAOBOVh0Gh6WU57QruzaU2s61vT9hOBMKp2NTWDeaBmdYgM5j
    jiMuJrQ6ORq0/AkiJcRw

----

# Creating your level

Importing level is now available. Import the follow code to test it.

    Level;eNqrVirJzE1VsjIy0FEqV7Ky1FHKAJMFmXnFSlbR1Up5iSBpJUclHaUKoDIdpUowWVJZABQ21FFKAUoaGJSVKdXqwBU7QRSbgRWbwRQbYVesHebsjGw4XL0JVkVmqC4wqY2tBQD7NTQ1

A level always begin with `Level;` to tell from a solution. Following is the level compressed by `deflate` and encoded `base64`.

In this example, the uncompressed level is:

    {
        "time": 20,
        "w": 9,
        "h": 9,
        "pins": [
            {
                "name": "A",
                "x": 2,
                "y": 2,
                "type": 1,
                "d": "00vv"
            },
            {
                "name": "B",
                "x": 6,
                "y": 6,
                "type": 2,
                "d": "00vv"
            },
            {
                "name": "+VCC",
                "x": 2,
                "y": 6,
                "type": 4
            },
            {
                "name": "+VCC",
                "x": 6,
                "y": 2,
                "type": 4
            }
        ]
    }

Here `time` mean the simulating time length, `w` and `h` are width and height. `pins` are all pins:

* `x` and `y` its position;
* `name` its name which would display on circuit and scope;
* `type` describes its type:
* * `0` means decorative (typically named `N/C`. GND in [some other remake](https://github.com/pavel-krivanek/PharoChipDesigner) also use this type);
  * `1` means input;
  * `2` means output;
  * `3` is reserved (was for two-way pin but it's now implemented as two pins stacked together)
  * `4` is power (typically named `+VCC`).
* `d` describes the input/output signal:
* * Encoded in base-32, big-endian. This makes thing more clear because it typically only changes on multiple of 5.
 
The game would automatically cover 3x3 metal around pins. To cusomize default circuit(or maybe if you want to share solution meanwhile), append `;` and the exported solution at end.
