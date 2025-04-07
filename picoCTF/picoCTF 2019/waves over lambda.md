# waves over lambda

## Task

*We made a lot of substitutions to encrypt this. Can you decrypt it? Connect with `nc jupiter.challenges.picoctf.org 39894`.*

### Hints

No hints were given.

## Input data

* Connection to remote server jupiter.challenges.picoctf.org on port 39894.
    After establishing connection we get the following message:
  
```console
$ nc jupiter.challenges.picoctf.org 39894
-------------------------------------------------------------------------------
uxkqzwjv naza sv exlz riwq - rzatlakue_sv_u_xbaz_iwpfhw_wqriuqjela
-------------------------------------------------------------------------------
ma maza kxj plun pxza jnwk w tlwzjaz xr wk nxlz xlj xr xlz vnsy jsii ma vwm naz vskc, wkh jnak s lkhazvjxxh rxz jna rszvj jspa mnwj mwv pawkj fe w vnsy rxlkhazskq sk jna vaw.  s plvj wuckxmiahqa s nwh nwzhie aeav jx ixxc ly mnak jna vawpak jxih pa vna mwv vskcskq; rxz rzxp jna pxpakj jnwj jnae zwjnaz ylj pa skjx jna fxwj jnwk jnwj s psqnj fa vwsh jx qx sk, pe nawzj mwv, wv sj maza, hawh msjnsk pa, ywzjie msjn rzsqnj, ywzjie msjn nxzzxz xr pskh, wkh jna jnxlqnjv xr mnwj mwv eaj farxza pa.
```

## Analysis

From a task description we can understand that we need to use **substitution cipher** to decrypt the received message.
For that we used [PLANETCALC](https://planetcalc.com/8047/).

## Solution

Decryption of the received message with the key `OZSXYFMBQTEUDCVJAKNIRLHPWG` gives us the following plaintext:

```
-------------------------------------------------------------------------------
CONGRATS HERE IS YOUR FLAG - FREQUENCY_IS_C_OVER_LAMBDA_AGFLCGTYUE
-------------------------------------------------------------------------------
WE WERE NOT MUCH MORE THAN A QUARTER OF AN HOUR OUT OF OUR SHIP TILL WE SAW HER SINK, AND THEN I UNDERSTOOD FOR THE FIRST TIME WHAT WAS MEANT BY A SHIP FOUNDERING IN THE SEA.  I MUST ACKNOWLEDGE I HAD HARDLY EYES TO LOOK UP WHEN THE SEAMEN TOLD ME SHE WAS SINKING; FOR FROM THE MOMENT THAT THEY RATHER PUT ME INTO THE BOAT THAN THAT I MIGHT BE SAID TO GO IN, MY HEART WAS, AS IT WERE, DEAD WITHIN ME, PARTLY WITH FRIGHT, PARTLY WITH HORROR OF MIND, AND THE THOUGHTS OF WHAT WAS YET BEFORE ME.
```

So, the flag is `FREQUENCY_IS_C_OVER_LAMBDA_AGFLCGTYUE`
