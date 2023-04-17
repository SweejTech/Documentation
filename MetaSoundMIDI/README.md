# SweejTech MetaSound MIDI Documentation

This documentation is for a plugin called **SweejTech MetaSound MIDI**, designed specifically for Unreal Engine 5's MetaSounds. The plugin facilitates working with MIDI messages in MetaSounds by providing several interfaces and utilities for handling MIDI events such as Note On, Note Off, Control Change, and Pitch Bend.

## Table of Contents

- [Disclaimers](#disclaimers)
- [Quick Setup](#quick-setup)
- [Interfaces](#interfaces)
  - [SweejTech.MIDI.Simple](#sweejtechmidisimple)
  - [SweejTech.MIDI.Voices](#sweejtechmidivoices)
  - [SweejTech.MIDI.ControlChange](#sweejtechmidicontrolchange)
  - [SweejTech.MIDI.PitchBend](#sweejtechmidipitchbend)
- [Settings](#settings)
  - [MIDI Devices](#midi-devices)
  - [Voices MetaSound Interface Options](#voices-metasound-interface-options)
  - [MIDI Monitor](#midi-monitor)
- [Console Commands](#console-commands)
- [Utilities and Example Content](#utilities-and-example-content)
- [Log Warning/Error Messages](#log-warningerror-messages)
  - [SweejMIDIDeviceInputController.cpp](#sweejmidideviceinputcontrollercpp)
  - [SweejMIDIDeviceManager.cpp](#sweejmididevicemanagercpp)
  - [SweejMIDIMessagesManager.cpp](#sweejmidimessagesmanagercpp)

## Disclaimers

### Can only be used in MetaSound preview
This plugin is intended as a tool for use directly when previewing a MetaSound. It cannot be used when playing a game in the viewport in engine or in a game.

### Latency is mainly dependent on Unreal
Please note that you might experience latency when using this plugin. Latency is mainly dependent on Unreal Engine, and we cannot get rid of this delay, as it is part of the Unreal Engine runtime. However, adjusting the Callback Buffer Size can improve latency. To do so, go to Edit → Project Settings → Platforms → Windows → Callback Buffer Size. Please note that values below 512 do not improve latency in our experience.

### Update rate is dependent on Unreal
When operating at a higher BPM or smaller beat divisions, it may not be possible to get perfect intervals between triggers. These values will be quantized by the editor's update rate, and problems may get worse if there are performance issues.

Additionally, debug tools such as pulsating wires in MetaSounds can affect performance.

Finally, please note that when Unreal is out of focus, framerate may be lower.

### Only one device supported at a time
We only support one device at a time.

## Quick Setup

Watch the overview video (coming soon) where we go through these steps
1. Enable the plugin in Edit… → Plugins
2. Choose a MIDI device in Project Settings → SweejTech MetaSound MIDI
3. Open a MetaSound
4. Add interface SweejTech.MIDI.Simple
5. Right click the graph and add event NoteOn from the newly added interface
6. Start preview by pressing space on your keyboard, or the preview button in the toolbar
7. NoteOn events can now be received from your selected MIDI device

## Interfaces

### SweejTech.MIDI.Simple

- **Channel**: MIDI channel number of the latest MIDI message.
- **Note Off**: Triggers when a NoteOff MIDI message is received. Because of how MetaSounds work, more than one MIDI message in a frame will still result in one trigger. If you want to read multiple notes in one frame, use the Voices interface.
- **Note Off Number**: Note number value of the latest NoteOff MIDI message received.
- **Note Off Velocity**: Velocity value of the latest NoteOff MIDI Message received.
- **Note On**: Triggers when a NoteOn MIDI message is received. Because of how MetaSounds work, more than one MIDI message in a frame will still result in one trigger. If you want to read multiple notes in one frame, use the Voices interface.
- **Note On Number**: Note number value of the latest NoteOn MIDI Message received.
- **Note On Velocity**: Velocity value of the latest NoteOn MIDI Message received.

### SweejTech.MIDI.Voices

- **Active Values**: The active state for each voice, trying to represent while a MIDI key is pressed. The value for each voice is set to true when a NoteOn MIDI message is received, and false when a NoteOff MIDI message with the same NoteNumber is received.
- **Channel Values**: The channel for each voice. Initial value for each item is 0.
- **Note Number Values**: The note number for each voice. Can be sent to a 'MIDI To Frequency' node to get a frequency value. Initial value for each item is -1.
- **Note Off Values**: The note off state for each voice. Each value behaves like a trigger impulse, and will only be true for one frame when a NoteOff MIDI message is received for a voice.
- **Note On Values**: The note on state for each voice. Each value behaves like a trigger impulse, and will only be true for one frame when a NoteOn MIDI message is received for a voice.
- **Note Velocity Values**: The velocity value between 0 and 127 for each voice. Initial value for each item is 0.
- **OnUpdate**: Triggers when any of the Voice arrays have been updated.

### SweejTech.MIDI.ControlChange

- **Values**: An array with all Control Change values. It always has 128 entries, one for each CC ID, and each item can be set between 0 and 127. If an index has not received a value, it defaults to -1. There is a MetaSound Patch named “SweejTech MIDI Map ControlChange” which is designed to work with this array, and can be added as any other node in the MetaSound editor.
- **OnUpdate**: Triggers when the Values array has been updated.

### SweejTech.MIDI.PitchBend

- **Value**: The Pitch Bend value, wheely good. Outputs a value between 0 and 16383.
- **Scaled**: The Pitch Bend value scaled to a float. Outputs a value between -1 and 1, and can simply be multiplied to get a pitch range.

## Settings

These settings are found in the SweejTech MetaSound MIDI section of the Project Settings.

### MIDI Devices

- **Enabled**: Which of your detected MIDI devices that is enabled. Only one device can be enabled.
- **MIDI Device**: List of all detected MIDI devices.
- **Status**: Current status of all MIDI devices.
- **Refresh MIDI Device**: Refreshes the list of connected MIDI devices. Click this if a new device has been connected but not yet detected.

### Voices MetaSound Interface Options

- **Number of Voices**: Number of voices that will be sent through the SweejTech.MIDI.Voices MetaSound Interface. The “SweejTech MIDI Voice Select” MetaSound Patch is made to get all the information from its VoiceIndex. You need to use one Voice Select for each of the voices.

### MIDI Monitor

- **Log MIDI Messages**: Check this to enable MIDI messages logging in the MIDI Monitor and the editor's Output Log.
- **Enable Voice Frame Monitor**: Check this to enable Voice Frame logging in the MIDI Monitor and the editor’s Output Log. The Voice Frame logging will log all voice values (i.e. the SweejTech.MIDI.Voices) for each message, in a table.
- **Clear MIDI Monitor Log**: Clear the MIDI Monitor log history.

## Console Commands

- `SweejTech.MIDI.EnableMIDIMessagesLogging`: Enable/Disable MIDI messages to be logged in the output log. Send 1 to enable and 0 to disable.
- `SweejTech.MIDI.EnableVoicesFrameLogging`: Enable/Disable Voices Frame information to be logged in the output log. Send 1 to enable and 0 to disable.

## Utilities and Example Content

| File Name                          | Display Name                      | Description |
|------------------------------------|-----------------------------------|-------------|
| Sweej_Utility_Arpeggiator_MSP      | SweejTech MIDI Arpeggiator        | Takes your last 3 played notes and arpeggiates them. Each trigger can be gated based on the Probability value. For example, a probability of 0.7 gives a 70% chance of triggering. Patch assumes you are using the default 8 voices. |
| Sweej_Utility_ControlChange_MSP    | SweejTech MIDI Map ControlChange  | Maps the Control Change value of the given index. |
| Sweej_Utility_VoiceSelect-Simple_MSP | SweejTech MIDI Voice Select Simple | Gets the NoteOn and NoteNumber value from the given index. |
| Sweej_Utility_StereoGain_MSP       | SweejTech MIDI Stereo Gain        | |
| Sweej_Utility_VoiceSelect_MSP      | SweejTech MIDI Voice Select       | Gets all voice values from the given index. |
| Sweej_Sequencer_DrumRack_MSP       | SweejTech MIDI Drum Sequencer     | Sends a trigger based on the TriggerProbability array. If you send a list with 4 entries where the first is 1 and the rest is 0 you will get a trigger every 4 beat. An entry of 0.7 gives a 70% chance of triggering. |
| Sweej_Sequencer_Randomized-DrumRack_MSP | SweejTech MIDI Drum Sequencer Randomized | An 8 slot drum machine. Each slot has a probability and seed value. A probability of 0.7 gives a 70% chance of triggering. Changing the seed to get a new probability pattern. |
| Sweej_Sequencer_Scale-Arpeggiator_MSP | SweejTech MIDI Scale Arpeggiator | Takes your selected scale and arpeggiates it. Each trigger can be gated based on the Probability value. For example, a probability of 0.7 gives a 70% chance of triggering. |
| Sweej_Sample_Player_MSS            | SweejTech MIDI Sample Player      | Enables you to input an array of waves and make them playable with your MIDI device. |
| Sweej_DrumRack_Logic_MSS           | SweejTech MIDI Drum Rack          | An 8 slot drum rack. |

## Log Warning/Error Messages

### SweejMIDIDeviceInputController.cpp

| Type    | Message |
|---------|---------|
| Error   | Failed to initialize MIDI device: The MIDI buffer size: \"%i\" must be greater than zero. Make sure that the defined audio callback buffer size is set to a value greater than zero in: Project Settings > Platforms > (Windows or Mac) > Callback Buffer Size, and then proceed to restart the editor. |
| Error   | Failed to initialize MIDI device \"%s\" (ID: %i): Device is already in use. Close other applications using the MIDI device, refresh MIDI devices and try to connect again. |
| Warning | Failed to bind to MIDI device \"%s\" (ID: %i): Device is already in use. Close other applications using the MIDI device, refresh MIDI devices and try to connect again. |
| Warning | Failed to initialize MIDI device \"%s\" (ID: %i): The device does not support MIDI In. |
| Error   | Unable to open input connection to MIDI device \"%s\". Check if the MIDI device is already owned by another application. Close other applications using the MIDI device, refresh MIDI devices and try to connect again. [Device ID: %i, PortMidi error: %s] |
| Error   | Encountered an error when closing the input connection to MIDI device \"%s\". The device has probably been disconnected or owned by another process. [Device ID: %i, PortMidi error: %s] |
| Error   | Encountered an error when processing incoming MIDI events for device \"%s \". [Device ID: %i, PortMidi error: %s] |

### SweejMIDIDeviceManager.cpp

| Type    | Message |
|---------|---------|
| Error   | Unable to initialize the MIDI device manager. [PortMidi error: %s] |
| Error   | Unable to query information about MIDI Device ID: %i. This device won't be available for input or output. |
| Warning | Unable to find MIDI devices since the MIDI device manager failed to initialize. |
| Warning | MIDI device manager is not initialized. |
| Warning | MIDI Device \"%s\" (ID: %i) re-initialization failed. Make sure that the device is not disconnected or owned by another process. |

### SweejMIDIMessagesManager.cpp

| Type | Message |
|------|---------|
| Info | MIDI devices not available. |
