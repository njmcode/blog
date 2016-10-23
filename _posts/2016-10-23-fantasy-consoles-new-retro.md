---
layout: post
title: "Fantasy consoles and the 'new retro'"
summary: "Discovering the PICO-8 and the idealised memory of old computing."
date: 2016-10-23 22:47
image:
    main:
        src: /img/headers/pico-header.png
        credit: http://www.lexaloffle.com/pico-8.php
---
Having owned an [Atari 2600][atari] as my first ever games machine, and having cut my programming, art and game design teeth on the humble [Commodore 64][c64], I've always held a real affection for giant pixels, single-digit colour palettes, 3-channel chiptunes and generally all things 'underpowered'. It's why I've often cited the [C64 demoscene][demoscene] as an example of constraints giving rise to focused, boundary-pushing creativity, why I love trying to [replicate the effects of that era][cpdemos] on [CodePen][codepen], and why I love the concept of [#LOREZJAM][lorezjam] and other restrictive community events.

### GOSUB 200 ###

To this end - and through my usual desire to JavaScript All The Things(TM) - I've been toying with the idea of writing a very basic API for creating super-limited programs as a demo of constraint-driven creativity. I envisioned a [HTML5 Canvas][canvas] with a very low resolution, a VERY limited colour palette and nothing in the way of blend modes, alpha channels, complex paths etc - just a `screen.setPixel(x,y,v)` method. Perhaps some basic [Web Audio][webaudio] presets for sound, again heavily restricted to Atari-style beeps and buzzes. Maybe even a custom program parser and DSL, with oldschool BASIC commands instead of fancy new ES6 JavaScript - just `PRINT`, `IF-THEN`, `GOTO`, and all the other trappings of my nerdy youth. Everything else one may want for even basic programs - clearing the screen, line and arc functions, text rendering, scrolling, regions, etc - would need to be built up as library functions using these very limited tools.

I imagined really flexing my core programming skills as I first created, then worked to overcome, the strict limitations of this 'imaginary computer'. Then I Googled something recently and discovered that such a concept is actually known as a 'fantasy console', and that it's already been done.

###  Everything Old Is New Again ###

{% include img-body.html title="PICO-8 screens (credit: lexaloffle.com)" file="pico-screens.gif" %}

The [PICO-8][pico8] describes itself as 'like a real-world console, but without the inconvenience of actual hardware': a program which behaves as if it's a [NES][nes]-era games platform with the attendant restrictions on sound, graphics, CPU, memory and program size. It has a Canvas-powered low-res screen, limited palette, basic sound, yet boasts a built-in Lua code editor, sprite/tilemap tools and so on, meaning you could theoretically develop a PICO-8 game or demo right within the 'machine' itself.  It even 'boots up' like a real console, with some visual garbage giving way to a splash/loading screen in gloriously pixellated text.

~~~ bash

# specs

- display: 128x128, fixed 16 colour palette
- input: 6 buttons
- cartridge size: 32k
- sound: 4 channel, 64 definable chip blerps
- code: lua, max 8192 tokens of code
- sprites: single bank of 128 8x8 sprites + 128 shared
- map: 128x32 8-bit cels + 128x32 shared

~~~

Programs are saved and distributed, amazingly, as PNG files that [visually look like a game cartridge][cart] with label, screen grab etc, but with the actual program code embedded _within_ the PNG file - an incredible, demoscene-worthy bit of hacky verisimilitude.  [There is a wealth][awesome-pico] of internal and external tools, documentation resources and hacks available for it, just like genuine retro consoles. It runs right in the browser, can be stuck on a bootable USB or Raspberry Pi for a more authentic hardware-based experience, and best of all is actually being made available as [a legit hack-friendly handheld][pocketchip].

### Pressing buttons ###

{% include img-body.html title="PICO-8 games (credit: codyssea.com)" file="pico-games.png" %}

This has been around for over a year, with a [vibrant community][pico-twitter] and [ever-growing catalogue][pico-games] of games and demos. To be blunt, I _can't fucking believe_ I'm only discovering this now. It speaks to all my interests and fetishes in the world of retrogaming while also directly triggering the 'constrained creative' aspirations of my low-res Canvas/JS concept.

I certainly hope to still do my original idea as a programming exercise, and also because - not to be too much of a hipster - the PICO-8 _clearly_ isn't constrained enough for my tastes.  But I just love this idea of modern browser and software development birthing the perfect 'imaginary' game console that we never had in our youths.  It's very much like recent [synthwave/newretrowave][synthwave] electronica that sounds like the 80s we _deserved_ - all neon chrome and cyborg sex under Sega sunsets, and _Akira_-style corporate-shadowed street menace - instead of what we actually _got_, which was The Smiths.

Watch this space in the near future for the fruits of my lo-res Canvas experiement, but frankly I'll be too busy learning Lua and trying to make a PICO-8 roguelike to worry about it for a while, most likely.

**- NJM**

[atari]: https://www.youtube.com/watch?v=ucRYLobga0g
[c64]: https://www.youtube.com/watch?v=ZJZY98Tc7G4
[demoscene]: https://www.youtube.com/watch?v=rEZGt7aaIG4
[cpdemos]: http://codepen.io/collection/nbGQxZ/
[codepen]: http://codepen.io/
[lorezjam]: https://itch.io/jam/lowrezjam2016
[canvas]: https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial
[webaudio]: https://www.html5rocks.com/en/tutorials/webaudio/intro/
[pico8]: http://www.lexaloffle.com/pico-8.php
[cart]: http://www.lexaloffle.com/bbs/cposts/2/28747.p8.png
[awesome-pico]: https://github.com/felipebueno/awesome-PICO-8
[pocketchip]: https://getchip.com/pages/pocketchip
[nes]: https://en.wikipedia.org/wiki/Nintendo_Entertainment_System
[pico-games]: http://www.lexaloffle.com/bbs/?cat=7&sub=2&orderby=rating
[pico-twitter]: https://twitter.com/hashtag/pico8
[synthwave]: https://www.youtube.com/user/NewRetroWave/videos


