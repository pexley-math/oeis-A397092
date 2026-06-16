# Polyplets with a single corner-pinch

*Peter Exley, Independent Researcher, Brisbane, Australia. Submitted: Jun 16 2026.*

Squares on a grid can touch in two ways: along a full edge, or at a single
corner. A corner-only touch is a delicate kind of contact -- the two cells meet
at one point and nowhere else. We ask a simple question: among all the shapes you
can build by joining squares edge-to-edge or corner-to-corner, how many have
exactly one such corner-only touch?

## The problem

A *polyplet* is a set of unit cells on the square grid that is connected when
cells are allowed to touch either along an edge or at a corner (king-move, or
8-cell, connectivity). Polyplets are also called polykings or pseudo-polyominoes,
and the number of them on `n` cells -- counted up to the eight rotations and
reflections of the square -- is the well-studied sequence OEIS A030222.

Call an unordered pair of cells a *corner-pinch contact* when the two cells touch
only at a corner: they sit diagonally adjacent, and both of the cells that would
fill the gap between them along the grid axes are absent. The single shared corner
is then their only point of contact. We count the polyplets in which this happens
exactly once.

`a(n)` is the number of free polyplets on `n` cells (counted up to the symmetry
group of the square) that have exactly one corner-pinch contact.

## Definitions

A *cell* is a unit square of the integer grid, identified by its lower-left corner
`(r, c)`. Two cells are *edge-adjacent* if they share a full edge, and
*king-adjacent* if they share an edge or a corner. A *polyplet* is a finite,
king-connected set of cells. Two polyplets are the *same free shape* if one maps
to the other under a symmetry of the square (a rotation by a multiple of 90
degrees, possibly followed by a reflection) together with a translation; A030222
counts free polyplets.

Two cells `(r, c)` and `(r + s, c + t)` with `s, t` each `+1` or `-1` form a
*corner-pinch contact* when both intervening axis-neighbours, `(r + s, c)` and
`(r, c + t)`, are absent from the shape. Equivalently, the two cells are diagonal
neighbours joined through no filled edge-cell, so their only contact is the one
shared corner. Note that this is intrinsically a corner phenomenon: on the
edge-only (rook) grid diagonal cells are never adjacent, so the notion has no
edge-connected analogue.

## The values

**Result.** The number of free polyplets on `n` cells with exactly one
corner-pinch contact, for `n` from 1 to 10, is:

| `n` | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
| `a(n)` | 0 | 1 | 1 | 6 | 20 | 86 | 341 | 1406 | 5697 | 23187 |

A single cell has no pair of cells at all, so `a(1) = 0`. The first qualifying
shape is the diagonal domino at `n = 2`: two cells meeting at one corner, with both
gap cells empty. As `n` grows the shapes acquire more interior, and the qualifying
polyplets become a smaller and smaller fraction of all polyplets.

The complete family of qualifying shapes for the small cases, with the single
corner-pinch pair highlighted, is shown in [`figures.pdf`](figures.pdf).

## How we know

Every term is an exact count obtained by exhaustively enumerating the polyplets on
`n` cells and keeping those with exactly one corner-pinch contact. The enumeration
is window-free: shapes are grown one cell at a time from a single seed over the
whole grid, with no bounding box to be chosen, so there is no search region that
could be too small. Growth this way is known to generate every shape exactly once.

We confirm each count two independent ways. The first enumerator works with free
shapes directly, deduplicating under the square's symmetry group. The second works
with fixed (translation-distinct) shapes and never deduplicates by symmetry; the
two are tied by the orbit-counting identity, which says the free count, weighted by
each shape's number of symmetric images, must equal the fixed count. A missed or
double-counted shape would break the identity, so agreement certifies the count.
As an external anchor, the same machinery reproduces the published polyplet count
A030222 at every `n`. A third, fully independent program -- written from scratch,
sharing no code with the first two -- reproduces both the polyplet totals and our
counts for `n` up to 10.

## Patterns and open questions

The counts grow quickly: each is roughly four times the one before at the right-
hand end of the table. That is slower than the polyplet totals themselves, which
grow by a factor approaching six and a half, so the qualifying shapes thin out. The
fraction `a(n)` of all polyplets falls steadily -- about 0.27 at `n = 4`, about
0.03 at `n = 10` -- and tends to zero. By Madras's pattern theorem for lattice
clusters (1999), a corner-pinch contact is a local pattern that occurs in the
interior of large polyplets, so all but an exponentially small fraction of n-cell
polyplets contain many of them; the shapes with exactly one are therefore a
vanishing minority.

We found no closed form, polynomial, or finite linear recurrence for `a(n)`, and
expect none: the polyplet count itself has no closed form, its growth being
governed by a lattice-animal growth constant, and a structurally defined subfamily
inherits that intractability.

The frontier is set by memory, not time. Counting `a(n)` requires holding the
fixed-shape family in memory, and that family grows by a factor of about six per
step; on the hardware used, `n = 11` exceeds the memory budget. A machine with more
memory, or a partitioned count, would reach further.

## Further reading

This work was inspired by the OEIS and the community of contributors who maintain it.

- Sloane, N. J. A., editor. "A030222: Number of n-polyplets (polyominoes connected
  at edges or corners); may contain holes." The On-Line Encyclopedia of Integer
  Sequences. https://oeis.org/A030222
- Madras, N. (1999). "A pattern theorem for lattice clusters." Annals of
  Combinatorics, 3, 357-384. (arXiv:math/9902161)
- Redelmeier, D. H. (1981). "Counting polyominoes: yet another attack." Discrete
  Mathematics 36(2), 191-203.

## License

[CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/) -- see `LICENSE`. This work is freely available; a citation or acknowledgment is appreciated but not required.
