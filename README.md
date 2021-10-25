# SignPin
> Better on-screen drawn signature verification by factoring in timing, angle & velocity.

SignPin is an algorithm for matching and verifying signatures drawn by hand on a digital input medium such as a touch screen, drawing pad or graphics tablet, while still accounting for personal variance while drawing each signature. This is mainly for detecting signature forgeries. It will specifically normalize drawn signature data over a `time` component, and `drawn_distance` component (i.e. it will look at how far along in the drawing of one's signature is each 2D point in the data both according to the distance traveled by the fingur / stylus and the time till the end of the signature drawing).

An additional metric it will look at is the number of times the finger / stylus has been lifted and re-placed on the drawing surface.

This is to explore an hypothesis that
- this would work with higher accuracy than fully vision based solutions
- this would work with higher efficiency than deep learning based approaches.

A typical tuple in the normalized data will look like:

![formula](https://render.githubusercontent.com/render/math?math=(x,%20y,%20d,%20t,%20c)_i)

Here ...

- `0 <= x <= 1`: The normalized x-coordinate.
- `0 <= y <= 1`: The normalized y-coordinate.
- `0 <= d <= 1`: Ratio - distance traveled by stylus : total distance traveled by stulys.
- `0 <= t <= 1`: Ratio - time elapsed since first touch : total time elapsed.
- `0 <= c`: The number of stylus has been re-placed on drawing surface since first touch.
- `0 <= i <= n`: Here, `n` is a preset size of tuple sets to standardize comparisons.

## Data Collection
We built a web based signature pad to be used on smartphone browsers. We are forwarding links to it to individuals who conset to
participating in the signature verification project.

### Methodology
- We forward an individual a link to the web pad.
- They sign their own signature 10 times on the pad.
- We store each signature on a database tagged with an anonymous ID.
- The individual is then encouraged to forge random anonymous signatures of other consenting individuals based on
  - A: A 2D image of the anonymous individual's signatue
  - B: An animation of the signature being drawn. (Only to be unlocked _after_ at least 3 2D forgeries had been done)

## Comparison Method
Comparison and/or similarity would be calculated via cosine distances between signatures,
optionally on smaller tuple set sizes to speed up comparisons for search purposes.

## Verification Method
Signatures will be verified against a limited set of reference signatures in the database based on cosine similarity
on an expanded tuple set. The accepable margin of error for each corresponding pair would be set based on the individuals
in variance between each reference signature.

## Use Cases

- Verification at Financial Institutions
- Confirming Receipt of High Priority Parcels
- Full Web Based Digital-Document Signing (with no physical cryptographic key containing device)
