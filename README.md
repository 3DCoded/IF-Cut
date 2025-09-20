# IF-Cut Configurations
Simple inline universal filament cutter

```bash
if cut, then print
```

## Installation

1. Open the `mmu/base/` folder in your printer's configuration on Mainsail or Fluidd.
2. Create a new file: `ifcut.cfg`.
3. Copy-and-paste the contents of this [`ifcut.cfg`](https://github.com/3DCoded/IF-Cut-Configs/blob/main/ifcut.cfg) into it.
4. Create a new file: `ifcut_hw.cfg`.
5. Copy-and-paste the contents of this [`ifcut_hw.cfg`](https://github.com/3DCoded/IF-Cut-Configs/blob/main/ifcut_hw.cfg) into it.

## Servo Configuration

> [!IMPORTANT]
> Make sure the printed arm piece is disconnected from the servo horn while performing initial configurations.

1. First, set the proper servo pin under the `[servo ifcut]` config in `ifcut_hw.cfg`.
2. Remove the `initial_angle: ` line.
3. Save and restart Klipper.
4. Use the `SET_SERVO SERVO=ifcut ANGLE=` command to move the servo, adjusting the `ANGLE` parameter to find two key positions: open (top arm filament hole should be just oudside of the blade) and closed (top arm and main body filament holes should line up). You can gently rest the arm on the servo horn while doing these angle tests.
5. Open `ifcut.cfg`
6. Set `variable_close_angle` to the closed angle you found.
7. Set `variable_open_angle` to the open angle you found.
8. Open `ifcut_hw.cfg` and set `initial_angle` to the closed angle you found.
9. Save and restart Klipper.
10. You may now secure the printed arm to the servo horn using the two bolts that came with it.

## Movement Calibration

1. Make sure your filament is in the parking position. You can do this by loading a tool (with `T0`), then unloading it (with `MMU_UNLOAD`).
2. Issue an `IFCUT_OPEN` command. The servo should open to the previously found open angle.
3. Use `MMU_TEST_MOVE MOVE=` commands to slowly move the filament towards the cutter.
4. Once the filament is about flush with the bottom of the arm, issue a `MMU_STATUS` command and note the last line: `UNLOADED -315.4mm` In this case, the cutting position is 314.4mm (no negative sign)
5. Set `variable_cut_position` to the found cut position in `ifcut.cfg`
6. Save and restart Klipper.

## Initial Tests

1. Move the filament back to the parking position.
2. Issue an `IFCUT_ROUTINE` command.

This command should:

1. Open the cuttter arm.
2. Slowly load the filament to the end of the arm.
3. Rapidly push the filament out into the blade's path.
4. Close and reopen the cutter arm.
5. Repeat steps 3 and 4.
6. Unload filament.
7. Close the cutter arm.

If all goes well, you're ready to move on to final configuration!

## Final Configuration

Now we need to tell Happy Hare to use the IF-Cut cutter.

1. In `mmu_parameters.cfg`, set `form_tip_macro` to `_MMU_FORM_TIP`.
2. In `mmu_macro_vars.cfg`, set `variable_user_post_unload_extension` to `'IFCUT_ROUTINE'`.
3. Save and restart Klipper.

## What's next?

Now, you'll have to calibrate a very rough tip forming routine. You don't need to calibrate it enough to get smooth loads, just reliable unloads, and the IF-Cut will cut off the ugly tip so it doesn't jam up on loads.

What's next? Lots of printing...and maybe a star here or on the [3MS](https://github.com/3dcoded/3MS) :smiley: 🌟
