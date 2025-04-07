# Glory of the Garden

## Task

*This **garden** contains more than it seems.*

### Hints

*What is a hex editor?*

## Input data

* `garden.jpg`.

## Analysis

### `exiftool`

Nothing interesting was found in metadata.

### `strings`

A string with the flag was found.

```console
$ strings garden.jpg
...
mjx/
s\]|."Ue
\qZf
Here is a flag "picoCTF{more_than_m33ts_the_3y3eBdBd2cc}"
```

## Solution

So, the flag is `picoCTF{more_than_m33ts_the_3y3eBdBd2cc}`
