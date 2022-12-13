# SweejTech Audio Inspector

Thanks for getting our Audio Inspector for Unreal Engine 5! It allows people working with sound to see detailed, useful info about all active sounds in game in a compact editor tab window.

## Installation

Get the plugin from Unreal Marketplace. Enable it. That's it!

## Usage

### Opening the window

To open the tab window, open the Window menu at the top of the editor and select SweejTech Audio Inspector. Dock the window anywhere you like, or keep it floating!

![How to bring up the window](https://github.com/SweejTech/Documentation/blob/main/AudioInspector/open-audio-inspector.png)

### Active Sounds

The Audio Inspector shows a table of playing sounds, with name first, followed by a customisable set of columns. By default it will show distance, volume, sound class, and attenuation.

![Docked AudioInspector tab with sounds](https://github.com/SweejTech/Documentation/blob/main/AudioInspector/audio-inspector-docked.png)

### Sorting

You can sort the sounds in the table based on a column's values by clicking the column header. They switch between ascending and descending when clicked multiple times. The default sort is on distance.

### Filtering

Sounds in the table can be filtered with a string using the search box at the top of the tab. The active sounds table will only show sounds whose names contain the filter string.

![Filtered list of active sounds in AudioInspector](https://github.com/SweejTech/Documentation/blob/main/AudioInspector/filter-by-name.png)

## Customisation

To customise the SweejTech Audio Inspector, open the Editor Preferences window and find Audio Inspector under the SweejTech header. You can customise:


- Which columns are shown
- The colour scheme for the Volume column
- The update rate of the UI


