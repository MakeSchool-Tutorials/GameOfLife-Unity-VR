---
title: To Life!
slug: to-life
---

Now that we have an arrangement of Cells, let’s make them alive or dead!

>[action]
>Open up Grid again, and declare another private member variable:
>
>```
>private Cell[,] cells;
>```

This is a special type of array in C\# called a multidimensional array.
A cell in cells can then be accessed like cells\[col,row\] or by
iterating using a foreach loop, both of which we’ll eventually use.

>[action]
>Add the following before the for loops in the Start method:
>
>```
>cells = new Cell[numCols,numRows];
>```

This line initializes our array of cells. You may be wondering why we
didn’t do this inline like the rest of our member variable declarations.
We couldn’t do this because we wanted to rely on numCols and numRows,
which we cannot reference until we’re inside a method.

>[action]
>Add the following below the Cell instantiation inside the for loop in
Start:
>
>```
>cells[col,row] = cell;
>```
>
>Then open Cell and add the following to the Update method:
>
>```
>float scaleFactor = isAlive ? 1 : 0.5f;
>transform.localScale = Vector3.one * scaleFactor;
>Color color = isAlive ? Color.green : Color.grey;
>Utilities.ChangeCellColor(this,color);
>```
>
>and add the following member variable to it:
>
>```
>private bool isAlive;
>```

Since we’re never assigning this bool, it will always be false, because
C\# values default to 0 or null when initialized without assignment.

Save the components and run the Scene. All your Cubes should appear
smaller than before!

![](../media/image56.png)

How have we done this? Well, our multidimensional array stores a Cell
for each spot in our grid. Each cell has an isAlive flag, which is set
to false by default.

In our Update method, we’re looking at each Cell in our array of Cells
and setting the size of the Cell (transform.localScale)l based on
whether or not that cell is alive. The property transform.localScale is
a Vector, with one component for each of the 3 spatial dimensions the
Game Object occupies. We haven’t initialized a new Vector in this case,
but instead multiplied a constant Vector of length (1,1,1) -- Vector.one
-- by a scalar to get a vector with each component scaled up.

The line:

```
float scaleFactor = isAlive ? 1.0f : 0.5f;
```

is what’s known as “syntactic sugar,” or a prettier version of, the
following:

```
float scaleFactor;

if (cell.isAlive) {
  scaleFactor = 1;
} else {
  scaleFactor = 0.5f;
}
```

Neither is any more correct than the other, one is just more concise;
however, if you prefer the longer version because it is more clear, by
all means **USE IT!** Code is meant to be readable!

You may also have noticed the “this” used in our Utility function used
to set the color. This is because the Utility function requires a
parameter of type Cell. “this” is the word C\# uses within a class to
refer to the instance itself.

But how can we be sure our code isn’t just shrinking all of our Cells
unconditionally? Let’s add some test code.

>[action]
>Change Cell’s isAlive method from private to public:
>
>```
>public bool isAlive;
>```
>
>Then put the following at the bottom of the Start method of Grid:
>
>```
>cells[2,3].isAlive = true;
>cells[2,4].isAlive = true;
>cells[5,3].isAlive = true;
>cells[5,4].isAlive = true;
>cells[1,1].isAlive = true;
>cells[6,1].isAlive = true;
>cells[2,0].isAlive = true;
>cells[3,0].isAlive = true;
>cells[4,0].isAlive = true;
>cells[5,0].isAlive = true;
>```

When you save and run this, you should see this totally arbitrary and
entirely randomly-chosen pattern appear:

![](../media/image53.png)
