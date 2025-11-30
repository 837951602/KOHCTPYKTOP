# KOHCTPYKTOP
Remake of KOHCTPYKTOP, Zachtronics. WIP

## Done
* Drawing
* Copying&Cutting
* Save&Load (Only support vanilla save format. It's others' responsibility to support vanilla in their remakes, not me to support theirs)
* Different size of board (vanilla format also save size but fail to read different sizes)
* Simulation
* Level format

## Todo
* Specification
* Bottom display detach

# Why make another while there are already some remakes?

I don't like their operations. I put N-silicon, P-silicon, Metal, Via, Erase silicon and Erase Metal on (no|Ctrl|Shift) + (Left|Right) mouse, which feels at least better


----

# Creating your level

Importing level is now available. Import the follow code to test it.

    Level;eNqrVirJzE1VsjIy0FEqV7Ky1FHKAJMFmXnFSlbR1Up5iSBpJUclHaUKoDIdpUowWVJZABQ21FFKAUoaGJSVKdXqwBU7QRSbgRWbwRQbIRSjqNYOc3ZGNh2uwQSrIjNUJ5jUxtYCAD60NKs=

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
                "d": "00vvv"
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
  * Due to how scoring works, it's suggested to extend output 5 cycles longer at end to match scoring in original game.
 
The game would automatically cover 3x3 metal around pins. To cusomize default circuit(or maybe if you want to share solution meanwhile), append `;` and the exported solution at end.
