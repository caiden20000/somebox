# Somebox
I want to implement a feature, where each pitch lane of a drum-type instrument can play a sample, like a normal drum machine interface.

### Current progress:

- [x] Have the idea
- [ ] Understand the codebase
- [ ] Implement the feature

Hey, that's 1/3 of the way there!

### Implementation ideas:
There are 2 ways I think I could implement this:  
1. As a new instrument "type" selection for drum/noise channels
2. As a new channel type

Option 1 seems the easiest, but I see an issue/s:  
- Feature consistency
    - I think the way I'd implement it is incongruous with the way beepbox is set up
    - EG. How would my system deal with sliding notes between pitch lanes?
    - The "mod" channel type is set up in such a way that you can't pitch bend, but it's not set up to play notes. How easy would it be to change the mod vs the drum channel type?

Option 2 would be even better, but...  
- Difficulty
    - I'm sure a *lot* of this codebase revolves around there being 3 channel types, and I'll have to find every place this assumption exists

I'm trying for option 1 first, my current game plan is: 
- [x] Add a new instrument type for drum/noise channels
    - It's called "sampler"
    - When you click it, you get an error message saying things corrupted. It's a start!
- [ ] Add a new settings element that lets you choose custom chipwaves for each drum pitch lane
- [ ] Add these settings to the instrument
- [ ] Add a new "change" class that allows the changing of these samples and sample settings
- [ ] Record these settings in the song url (is this automatic?)

This plan will expand and evolve as time goes on and I learn more about how the codebase works.

### Update

I'm trying option 2 now. I haven't yet removed the "sampler" instrument type from the preset selection menu, but I am adding a sampler channel type anywhere I can.
I'm following the trail of mod -- anywhere there is a mod channel type logic, the sampler channel logic may need to be included.

There are a lot of logic assumptions about how the channels are arranged, that it goes pitches -> noises -> mod, and channels are often counted like that and indexed like that.  
This will cause errors in places I don't catch this assumption when adding the sampler channel type.

Things to add sampler channel logic to:  
- [ ] changes.ts
    - [x] ChangeChannelCount
    - [x] ChangeAddChannelInstrument
    - [x] ChangeAppendInstrument
    - [ ] More?
- [ ] synth.ts
    - [ ] Instrument
        - [x] variables
        - [x] constructor
        - [x] setTypeAndReset
        - [x] toJsonObject
        - [ ] fromJsonObject
        - [x] getFadeInSeconds
        - [x] getFadeOutTicks
    - [ ] Song
        - [x] samplerChannelCount variable
        - [x] getChannelCount
        - [x] getChannelIsSampler
        - [ ] toBase64String
        - [ ] fromBase64String
        - [ ] toJsonObject
        - [ ] fromJsonObject
        - [ ] More...
    - [ ] 
- [ ] More...

*note: Current commit is **broken** as this "refactor" will take a long time.



---

# UltraBox

UltraBox is an online tool for sketching and sharing instrumental music.
You can find it [here](https://ultraabox.github.io).
It is a modification of [JummBox](https://github.com/jummbus/jummbox), which inturn is a modification of the [original BeepBox](https://beepbox.co).

The goal of UltraBox is to combine every single beepbox mod into one. Feel free to contribute!


All song data is packaged into the URL at the top of your browser. When you make
changes to the song, the URL is updated to reflect your changes. When you are
satisfied with your song, just copy and paste the URL to save and share your
song!

UltraBox, as well as the beepmods which it's based on, are free projects. If you ever feel so inclined, please support the original creator, [John Nesky](http://www.johnnesky.com/), via
[PayPal](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=QZJTX9GRYEV9N&currency_code=USD)!

## Compiling

The compilation procedure is identical to the repository for BeepBox. I will include the excerpt on compiling from that page's readme below for convenience:

The source code is available under the MIT license. The code is written in
[TypeScript](https://www.typescriptlang.org/), which requires
[node & npm](https://www.npmjs.com/get-npm), so install those first. Then to
build this project, open a command line ([Git Bash](https://gitforwindows.org/)) and run:

```
git clone https://github.com/ultraabox/ultrabox_typescript
cd ultrabox_typescript
npm install
npm run build
```

JummBox (and by extension, Ultrabox) makes a divergence from BeepBox that necessitates an additional dependency:
rather than using the (rather poor) default HTML select implementation, the custom
library [select2](https://select2.org) is employed. select2 has an explicit dependency
on [jQuery](https://jquery.com) as well, so you may need to install the following
additional dependencies if they are not picked up automatically.

```
npm install select2
npm install @types/select2
npm install @types/jquery
```

## Code

The code is divided into several folders. This architecture is identical to BeepBox's.

The [synth/](synth) folder has just the code you need to be able to play UltraBox
songs out loud, and you could use this code in your own projects, like a web
game. After compiling the synth code, open website/synth_example.html to see a
demo using it. To rebuild just the synth code, run:

```
npm run build-synth
```

The [editor/](editor) folder has additional code to display the online song
editor interface. After compiling the editor code, open website/index.html to
see the editor interface. To rebuild just the editor code, run:

```
npm run build-editor
```

The [player/](player) folder has a miniature song player interface for embedding
on other sites. To rebuild just the player code, run:

```
npm run build-player
```

The [website/](website) folder contains index.html files to view the interfaces.
The build process outputs JavaScript files into this folder.

## Dependencies

Most of the dependencies are listed in [package.json](package.json), although
I'd like to note that UltraBox also has an indirect, optional dependency on
[lamejs](https://www.npmjs.com/package/lamejs) via
[jsdelivr](https://www.jsdelivr.com/) for exporting .mp3 files. If the user
attempts to export an .mp3 file, UltraBox will direct the browser to download
that dependency on demand.


## Offline version

If you'd like to BUILD the offline version, enter the following into the command line of your choice:
```
npm run build-offline
```


After building, you can then enter the following to run it for testing purposes:
```
npm run start
```

And to package, run (do ```npm run package-host``` for your host platform; you may need to run git bash as an administrator for non-host platforms):
```
npm run package
```