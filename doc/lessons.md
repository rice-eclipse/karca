# `kARCA` Lessons Learned

## Design flaws

- No test pad on GND.

- Incorrect resistor value for 10V LED.

## Future design improvements

- We can cut the 10V line and substitute using 12V to power the pressure transducers and amplifiers.

- Redo the external wiring to have ground always on pin 1.

- I'm not convinced the fuses are worth the added complexity.
  We can probably cut them with no loss.

- I suspect that we could get away with a smaller box.

- We could move to XT30s for outside power connections instead of using random connectors we found
  off of Amazon.

## Challenges encountered

This section is for problems we ran into that weren't exactly design flaws and would be hard to fix
in the first place.

- Thermal mass on the ground plane is so enormous that it takes several minutes to get a pin hot
  enough for soldering.
