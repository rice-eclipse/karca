# `kARCA` Lessons Learned

## Design flaws

- No test pad on GND.

- 10V regulator just didn't work.
  We're not yet sure as to why, but I suspect that it's because the feedback resistors were too
  large.
  Independent of the cause of this issue, for some reason we managed to poke the PCB in just the
  right way to short 10V and 12V together (how?).

- The Raspberry Pi couldn't source enough current to run the heartbeat light.
  The heartbeat light requires 30 mA of current, while the Pi can only support 16 mA.
  Future designs will need to include a MOSFET for controlling this light.

## Future design improvements

- We can cut the 10V line and substitute using 12V to power the pressure transducers and amplifiers.

- Redo the external wiring to have ground always on pin 1.

- I'm not convinced the fuses are worth the added complexity.
  We can probably cut them with no loss.

- I suspect that we could get away with a smaller box.

- We could move to XT30s for outside power connections instead of using random connectors we found
  off of Amazon.

- We can swap to a different instrumentation amp which is actually in stock at JLC to prevent us
  from having to solder any SMD components.

- Not entirely necessary, but because JLC is assembling all our passives, we can use smaller passive
  footprints (0402, anybody?).

## Challenges encountered

This section is for problems we ran into that weren't exactly design flaws and would be hard to fix
in the first place.

- Thermal mass on the ground plane is so enormous that it takes several minutes to get a pin hot
  enough for soldering.

- We were able to reverse-bias the router and smoke it.
  There's no 100% fix (we would need a diode inside the router) but I think having a "main" fuse
  would be a good start.

- The 10V LDO was just a mess.
  If you cut it, you can skip it, maybe?

- Unable to talk to the pressure transducer ADC.
  Currently investigating this...

- Sometimes wiggling the battery connector temporarily disconnects it, causing a reboot.
  Shoving the wire deeper in seems to sometimes fix it.
  Maybe this is because it uses a very large gauge?
