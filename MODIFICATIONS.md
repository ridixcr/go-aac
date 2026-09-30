# Modified copy of github.com/skrashevich/go-aac v0.1.0

Original license: LGPL-3.0 (see `LICENSE`). Used via `replace` in `player/go/go.mod`.

## Changes

- `pkg/ics/ics.go` (PNS / `NoiseBT`): the pseudo-random generator used
  `state * 1015568748` (a multiple of 4, with no increment), so the state
  collapsed to 0 within a few iterations. The noise band energy ended up at 0,
  `sf / sqrt(0)` produced `Inf`, and the spectrum filled with `NaN`, which then
  propagated through the IMDCT overlap into the following frames. During playback
  this was heard as choppy audio that "bounced" between channels.
  It now uses a full LCG (`state*1664525 + 1013904223`) and guards the division
  when the energy is 0.
