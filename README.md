# chord console

A chord player you drive from a command line. Type a chord, hit enter, and it holds
until you type the next one. Play a progression through once and it works out your
tempo, then loops it back.

## Running it

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 4173
```

Or on the web: https://socerest2.github.io/chord-console/

Browsers block audio until you interact with the page, so click into the prompt once
before playing.

## Chords

Type a root plus a quality: `c`, `cm`, `c7`, `cmaj7`, `cm7`, `c9`, `cadd9`, `csus4`,
`cdim`, `caug`, `cm7b5`, `c6`, `c11`, `c13`, and more. Sharps and flats work
(`f#m7`, `bbmaj7`), as do longhand names (`gmaj`, `amin7`) and slash chords for
inversions (`c/e` is C major with E in the bass).

Autocomplete suggests qualities as you type. **Tab** accepts, **↑↓** picks from the
list, **enter** plays. **↑** on an empty prompt walks back through history.

## Commands

| Command | What it does |
| --- | --- |
| `key g`, `key am` | Shows that key's seven chords as clickable buttons, and ranks autocomplete by what's diatonic |
| `bpm 92`, `bpm off`, `click off` | Tempo and metronome |
| `oct -1`, `alt+↑↓` | Shifts the whole voicing by octaves, re-voicing a held chord live |
| `roll`, `arp`, `noodle`, `pad` | Play styles |
| `pluck 1 3 5 3` | Repeating pattern of chord tones; `.` is a rest, `pluck off` stops |
| `1`–`8` | Pluck individual chord tones by hand while a chord holds |
| `cmaj7 p`, `p`, `p on/off` | Keep a chord in the progression |
| `prog c am f g`, `clear` | Set or empty the progression outright |
| `loop`, `loop 2`, `loop off` | Loop the progression, optionally N bars per chord |
| `fill`, `fill big`, `fill on/off` | Drum fill now, or automatic ones into each chord change |
| `rec`, `rec stop` | Record what you play and download a `.wav` |
| `stop`, `esc` | Silence everything |
| `help` | Full command list in the console |

## Parts

Record a named section by playing it:

```
loop start verse
cmaj7          (hold it as long as you want)
am7
fmaj7
loop end
```

It times your chord changes, infers the tempo, snaps the total to whole bars, and
starts looping. After that, typing `verse` plays it — switching waits for the next
chord change so it lands musically. Build a `chorus` and a `bridge` the same way and
move between them by name.

Set `bpm` before recording and it uses your tempo instead of inferring one, which is
what you want when covering a song at a known tempo.

`sections` lists them, `loop delete verse` removes one.

## Notes

Nothing is stored.

Audio is synthesized with the Web Audio API: a detuned sawtooth pad with a triangle
bass, plucked tones for the arp and pluck patterns, and synthesized drums for fills.
Recording taps the post-compressor mix and encodes 16-bit stereo WAV in the browser.
