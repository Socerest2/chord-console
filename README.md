# chord console

A chord player you drive from a command line. Type a chord, hit enter, and it holds
until you type the next one. Play a progression through once and it works out your
tempo, then loops it back.

## Running it

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 4173
```

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

## The beat

A step grid on the same clock as the chords, so a drum track and a chord loop stay
locked together.

```
beat funk                    a starting point
kick x...x...x...x...        or set a lane yourself
beat swing 30                push the offbeats late
beat off
```

Presets: `basic` `rock` `four` `funk` `trap` `boom` `bossa`. Lanes: `kick` `snare`
`hat` `open` `clap` `tom` — each takes a string of sixteenths where `x` is a hit and
`.` is a rest. Short patterns repeat to fill the bar, so `kick x...` gives you four
on the floor. Click cells in the grid to toggle them, click a lane name to clear it.

`tap` plays one in live against the click: **k** kick, **s** snare, **h** hat,
**o** open hat, **c** clap, **t** tom. Each hit lands on the nearest sixteenth.
`done` when you have it.

`beat bars 2` for a two-bar pattern, `beat show` prints it as text, `beat clear`
empties it.

## Singing an idea in

`sing` opens the microphone. Hum or sing a melody, type `done`, and it turns what it
heard into notes:

```
> sing
listening — hum or sing your idea, then type "done"
> done
heard 6 notes at 120 bpm — C4 E4 G4 F4 A4 C5
"melody" plays it back · "harmonize" puts chords under it
```

`melody` replays the transcription. `harmonize` works out what key it's in, picks a
chord for each bar that fits the notes you sang, and fills the progression — so
`loop` plays your idea back as a song part.

Set a tempo first (`bpm 100`) and the notes line up to it. Otherwise it estimates one
from how you sang.

This is ordinary signal processing, not a model: it finds the pitch of each frame of
audio by normalised autocorrelation, groups frames that agree into notes, and snaps
the lengths to the beat. It needs a reasonably quiet room and a steady hum — it
ignores anything too faint to be sure about rather than guessing.

## Writing words

`write` turns the prompt into a lyric sheet laid over the bars you've got. Each line
you type fills the next bar and gets counted against that bar's beats, so you can see
when a line is crowded or leaving space:

```
PROGRESSION                              4 bars · 4 written
 1  Cmaj7      woke up on the wrong side          6 syl
 2  Am7        of a borrowed afternoon            7 syl
 3  Fmaj7      the radio was playing              6 syl
 4  G7         something close to out of tune     8 syl
```

Type `/loop` while writing and the bar that's currently sounding is marked as it goes
round, so you can sing against it while you work.

In write mode: `back`, `next`, `del`, `sheet` to print it as plain text, `done` to
leave. Anything starting with `/` runs a console command without dropping out.

Each part keeps its own lyrics, so `verse` and `chorus` don't overwrite each other.

`rhyme moon` suggests rhymes. It matches on spelling rather than pronunciation, so
it's approximate — a nudge rather than a dictionary.

## On a phone

`mobile.html` is a touch version: twelve root notes laid out in fifths, so neighbours
on the pad are neighbours in the music. Hold a root and a wheel of chord shapes fans
out around your thumb — slide to one and release. Sliding further out swaps the shape
for its extended version (`7` becomes `9`, `m7` becomes `m11`, and so on), and a plain
tap gives you the major triad.

The chord changes as you slide, so you can hear it move before you commit.

In Safari, share sheet → **Add to Home Screen** installs it full-screen with its own
icon. It runs entirely offline once loaded.

## Notes

Nothing is stored.

Audio is synthesized with the Web Audio API: a detuned sawtooth pad with a triangle
bass, plucked tones for the arp and pluck patterns, and synthesized drums for fills.
Recording taps the post-compressor mix and encodes 16-bit stereo WAV in the browser.
