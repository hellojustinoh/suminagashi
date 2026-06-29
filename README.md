# suminagashi

An interactive ink-marbling signature. Type your name and each keystroke drops pigment onto water; the ink blooms, displaces its neighbors, and melts together into a unique marbled pattern. Drag to stir, press Enter for a flourish, and the painting dries to a still, terminal state like real watercolor.

A single self-contained `index.html`. No build step, no dependencies.

## Run it

Open `index.html` in any modern browser, or serve the folder:

```sh
python3 -m http.server 4177
# then visit http://localhost:4177
```

Requires WebGL2 (a fallback message shows otherwise).

## How it works

The piece combines two ideas: the mathematical model of suminagashi (Japanese ink marbling) and a GPU fluid simulation.

- **Drop warp.** Each keystroke applies the classic marbling displacement (Lu et al.) directly to a dye texture: everything outside the drop is pushed radially outward while the interior fills with fresh pigment. Warp radii compose in quadrature, so a drop is applied as many small increments and you watch the ink soak outward over about a second, deforming whatever is already on the page.
- **Fluid sim.** A stable-fluids solver (advection, pressure projection, vorticity confinement) on float framebuffers carries the dye when you stir. Dragging injects velocity; Enter sweeps a current under the whole signature.
- **Terminal state.** Velocity dissipates quickly so currents die out, and once nothing has moved for a few seconds the simulation freezes entirely, the way a wet painting dries.
- **Rendering.** Beer-Lambert absorption over a paper color, with pigment grain, edge pooling, and paper texture.

Typing rhythm drives drop size (a pause loads the brush), and each character selects from a sumi / indigo / vermilion / pine / gold palette, so the same name marbles differently depending on who types it and how fast.

## Controls

| Action | Effect |
| --- | --- |
| Type | Drop ink along an advancing pen position |
| Space | A gentle current instead of a drop |
| Backspace | Clear water pushes the last drop's pigment back out |
| Enter | A sweeping flourish under the signature |
| Click | Drop ink anywhere |
| Drag | Stir the water |
| Clear / Save | Reset, or download the painting as a PNG |
