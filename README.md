# nonotypst

Typst helpers for rendering nonogram boards in a flexible manner, offering a core, `draw-board`, which computes or accepts clues, lays out the board, and delegates the actual drawing of puzzle and clue cells to callbacks. Two ready-to-use implementations are included:

- `classical-board` for a more classical and practical square-grid presentation.
- `modern-board` for a more modern visual style.

Both offer numerous options for customization of the board, but deeper customization is possible by implementing alternative callbacks and passing them to the `draw-board` function.

## The board matrix

The board matrix is a 2D array where 0s represent empty cells (not included in the clues) and other values represent filled cells. By default, 1s are used to represent standard cells, and short strings are used to represent colored ones (e.g., "r" for red, "g" for green, etc.). The package can be configured to use other values as needed, as well as different colors, content, and styles.

## Gallery

### Solved boards

<p>
  <img src="examples/Example%2001.svg" alt="Solved monocolor board in classical style" width="32%">
  <img src="examples/Example%2002.svg" alt="Solved monocolor board in tweaked classical style" width="32%">
  <img src="examples/Example%2003.svg" alt="Solved monocolor board in modern style" width="32%">
</p>

<p>
  <img src="examples/Example%2004.svg" alt="Solved multicolor board in classical style" width="32%">
  <img src="examples/Example%2005.svg" alt="Solved multicolor board in tweaked classical style" width="32%">
  <img src="examples/Example%2006.svg" alt="Solved multicolor board in modern style" width="32%">
</p>

### Unsolved boards

<p>
  <img src="examples/Example%2007.svg" alt="Unsolved monocolor board in classical style" width="32%">
  <img src="examples/Example%2008.svg" alt="Unsolved monocolor board in tweaked classical style" width="32%">
  <img src="examples/Example%2009.svg" alt="Unsolved monocolor board in modern style" width="32%">
</p>

### Only solution

<p>
  <img src="examples/Example%2010.svg" alt="Compact solution-only rendering" width="20%">
</p>

### Board created from clues

<p>
  <img src="examples/Example%2011.svg" alt="Board rendered from manual clues" width="20%">
</p>

## Usage

The code corresponding to the examples of use and customization in the provided images can be seen in `examples/examples.typ`.

An example of a minimal use of the `classical-board` implementation is:

```typst
// 5x10 board
#classical-board(
  (
    (0, 0, 1, 1, 0, 1, 1, 1, 1, 0),
    (0, 0, 1, 1, 0, 1, 1, 1, 1, 0),
    (0, 1, 0, 0, 0, 0, 0, 0, 0, 0),
    (1, 1, 0, 0, 0, 0, 0, 1, 1, 0),
  ),
  show-solution: true,
)
```

## Core API

`draw-board` is the core function exposed by the package. It is responsible for:

- accepting a `board-matrix`, or inferring board size from `column-clues` and `row-clues`
- computing missing clues when a solution matrix is provided
- laying out the clue area, corner cell, and puzzle grid
- calling injected callbacks to render each board cell and clue cell

Its main rendering hooks are:

- `cell-drawer` for puzzle cells
- `column-cell-drawer` for column clues
- `row-cell-drawer` for row clues
- `corner-cell-drawer` for the top-left spanning corner cell

If you want a custom appearance, build it on top of `draw-board` by supplying your own callbacks. If you want ready-made styles, use `classical-board` or `modern-board`.

## Files

- `core.typ` contains `draw-board` and the clue-processing logic.
- `implementations.typ` contains the provided drawer callbacks plus the `classical-board` and `modern-board` wrappers.
- `lib.typ` re-exports the package API.
- `examples/examples.typ` contains the example document used to generate the SVG gallery.