# Copia modificada de github.com/skrashevich/go-aac v0.1.0

Licencia original: LGPL-3.0 (ver `LICENSE`). Se usa vía `replace` en `player/go/go.mod`.

## Cambios

- `pkg/ics/ics.go` (PNS / `NoiseBT`): el generador pseudoaleatorio usaba
  `state * 1015568748` (múltiplo de 4, sin incremento), por lo que el estado
  colapsaba a 0 en pocas iteraciones. La energía de la banda de ruido quedaba en 0,
  `sf / sqrt(0)` producía `Inf` y el espectro se llenaba de `NaN`, que además se
  propagaba por el overlap de la IMDCT a las tramas siguientes. En reproducción se
  oía como audio entrecortado que "rebotaba" entre canales.
  Ahora usa un LCG completo (`state*1664525 + 1013904223`) y protege la división
  cuando la energía es 0.
