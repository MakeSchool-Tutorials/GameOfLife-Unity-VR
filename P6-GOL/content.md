---
title: 3D
slug: all-the-dees
---

#3D

All the Dees. All of them.

Actually, making the game 3D isn’t terribly complicated. It just
involves giving our cells one more dimension and changing our rules to
work better, now that a Cell can be surrounded by 26 Cells instead of 8.

Let’s do it!

>[action]
>First, change the cells member variable to allow a third dimension by
changing it to be:
>
>```
>private Cell[,,] cells;
>```

It may look very similar to before; we’ve just added a comma.

>[action]
>Next, add the follow member variable:
>
>```
> private int numLays = 5;
>```

We’re using the term “layers” to refer to this new dimension we’re
adding to our grid, hence “lay” for short.

>[action]
>Add this information to the instantiation of our multidimensional array
of cells by changing that line in the Start method to be:
>
>```
>cells = new Cell[numCols,numRows,numLays];
>```

<!-- -->

>[action]
>Nest a third for loop anywhere you have two and rename the variables as
appropriate. For example, in Start, you’d add:
>
>```
> for (int lay = 0; lay < numLays; ++lay) {
>```
>
>under:
>
>```
> for (int row = 0; row < numRows; ++row) {
>```

Be sure to close each new for loop you add with a closing curly brace!

Anywhere you get a Cell using cells\[col,row\], change that to use your
third parameter. For example, in Start, that would change to
“cells\[col,row,lay\]”

Be sure that GetNumAliveNeighbors gets a third parameter added, that its
first continue conditional include information about the third
dimension, and that the third dimension is adjusted like the rest!

Either get rid of the code in Start that we were using to set an initial
condition, or else add an extra parameter to it.

>[action]
>Finally, change the positioning code that sets where the Cells are, in
the Start method to:
>
>```
> float x = (col + 0.5f - numCols * 0.5f) * (cellSideLength + margin);
>
> float y = (row + 0.5f - numRows * 0.5f) * (cellSideLength + margin);
>
> float z = (lay + 0.5f - numLays * 0.5f) * (cellSideLength + margin);
>
> cell.transform.localPosition = new Vector3(x,y,z);
>```

Note that we’ve changed from a Vector2 to a Vector3. This is actually
okay because localPoisition is *really* a Vector3. When we assigned from
a Vector2, Unity was smart and just filled in a 0 for that 3rd
parameter.

All right. You’ve made a lot of changes. Take a deep breath.

Save the component, run the Scene, and hit Randomize.

![](../media/image52.png)

Did you get an error when you tried to run the Scene? If so,
double-click the error in the Console and it should take you to the spot
in your code that made Unity sad. See if you can figure out what the
issue is (a likely culprit is a missing curly brace). If you want help
though, of course ask staff! We’re here to help!
