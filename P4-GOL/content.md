---
title: Evolution Revolution
slug: evolution
---

Now that we’re convinced our grid appropriately displays Cells as small
or big to show whether or not they’re dead or alive, let’s add the logic
that will make our population of cells evolve!

First, we’ll need to set an evolution speed. We won’t hook it up to our
UI just yet; we want it to work first, then be interactive, so we’ll
pick a default value.

>[action]
>Add the following member variables to Grid:
>
>```
>private float evolutionPeriod = 0.5f;
>private float evolutionTimer;
>```
>
>Then add the following to the Update method:
>
>```
> evolutionTimer -= Time.deltaTime;
> if (evolutionTimer < 0) {
> evolutionTimer = evolutionPeriod;
> Debug.Log("Tick");
> }
>```

Save the components, run the Scene, and look in the Console. You should
see “Tick” logged every half-second.

![](../media/image55.png)

What we’ve done here is create a very simple timer. The variable
evolutionTimer starts out at 0, and every frame, we subtract from it the
amount of time that passed between this frame and the previous one
(Time.deltaTime).

Whenever evolutionTimer becomes negative, we bring it back up to the
value of the period we want for our timer.

Time.deltaTime, by the way, is not necessarily a constant; even if we’re
running at, say, 60 frames-per-second, we have no guarantee that each
frame will take exactly 1/60th of a second to process! If a frame takes,
say, 10 seconds to process, our timer won’t give us 20 ticks, but that’s
okay. The user will see at most the next tick, no matter how long a
cycle takes.

In order to update the grid, we’ll need to add the rules to the Game of
Life where this “Tick” is being logged.

In order to ensure that the values of cells that have already been
calculated don’t affect cells that haven’t yet been calculated, we’re
going to make our Cells evolve in two steps.

First, we’re going to calculate the state that each cell should have on
the next frame.

Then, we’re going to assign each cell to have the correct state.

>[action]Create a two new member variables in Cell:
>
>```
> private bool isAliveNext;
> private bool _isAlive;
>```

Note the underscore; this is just to prevent a name collision with
isAlive.

>[action]
>Then change the member variable isAlive to look like the following:
>
>```
> public bool isAlive {
>
> set {
> isAliveNext = value;
> }
>
> get {
> return _isAlive;
> }
>}
>```

This funny notation here is C\#’s getter/setter notation. The “get”
value is returned when you attempt to retrieve the value of the property
isAlive, and the “set” method is called when you assign a value (held in
the variable named “value”) to isAlive.

>[action]
>Add the following method to Cell:
>
>```
> public void UpdateIsAlive() {
> 	_isAlive = isAliveNext;
> }
>
```

<!-- -->

>[action]
> Now open Grid. Create a new method in Grid called Evolve:
>
>```
> 	private void Evolve() {
>	}
>```
>
>And replace the Debug.Log(“Tick”) line with:
>
>```
> Evolve();
>```
>inside Evolve, add the following code:
>
>```
>for (int col = 0; col < cells.GetLength(0); ++col) {
>for (int row = 0; row < cells.GetLength(1); ++row) {
>
>	int numAliveNeighbors = GetNumAliveNeighbors(col,row);
>
>	Cell cell = cells[col,row];
>
>	if (cell.isAlive) {
>
>		if (numAliveNeighbors < 2 || numAliveNeighbors > 3) {
>			cell.isAlive = false;
>		} else {
>			cell.isAlive = true;
>		}
>	} else if (!cell.isAlive && numAliveNeighbors == 3) {
>	cell.isAlive = true;
>		}
>	}
>}
>
>foreach (Cell cell in cells) {
>	cell.UpdateIsAlive();
>}
>```

<!-- -->

>[action]
>Then create a new method in Grid called GetNumAliveNeigbors with the
following definition:
>
>```
>private int GetNumAliveNeighbors(int colCenter, int rowCenter) {
>int numAliveNeighbors = 0;
>
>for (int dCol = -1; dCol <= 1; ++dCol) {
> for (int dRow = -1; dRow <= 1; ++dRow) {
>
>if (dCol == 0 && dRow == 0) {continue;}
>
>int col = colCenter + dCol;
>int row = rowCenter + dRow;
>
>if (col < 0 || col >= cells.GetLength(0) ||
>row < 0 || row >= cells.GetLength(1)) {continue;}
>
>if (cells[col,row].isAlive) {
>++numAliveNeighbors;
>}
>}
>}
>
>return numAliveNeighbors;
>}
>```

Save everything, then run the Scene!

![](../media/image34.gif)

For an easier-to-recognize pattern, you could also use a simple 3-pixel
oscillator instead:

![](../media/image61.gif)

Our GetNumAliveNeighbors method looks one row and one column around a
given cell to see if the cell at that location is alive or not. If it
is, it adds it to the count.

The lines:

```
if (col < 0 || col >= cells.GetLength(0) ||
row < 0 || row >= cells.GetLength(1)) {
continue;
}
```

prevent the check from going out of bounds in the case that the Cell is
at the edge of our grid (like (0,0), for example).

But… why not wrap around instead? Then gliders could propagate forever!

>[action]
>Replace those lines with the following:
>
>```
>if (col < 0) {col = cells.GetLength(0) - 1;}
>if (col >= cells.GetLength(0)) {col = 0;}
>
>if (row < 0) {row = cells.GetLength(1) - 1;}
>if (row >= cells.GetLength(1)) {row = 0;}
>```

![](../media/image39.gif)

If you want to test this with a glider, try the coordinates (2,3),
(3,2), (4,2), (4,3), and (4,4):

![](../media/image63.gif)
