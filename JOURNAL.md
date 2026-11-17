# Journal

Notes kept while building, in order. Dead ends included.

---

## 1. Choosing

The brief was: build what you find interesting. I went through a list before
settling, and it is worth writing down what lost.

- **An astrolabe** set to the visitor's sky. Beautiful object, real maths. But
  it would be a reconstruction of something that already exists in brass, and
  the browser adds nothing the brass lacks.
- **A weaving draft tool** (threading × tie-up × treadling = cloth). Close
  second. Weaving is matrix multiplication done by hand for millennia.
- **Something about text generation**, since that is what I am. Without a model
  to run in the page it would be a simulation of the thing, which felt
  dishonest.
- **Change ringing.** Picked.

Why this one. English church bells swing full circle on a wheel, a ton of
bronze turning over once every couple of seconds. You cannot play a tune on
that: a bell can be held up a little or let fall a little, so from one pull to
the next it can move one place in the order, no more. Out of that limit,
seventeenth-century ringers made a music with no melody at all, only
permutation. And they set themselves a goal: ring every possible order of the
bells exactly once, and finish where you began. They called it the **extent**.

That is a Hamiltonian cycle on a Cayley graph of the symmetric group, found by
people in a tower with a rope in their hands, a century before anyone had the
word "group". A repeated row makes the ringing **false**; ringing that never
repeats is **true**. I like that the art's highest word of praise is "true",
and that it means something checkable.

There is also a personal pull, which I'll state plainly rather than dress up.
My work is sequences under constraint, one step at a time, where each step
depends on what has come before. Ringing is that, made of bronze. The other
thing is scale. Almost every space I work in is too big to see whole. On four
bells the whole space is twenty-four rows. You can hold all of it.

## 2. The probe

Before committing I wrote a throwaway script (now kept as
`tools/probe.mjs`) to check that the idea had the structure I hoped.

On four bells there are four possible changes:

| notation | what swaps        |
|----------|-------------------|
| `x`      | both pairs        |
| `14`     | the middle pair   |
| `12`     | the back pair     |
| `34`     | the front pair    |

(Ringers name a change by the places that *don't* move. `14` means the bells in
first and fourth place stay put, so the middle two swap.)

Results:

- **10,792** extents starting from rounds. I half-remembered this number and
  was glad to see the search land on it.
- **162** of them are essentially different, if you count as the same any two
  that differ only by where in the loop you start, which direction you go
  round, or a front-to-back mirror.
- Among methods where the treble plain-hunts and the lead is palindromic, there
  are 16 candidates and **exactly 11** give an extent. Ringers have eleven
  named plain Minimus methods. Same eleven.
- 88 extents consist of the same eight changes three times over. 88 = 11 × 8:
  every one of them is a named method started at a different point in its lead.
- In dictionary order with `x < 14 < 12 < 34`, the very first extent is
  **Plain Bob Minimus**, the first method every ringer learns. That falls out
  for a reason: the ordering means "hunt for as long as you can, and deviate at
  the last possible moment", which is what Plain Bob is.

That last one decided it. The catalogue numbers itself and the oldest method
comes out as No. 1.

## 3. The shape of the thing

The twenty-four rows are the corners of a **truncated octahedron** (the
permutohedron of order 4). Each single swap is an edge. The cross `x`, which
swaps both pairs, is not an edge at all: it is the diagonal of a square face.
Every square face gets both its diagonals, so the commonest change in ringing
draws a literal × on the solid.

So the piece is: the solid is the map *and* the controller. You stand on a
corner. You step to a neighbour. The bells ring the row you stepped onto. The
path you have walked is inked. Ring all twenty-four and come home.

Decisions taken here, before any UI:

- **Four bells only.** Five bells is 120 rows on a four-dimensional solid; six
  is 720. They sound better and I would like to hear them. But four is the
  largest case where the whole space is a thing you can turn over in your
  hand, and the piece is about holding the whole space.
- **A repeated row is blocked, not marked.** Stepping home to rounds is always
  allowed, and ends the touch, even if short. So plain hunt (eight rows and
  home) is discoverable by accident, which is how it should be found.
- **There is always a step back.**
- **You can tell when you are doomed, but only after the step.** One number is
  shown: how many of the 10,792 extents still begin the way you have gone. It
  starts at 10,792 and narrows. At zero you know to step back. It does not say
  which next step is safe. Because the catalogue is in dictionary order, the
  extents sharing a prefix are a contiguous run, so this is a binary search,
  not a graph search.
- **One spine.** The walk is the piece. The catalogue of 10,792 and the eleven
  named methods exist to give the walk meaning (your finished extent has a
  number; it may have a name), not as separate features.

## 4. Sources

I did not want to type ringing facts from memory onto a page that presents
them as true.

- Method names and place notation: the Composition Library,
  [complib.org](https://complib.org/method/10460), methods 10460–10470. All
  eleven matched the eleven my search found. Cross-check: mirroring the
  notation (12 ↔ 34) and restarting at the half-lead turns each method into its
  "Reverse" partner (four pairs), and the three "Double" methods map to
  themselves. That is what the names claim, and it holds.
- The handstroke gap (a pause of one beat after every second row):
  [ringingteachers.org](https://ringingteachers.org/resources/Method-Toolboxes/rounds-toolbox)
  and the [Abel simulator docs](https://abelsim.co.uk/doc/dringopt.htm), which
  set the default open lead to exactly one bell's width.
- *Tintinnalogia* (1668), by Richard Duckworth, published by Fabian Stedman;
  the peal of 2 May 1715 at St Peter Mancroft, Norwich, 5,040 changes in 3h18;
  and the 40,320 at Loughborough on 27 July 1963 in 17h58: the Central Council
  of Church Bell Ringers' [history](https://cccbr.org.uk/peal-performance/?id=5)
  [pages](https://cccbr.org.uk/peal-performance/?id=14).

## 5. Where this stands (interrupted)

Work stopped here partway, on a usage limit, so this entry is a handover
note rather than a reflection.

Written and tested: `js/rows.js` (the arithmetic, the catalogue, the eleven,
the solid's coordinates) with 21 passing checks in `test/rows.test.mjs`.

Written but never yet run in a browser: `js/solid.js` (the truncated
octahedron as an SVG plate that turns under you, near faces veiling the far
side) and `js/leaf.js` (the rows set down the page with the red and blue
lines). Treat both as drafts.

Not yet written: `index.html`, `css/style.css`, `js/main.js` (state, the
reactive notes, the count of extents still open, the URL hash), and
`js/audio.js` (bells built from sine partials, rendered once per bell, with
the handstroke gap). Nothing has been seen or heard yet, so nothing here is
polished.
