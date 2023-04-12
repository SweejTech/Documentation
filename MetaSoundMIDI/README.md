This documentation is for a plugin called SweejTech MetaSound MIDI, designed specifically for Unreal Engine 5's MetaSounds. The plugin facilitates working with MIDI messages and MetaSounds in the context of audio processing within the game engine. It provides several interfaces and utilities for handling MIDI events such as Note On, Note Off, Control Change, and Pitch Bend. The documentation includes descriptions and tooltips for each interface, as well as the inputs and outputs of each interface. Additionally, it covers the settings, plugin info, console commands, and example content related to the plugin. Overall, this documentation serves as a guide to help users understand and utilize the plugin's features and functionalities in conjunction with Unreal Engine 5's MetaSounds system.


# Table of Contents
1. [SweejTech.MIDI.Simple](#sweejtechmidisimple)
   * [Channel](#channel)
   * [Note Off](#note-off)
   * [Note Off Number](#note-off-number)
   * [Note Off Velocity](#note-off-velocity)
   * [Note On](#note-on)
   * [Note On Number](#note-on-number)
   * [Note On Velocity](#note-on-velocity)
2. [SweejTech.MIDI.Voices](#sweejtechmidivoices)
   * [Active Values](#active-values)
   * [Channel Values](#channel-values)
   * [Note Number Values](#note-number-values)
   * [Note Off Values](#note-off-values)
   * [Note On Values](#note-on-values)
   * [Note Velocity Values](#note-velocity-values)
   * [OnUpdate](#onupdate)
3. [SweejTech.MIDI.ControlChange](#sweejtechmidicontrolchange)
   * [Values](#values)
   * [OnUpdate](#onupdate-1)
4. [SweejTech.MIDI.PitchBend](#sweejtechmidipitchbend)
   * [Value](#value)
   * [Scaled](#scaled)
5. [Final text in other parts of the plugin](#final-text-in-other-parts-of-the-plugin)
   * [Settings](#settings)
   * [Console Commands](#console-commands)
   * [Example Content](#example-content)



## SweejTech.MIDI.Simple
### Channel
MIDI Channel number of the latest MIDI message.

### Note Off
Triggers when a NoteOff MIDI message is received. Because of how MetaSounds work, more than one MIDI message in a frame will still result in one trigger. If you want to read multiple notes in one frame, use the Voices interface.

### Note Off Number
Note number value of the latest NoteOff MIDI message received.

### Note Off Velocity
Velocity value of the latest NoteOff MIDI Message received.

### Note On
Triggers when a NoteOn MIDI message is received. Because of how MetaSounds work, more than one MIDI message in a frame will still result in one trigger. If you want to read multiple notes in one frame, use the Voices interface.

### Note On Number
Note number value of the latest NoteOn MIDI Message received.

### Note On Velocity
Velocity value of the latest NoteOn MIDI Message received.

## SweejTech.MIDI.Voices
### Active Values
The active state for each voice, trying to represent while a MIDI key is pressed. The value for each voice is set to true when a NoteOn MIDI message is received, and false when a NoteOff MIDI message with the same NoteNumber is received.

### Channel Values
The channel for each voice. Initial value for each item is 0.

### Note Number Values
The note number for each voice. Can be sent to a MIDI To Frequency-node to get a frequency value. Initial value for each item is -1.

### Note Off Values
The note off state for each voice. Each value behaves like a trigger impulse, and will only be true for one frame when a NoteOff MIDI message is received for a voice.

### Note On Values
The note on state for each voice. Each value behaves like a trigger impulse, and will only be true for one frame when a NoteOn MIDI message is received for a voice.

### Note Velocity Values
The velocity value between 0 and 127 for each voice. Initial value for each item is 0.

### OnUpdate
Triggers when any of the Voice arrays have been updated.

## SweejTech.MIDI.ControlChange
### Values
An array with all Control Change values. It always has 128 entries, one for each CC ID, and each item has a value between 0 and 127. There is a MetaSound Patch named Sweej_Utility_MIDICC_MSP which is designed to work with this array, which can be added as any other node in the MetaSound editor.

### OnUpdate
Triggers when the Values array has been updated.

## SweejTech.MIDI.PitchBend
### Value
The Pitch Bend value, wheely good. Outputs a value between 0 and 16383.

### Scaled
The Pitch Bend value scaled to a float. Outputs a value between -1 and 1, and can simply be multiplied to get a pitch range.

# Final text in other parts of the plugin
## Settings
### MIDI Devices
- Enabled | MIDI Device | Status
- Refresh MIDI Device (Tooltip: Refresh the list with the available connected MIDI devices.)

### Voices Meta Sound Interface Options
- Number of Voices (Tooltip: Number of voices to use when using the SweejTech.Voices MetaSound Interface.)

### MIDI Monitor
- Log MIDI Messages (Tooltip: Check to enable MIDI messages logging.)
- Enable Voice Frame Monitor (Tooltip: Check to enable Voices Frame logging.)
- Clear MIDI Monitor Log (Tooltip: Clears the MIDI Monitor Log.)

## Console Commands
### SweejTech.MIDI.EnableMIDIMessagesLogging
- Enable/Disable MIDI messages to be logged in the output log.

### SweejTech.MIDI.EnableVoicesFrameLogging
- Enable/Disable Voices Frame information to be logged in the output log.

## Example Content
### File Name
- Display Name
- Description

### Sweej_Utility_Arpeggiator_MSP
- SweejTech MIDI Arpeggiator
- Takes your last 3 played notes and arpeggiates them. Each trigger can be gated based on the Probability value. For example, a probability of 0.7 gives a 70% chance of triggering. Patch assumes you are using the default 8 voices.

### Sweej_Utility_ControlChange_MSP
- SweejTech ControlChange Map Range
- Normalizes the Control Change value of the given index. You can choose to use linear or logarithmic normalization. OnTrue will trigger as long as the value is above the default -1.

### Sweej_Utility_GetNoteOnAndNumber_MSP
- SweejTech MIDI Voice-Simple
- Gets the NoteOn and NoteNumber value from the given index.

### Sweej_Utility_Voice_MSP
- SweejTech MIDI Voice
- Gets all voice values from the given index.

### Sweej_Sequencer_DrumRack_MSP
- SweejTech Drum Sequencer
- Sends a trigger based on the TriggerProbability array. If you send a list with 4 entries where the first is 1 and the rest is 0 you will get a trigger every 4 beat. An entry of 0.7 gives a 70% chance of triggering.

### Sweej_Sequencer_Randomized-DrumRack_MSP
- SweejTech Randomized Drums
- An 8 slot drum machine. Each slot has a probability and seed value. A probability of 0.7 gives a 70% chance of triggering. Changing the seed to get a new probability pattern.

### Sweej_Sequencer_Scale-Arpeggiator_MSP
- SweejTech Scale Arpeggiator
- Takes your selected scale and arpeggiates it. Each trigger can be gated based on the Probability value. For example, a probability of 0.7 gives a 70% chance of triggering.

### Sweej_DrumRack_Logic_MSS
- SweejTech Drum Rack
- An 8 slot drum rack.
