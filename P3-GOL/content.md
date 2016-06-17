---
title: To Life!
slug: to-life
---

Now that we have an arrangement of Cells, let’s make them alive or dead!  To do this, we'll need to keep track of all of our cells somehow.

>[action]
>Open up Grid again, and declare another private member variable:
>
>```
>private Cell[,] cells;
>```

This is a special type of array in C\# called a multidimensional array, and we'll use it to keep track of all the cells in our grid.

An individual cell of our array can be accessed like:
```
Cell cell = cells\[col,row\];
```
or by iterating using a foreach loop.

We can also set a cell in our array, once its initialized. To set the cell at column 19, row 22, for example, we would say:
```
cells[19,20] = cell;
```
where "cell" is an instance of cell.

Before we can do anything useful with it though, we need to initialize it.  When we initialize it, we need to specify how big it's gonna be.  If we want cells to represent a grid of 30 columns and 50 rows, we could initialize it by saying:

```
cells = new Cell[30,50];
```

>[action]
>Initialize the variable "cells" at the beginning of the Start method:

<!-- -->

>[solution]
>
>Your code should look like this:
```
cells = new Cell[numCols,numRows];
```
>
>It's good practice to use the variables we set up here rather than hard-coding it so that we can keep our actual code quite general. Then if we wanted to, say, change how many rows were in our grid, all we'd need to do is change one variable in one place ;)
>
>By the way, in case you're wondering why we didn’t do this inline like the rest of our member variable declarations. We couldn’t do this because we wanted to rely on numCols and numRows, which we cannot reference until we’re inside a method.

<!-- -->

>[action]
>Now populate cells with each cell.

<!-- -->

>[solution]
>
We did this by adding the following below the Cell instantiation inside the for loop in Start:
>
```
cells[col,row] = cell;
```

Be sure your code still compiles! So far, you shouldn't see any visible changes when you run the Scene, but if you save your code and navigate to Unity, once the code compiles, the Console will show you any warnings and/or errors it caught.

So that we *can* see something visual, we're going to make all our dead cells small and grey and all of our alive cells green and regular size.  To do this, we'll need some way to tell if a cell is alive or dead.

>[action]
>Open Cell and add a private bool named "isAlive" to it.

>[solution]
>
>Your code should look like this:
```
private bool isAlive;
```
>Since we’re never assigning this bool, it will always be false, because
C\# values default to 0 or null when initialized without assignment.

<!-- -->

>[action]
>Now in the Update method, add code to makes this cell green and regular sized if it's alive or grey and small if it's dead.  As a hint, you can set the size of a cell to be size 5 by saying:
```
transform.localScale = Vector3.one * 5;
```
> and we've written a Utility function you should use to set the color:
```
Utilities.ChangeCellColor(this,color)
```
>where color is of type Color. Color.green is a green color, and Color.grey is a grey color.

>[solution]
>
>Our code looks like this:
>
```
float scaleFactor = isAlive ? 1 : 0.5f;
transform.localScale = Vector3.one * scaleFactor;
Color color = isAlive ? Color.green : Color.grey;
Utilities.ChangeCellColor(this,color);
```
>
>If the '?' notation is new to you, this is what’s known as “syntactic sugar,” or a prettier version of, the
following:
>
```
float scaleFactor;
>
if (cell.isAlive) {
  scaleFactor = 1;
} else {
  scaleFactor = 0.5f;
}
```
>
>Neither is any more correct than the other, one is just more concise;
however, if you prefer the longer version because it is more clear, by
all means **USE IT!** Code is meant to be readable!

Save the components and run the Scene. All your Cubes should appear
smaller than before!

![](../media/image56.png)

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
