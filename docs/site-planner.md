# The Site Planner walkthrough

<span class="status status--complete">Tested 11 May 2026, three days before the walk</span>

<p class="epigraph">A step-by-step record of how we used the Meshtastic Site Planner to predict radio coverage from October Books, with the actual coverage map produced. Anyone with a laptop can reproduce this in about 15 minutes.</p>

The [Meshtastic Site Planner](https://site.meshtastic.org/) is a free open source tool, maintained by the Meshtastic project, that predicts the radio coverage of a LoRa transmitter using terrain elevation data and the ITM/Longley-Rice propagation model. It is a useful pre-deployment check, with important caveats noted at the end of this page.

## What the prediction looks like

This is the coverage map produced for a transmitter at October Books, run on 11 May 2026:

![Meshtastic Site Planner coverage prediction for October Books, Southampton. Yellow at centre shows strong signal, fading through orange and magenta to dark purple at the edges, approximately 1.5 to 2 km radius.](assets/images/mesh-site-planner.png)

The yellow core at centre is October Books on Portswood Road, where the signal is strongest. The coverage fades outward through orange, magenta, and dark purple to the simulation edge at roughly 2 km radius. The route between October Books and Highfield campus (1.2 km north of the transmitter) is visible along the centre of the map: it passes from the bright yellow core, through pink and magenta along Portswood Road, into the darker purple at the northern edge as it approaches Highfield.

The full colour scale runs from -130 dBm (dark purple, the weakest signal Meshtastic can decode) to -80 dBm (bright yellow, a very strong signal).

## How to reproduce this

If you want to run the same simulation for your own site, here is the exact sequence.

### Step 1: Open the Site Planner

Go to [site.meshtastic.org](https://site.meshtastic.org/) in a browser. You will see a world map with a panel of settings on the right hand side. The panel has five collapsible sections: Site / Transmitter, Receiver, Environment, Simulation Options, and Display.

### Step 2: Configure Site / Transmitter

Click the **Site / Transmitter** dropdown. Replace the default values with these:

| Field | Value | Why |
|-------|-------|-----|
| Site name | October Books | Or whatever you want to call your site |
| Latitude (degrees) | 50.9253 | October Books, 189 Portswood Road, Southampton |
| Longitude (degrees) | -1.3936 | The minus sign matters: it means west of Greenwich |
| Power (W) | 0.158 | Equivalent to 22 dBm, the maximum legal transmit power in the UK on the 868 MHz band |
| Frequency (MHz) | 869 | The European 868 MHz LoRa band (the Site Planner uses 869 as the centre value for EU) |
| Height AGL (m) | 3 | "AGL" is "above ground level." Three metres represents a node mounted just outside the shopfront, above the signage |
| Antenna gain (dB) | 2 | The default antenna on the SenseCAP Solar Node P1-Pro is 2 dBi |

Then click **Set with Map** and **Center map on transmitter** to verify the map jumps to Portswood Road in Southampton. This confirms the coordinates are correct.

### Step 3: Configure Receiver

Click the **Receiver** dropdown. Set:

| Field | Value | Why |
|-------|-------|-----|
| Sensitivity (dBm) | -133 | The default for the LongFast Meshtastic preset, the most commonly used configuration |
| Height AGL (m) | 1.5 | Phone-height for students taking RSSI readings during the walk |
| Antenna gain (dB) | 2 | Same antenna assumption as the transmitter |
| Cable loss (dB) | 0 | The SenseCAP Solar Node has the antenna directly mounted, no cable involved |

### Step 4: Configure Environment

Click the **Environment** dropdown. Set:

| Field | Value | Why |
|-------|-------|-----|
| Radio climate | Continental Temperate | The closest match for the UK in the options available |
| Polarization | Vertical | Standard for Meshtastic antennas |
| Clutter height (m) | 8 | The average height of typical terraced houses and mature street trees along Portswood Road. This parameter matters more than any other for an urban prediction |
| Ground dielectric, conductivity, atmospheric bending | leave at default | These are physics constants for British soil and atmosphere |

### Step 5: Configure Simulation Options

Click the **Simulation Options** dropdown. Set:

| Field | Value | Why |
|-------|-------|-----|
| Situation Fraction (%) | 95 | Standard reliability threshold |
| Time Fraction (%) | 95 | Standard reliability threshold |
| Max Range (km) | 2 | The walk is 1.2 km, so 2 km gives margin without unnecessary compute time |

### Step 6: Run Simulation

Scroll back up and click the green **Run Simulation** button. The map redraws with the coloured coverage overlay in 10 to 30 seconds depending on the size of the simulation area.

The Display section at the bottom controls how the coverage is rendered. The defaults (Plasma colour scale, 50% transparency, -130 to -80 dBm range) are sensible and were used for the prediction shown above.

## How to read the coverage map

The colour scale (visible in the legend on the right side of the screenshot above) goes from dark purple at the lowest end to bright yellow at the highest.

| Colour | Approximate dBm | What it means |
|--------|----------------|---------------|
| Bright yellow | -80 to -90 | Very strong signal, no problems decoding |
| Orange | -90 to -100 | Strong signal, reliable |
| Pink to magenta | -100 to -115 | Moderate signal, usable for Meshtastic |
| Dark purple | -115 to -130 | Weak signal, at the edge of what can be decoded |
| Off-map | below -130 | No usable signal predicted |

For our route, the prediction shows that a node at October Books alone would reach Highfield campus with weak but probably workable signal, which is exactly why we plan four nodes rather than one. The mesh fills in the marginal areas.

## What the Site Planner does not see

The Site Planner uses NASA SRTM elevation data, which models terrain (hills, valleys, ridges) but does not model buildings, trees, weather, or electromagnetic interference. For a flat urban route like Portswood Road, the terrain data has very little to work with, which is why the predicted coverage is almost a perfect circle. In reality:

- Brick terraced houses absorb LoRa signal significantly, especially perpendicular to the line of the road
- Mature street trees in full leaf attenuate signal by roughly 20 to 30 percent compared to bare branches
- Steel-framed buildings and large metal objects (skips, parked vans) create reflective and shadowed zones the model cannot anticipate

The Site Planner itself flags this directly in its documentation:

> The Site Planner uses NASA SRTM elevation data and the ITM/Longley-Rice propagation model. It is a good idea to always verify predictions from this tool using real-world testing.

This is why the prediction is useful as a starting point but the walk is what decides the actual node placement.

## Three predictions on record

Before the walk on 14 May 2026, we are committing to three predictions in writing, so that the comparison with what we find is honest:

1. **Strong signal along the first 600 to 700 metres of the walk.** The map shows orange to pink coverage along Portswood Road for this distance.
2. **Weakening to marginal signal as the route approaches Highfield.** The map shows magenta to dark purple in this zone.
3. **The roundness of the prediction is suspicious.** The coverage map is nearly circular, which is unlikely in dense urban housing with mature trees. The walk should reveal coverage that is more elongated along the road axis and more attenuated perpendicular to it.

These will be checked against the findings on the [findings page](findings.md) after the walk.

## Why publish predictions in advance

Predictions published before the data cannot be quietly retrofitted to match what was found. This is a small but useful piece of intellectual honesty borrowed from preregistered scientific research, and it makes the eventual comparison meaningful. It also models for students that grown-ups make predictions in writing and then check whether they were right.
