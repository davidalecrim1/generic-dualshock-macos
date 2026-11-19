# Generic DualShock pad on macOS: the left stick moves the right stick

You plug a cheap "PS2-style" USB controller into a Mac, and the right analog
stick twitches whenever you push the left one sideways. Up and down are fine.
Every button is fine. Only the left stick's horizontal movement leaks into the
right stick. Two pads from the same brand do exactly the same thing, which
usually sends people looking for a bad solder joint or a broken driver.

It is neither. The pad is not defective and macOS is not misreading it.

The name here refers to the pad's shape and layout, not to a Sony DualShock.

## What is actually happening

The controller reports the left stick's left/right position twice: once in the
field reserved for the left stick, and again in the field reserved for the right
stick. The right stick's own horizontal position is also sent, but in a field the
controller mislabels, so programs do not know to look there.

macOS reads what the controller sends and passes it on unchanged. Every game,
emulator and controller panel then sees the right stick moving along with the
left, because that is literally what the hardware claims is happening.

Because the mistake lives in the controller's firmware, it is identical on every
unit of that model. Nothing can be repaired by opening the shell, and no driver
update from Apple will fix it.

## Which controllers

Generic PS2-style USB pads that identify themselves as:

- Name: `Generic   USB  Joystick`
- Vendor: `DragonRise Inc.`
- USB ID: `0079:0006`

If your controller shows up under a different name or ID, this repo does not
apply to it.

## Can it be fixed?

Yes, on macOS, for every application at once — no per-game remapping, no
disabling system security, no kernel extensions.

The fix teaches a small signed driver app to read the pad's raw report and
rebuild the controls correctly, then present a clean controller to macOS. Games
see a normal pad; the flawed firmware is bypassed entirely.

It has been verified working with PCSX2 on macOS 27.2.1 (Apple Silicon) and is
not emulator-specific.

## Read the fix

Step-by-step instructions, the diagnosis and the exact configuration file are in
**[fix.md](fix.md)**.

If you only want the short version: install [OpenJoystickDriver](https://github.com/xsyetopz/OpenJoystickDriver),
drop [`records/0079-0006.patch.json`](records/0079-0006.patch.json) into
`~/Library/Application Support/OpenJoystickDriver/Controllers/`, and move the
left stick to confirm the right stick stays still.
