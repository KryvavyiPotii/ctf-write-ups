# Mr-Worldwide

## Task

*A musician left us a **[message](https://jupiter.challenges.picoctf.org/static/d5570d48262dbba2a31f2a940409ad9d/message.txt)**. What's it mean?*

### Hints

No hints were given.

## Input data

* `message.txt` text file.

```
picoCTF{(35.028309, 135.753082)(46.469391, 30.740883)(39.758949, -84.191605)(41.015137, 28.979530)(24.466667, 54.366669)(3.140853, 101.693207)_(9.005401, 38.763611)(-3.989038, -79.203560)(52.377956, 4.897070)(41.085651, -73.858467)(57.790001, -152.407227)(31.205753, 29.924526)}
```

## Analysis

The flag looks like a number of GPS coordinates.
Here is a list of cities that correspond to these coordinates:\
(35.028309, 135.753082) - Kyoto\
(46.469391, 30.740883) - Odesa\
(39.758949, -84.191605) - Dayton\
(41.015137, 28.979530) - Istanbul\
(24.466667, 54.366669) - Abu Dhabi\
(3.140853, 101.693207) - Kuala Lumpur\
(9.005401, 38.763611) - Addis Ababa\
(-3.989038, -79.203560) - Loja\
(52.377956, 4.897070) - Amsterdam\
(41.085651, -73.858467) - Sleepy Hollow\
(57.790001, -152.407227) - Kodiak Island\
(31.205753, 29.924526) - Alexandria

## Solution

If we assume that the flag consists of the first letters of found places then we get the flag `picoCTF{KODIAK_ALASKA}`.
