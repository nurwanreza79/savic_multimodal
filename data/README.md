# Dataset Notes

This folder is for **shareable input media only**.

## Recommended contents

- small demo files that are legally redistributable,
- examples required to run the qualitative reconstruction pipeline,
- placeholder structure for users who prepare the full dataset locally.

## Recommended demo file

```text
data/lionel.mp4
```

## Important policy

Do not upload raw data that cannot be redistributed. If the full training set is institution-owned, private, licensed, or otherwise restricted, describe:

- how the clips were prepared,
- the preprocessing steps,
- the temporal clip generation settings,
- the access conditions for qualified researchers.

## Clip generation settings used in the manuscript package

- frame size: `360 x 640`
- frames per clip: `12`
- chunk duration: `1.0 s`
- stride: `0.5 s`
- audio sample rate: `24 kHz`
- audio mode: `mono`

These settings should match the config files cited by the manuscript.
