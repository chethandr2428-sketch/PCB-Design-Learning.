
# PCB Design Learning — Day 1

**Software:** KiCad  
**Project:** Simple LED Circuit PCB  
**Level:** Beginner  
**Cost:** Free — no physical PCB order required

## Project Goal

Learn the complete beginner PCB design workflow in KiCad:

1. Create a project.
2. Draw a circuit schematic.
3. Assign component footprints.
4. Transfer components to PCB Editor.
5. Place components and draw the board outline.
6. Route copper tracks.
7. Run Design Rules Checker (DRC).
8. Generate Gerber and drill files.
9. Inspect the design.

> **Important:** This is a learning exercise. No physical PCB has been ordered. Passing DRC does not prove that a circuit is electrically correct. Check component ratings, LED polarity, power connections, connector pinout, and footprints before building or manufacturing.
---
## Step 1 — Create a KiCad Project

1. Open KiCad.
2. Click **File → New Project**.
3. Enter the project name:

   ```text
   LED_PCB
   ```

4. Choose a folder to save the project.
5. Click **Save**.

KiCad creates the project files.

## Step 2 — Open Schematic Editor

1. In the KiCad project window, double-click the schematic file or the **Schematic Editor** icon.
2. A blank schematic sheet should open.
3. Press `A` to open **Choose Symbol** when adding a component.

## Step 3 — Add Circuit Components

Press `A`, search for the symbol, select it, click **OK**, and click on the sheet to place it.

| Component | Search term in KiCad | Purpose |
|---|---|---|
| Resistor | `Device:R` | Limits current through the LED |
| LED | `Device:LED` | Produces light |
| Ground | `power:GND` | Ground / return connection |
| Positive supply | `power:+5V` or an appropriate power net | Positive supply connection |
| 2-pin connector | `Connector_Generic:Conn_01x02` | External battery or power input |

### Set the resistor value

1. Double-click the resistor.
2. In the **Value** field, enter:

   ```text
   330
   ```

3. Click **OK**.

This means a 330-ohm resistor.

### Draw wires

1. Press `W` while the cursor is on a pin to start a wire.
2. Click the intended destination pin to complete the connection.
3. Press `Esc` to stop the tool.
4. Save with `Ctrl+S`.

**Check:** Make sure the battery/power input, resistor, and LED form the intended circuit. Confirm the LED polarity and use a suitable supply voltage.

## Step 4 — Run Electrical Rules Checker (ERC)

1. Open **Inspect → Electrical Rules Checker (ERC)**.
2. Click **Run ERC**.
3. Review errors and warnings.
4. Correct any problems that apply to your circuit.
5. Save with `Ctrl+S`.

### Common power warning

If KiCad reports:

```text
Input Power pin not driven by any Output Power pins
```

check how the power is supplied. A `PWR_FLAG` can tell ERC that a net is driven by an external power source, but it does **not** provide real electrical power. Use it only when it accurately represents the circuit.

## Step 5 — Assign Component Footprints

A footprint defines the physical pads and dimensions for a component on the PCB.

1. In Schematic Editor, choose **Tools → Assign Footprints**. The exact menu wording may vary by KiCad version.
2. Select each component.
3. Search for a footprint matching the real physical component.
4. Assign the footprint and save the project.

### Important for a 2-pin battery connector

Choose a footprint that matches the actual connector you plan to use. A screw terminal and a pin header are different physical parts. Do not choose a footprint only because it has two pins; check pin spacing, body dimensions, and pin arrangement.

## Step 6 — Open PCB Editor and Update from Schematic

1. Return to the KiCad project window.
2. Open **PCB Editor**.
3. Choose **Tools → Update PCB from Schematic**, or press `F8` in common KiCad versions.
4. Click **Update PCB**.
5. Click **Close**.

The footprints should appear ready to place.

## Step 7 — Place Components

1. Hover over a footprint.
2. Press `M` to move it.
3. Move the mouse to the desired position.
4. Click to place it.
5. Arrange the connector, resistor, and LED neatly.
6. Leave enough room for tracks, pads, and the board edge.

Save with `Ctrl+S`.

## Step 8 — Draw the PCB Outline

The `Edge.Cuts` layer defines the physical board boundary.

1. In the Layers panel, select:

   ```text
   Edge.Cuts
   ```

2. Select the graphic rectangle tool, or use the graphic line tool to draw four connected edges.
3. Draw a rectangle around all components.
4. Make sure the edges meet and form a closed outline.
5. Ensure all outline edges are on `Edge.Cuts`.

If DRC reports:

```text
Board has malformed outline (no edges found on Edge.Cuts layer)
```

check that the outline exists, is closed, and is drawn on `Edge.Cuts`.

## Step 9 — Route Copper Tracks

1. Select the front copper layer:

   ```text
   F.Cu
   ```

2. Hover over a pad that has an airwire.
3. Press `X` to start routing.
4. Follow the airwire to the correct destination pad.
5. Click the destination pad to finish the track.
6. Repeat for all intended connections.
7. Save with `Ctrl+S`.

**Remember:** Thin airwires are connection guides; they are not copper tracks. Connect only the intended pads. Do not connect pads randomly.

## Step 10 — Run Design Rules Checker (DRC)

1. Open **Inspect → Design Rules Checker**.
2. Click **Run DRC**.
3. Check both **Violations** and **Unconnected Items**.
4. Fix reported issues.
5. Save the board.

Common problems:

| Error | What to check |
|---|---|
| Malformed board outline | Closed outline on `Edge.Cuts` |
| Clearance violation | Space between tracks and pads |
| Track width violation | Track width meets design rules |
| Unconnected items | Missing copper connection |
| Power-related schematic warning | Power-source setup and ERC flags |

A result of zero errors and zero unconnected items is a good milestone, but it does not guarantee electrical correctness.

## Step 11 — View the PCB in 3D

1. In PCB Editor, open the 3D Viewer. In standard KiCad versions, `Alt+3` is a common shortcut.
2. Inspect the board shape and component placement.
3. Check that footprints look correct.
4. Confirm that components fit inside the board outline.

If a footprint does not match the real component, correct it before manufacturing.

## Step 12 — Generate Gerber Files

Gerber files describe the PCB manufacturing layers.

1. In PCB Editor, open **File → Fabrication Outputs → Gerbers** or the **Plot** dialog, depending on the KiCad version.
2. Set the output folder to:

   ```text
   Gerber
   ```

3. Select the layers used by your board. For a simple front-copper board, these commonly include:

   ```text
   F.Cu
   F.Mask
   F.Silkscreen
   Edge.Cuts
   ```

4. Include other layers if your design uses them.
5. Keep the scale at `1:1`.
6. Generate or plot the files.

Common Gerber file examples:

| Layer/file | Purpose |
|---|---|
| `F_Cu.gbr` | Front copper |
| `F_Mask.gbr` | Front solder mask |
| `F_Silkscreen.gbr` | Front component labels |
| `Edge_Cuts.gbr` | Board outline |
| `F_Paste.gbr` | Front solder-paste layer, when needed |

Actual filenames depend on your project name and KiCad settings.

## Step 13 — Generate Drill Files

1. In the plotting window, click **Generate Drill Files**.
2. Select **Excellon** format.
3. Choose **millimeters** unless a manufacturer specifies otherwise.
4. Generate the files.

Common outputs include:

```text
Day1-PTH.drl
Day1-NPTH.drl
```

- `PTH.drl` — plated through-hole drill locations.
- `NPTH.drl` — non-plated through-hole drill locations.

Check the output messages and confirm the files exist in your output folder.

## Step 14 — Inspect Gerber and Drill Files

1. Open **Gerber Viewer** in KiCad.
2. Load the generated `.gbr` files.
3. Load the `.drl` drill files using the drill-file option.
4. Check that copper tracks, pads, holes, solder mask, silkscreen, and the board outline align.
5. Review any remaining DRC warning.

Generating files is only a practice step. You do not need to order a physical PCB.

## Step 15 — Save Your Project Files

Keep the editable KiCad files together:

```text
LED_PCB.kicad_pro
LED_PCB.kicad_sch
LED_PCB.kicad_pcb
```

Keep manufacturing outputs in a separate folder:

```text
Gerber/
```

Gerber and drill files are manufacturing outputs; the `.kicad_pro`, `.kicad_sch`, and `.kicad_pcb` files are the editable project sources.

---

## Day 1 Checklist

- [x] Created a KiCad project
- [x] Added resistor, LED, and power/battery connection
- [x] Set the resistor value to 330 ohms
- [x] Assigned component footprints
- [x] Transferred the schematic to PCB Editor
- [x] Placed components
- [x] Drew the `Edge.Cuts` outline
- [x] Routed copper tracks
- [x] Ran DRC
- [x] Generated Gerber files
- [x] Generated drill files
- [ ] Inspect Gerber and drill files together
- [ ] Review any remaining DRC warning
- [ ] Verify actual footprints, polarity, supply voltage, and electrical connections

## Day 1 — Key Terms

- **Schematic:** Diagram of electrical connections.
- **Footprint:** Physical pad layout for a component.
- **PCB layout:** Physical arrangement of components and tracks.
- **Routing:** Drawing copper tracks between pads.
- **Airwire:** Guide showing a connection that needs routing.
- **DRC:** Checks PCB layout against design rules.
- **Gerber files:** Manufacturing data for board layers.
- **Drill files:** Locations of holes in the PCB.
- **Edge.Cuts:** Layer that defines the board boundary.

## Tomorrow — Day 2

1. Learn how to use Gerber Viewer.
2. Inspect each PCB layer.
3. Check the PCB in 3D.
4. Review common layout mistakes.
5. Start a small sensor-interface PCB project.

## Free Learning Resources

- [KiCad official website](https://www.kicad.org/)
- [KiCad Getting Started Guide](https://docs.kicad.org/9.0/en/getting_started_in_kicad/getting_started_in_kicad.html)
- [KiCad learning resources](https://www.kicad.org/help/learning-resources/)
- [KiCad beginner tutorials on YouTube](https://www.youtube.com/results?search_query=KiCad+PCB+design+beginner+tutorial)
