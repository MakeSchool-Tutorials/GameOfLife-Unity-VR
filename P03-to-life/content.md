---
title: To Life!
slug: to-life
---

Now that we have an arrangement of Cells, let’s make them alive or dead!  To do this, we'll need to keep track of our cells somehow.

> [action]
>
Open up Grid again, and declare another private member variable:
>
```
private Cell[,] cells;
```

This is a special type of array in C# called a _multidimensional array_, and we'll use it to keep track of all the cells in our grid. An individual cell of our array can be accessed by indexing in. To get the cell at column 42, row 17, we could say:

```
Cell cell = cells[42,17];
```

or by iterating using a `foreach` loop.

We can also set a cell in our array, once its initialized. To set the cell at column 19, row 22, for example, we would say:

```
cells[19,20] = cell;
```

where "cell" is an instance of Cell.

# Initializing the cells

Before we can do anything useful with cells though, we need to initialize it. When we initialize it, we need to specify how big it's gonna be. If we want cells to represent a grid of 30 columns and 50 rows, we could initialize it by saying:

```
cells = new Cell[30,50];
```

> [action]
>
Initialize the variable "cells" at the beginning of the `Start` method. Use the sample code above to try it out before checking the solution!

<!-- -->

> [solution]
>
Your code should look like this:
>
```
cells = new Cell[numCols,numRows];
```

It's good practice to use the variables we set up here rather than hard-coding it so that we can keep our actual code generalized. Then if we wanted to change the number of rows in our grid, all we'd need to do is change one variable in one place ;)

> [info]
>
By the way, in case you're wondering why we didn’t do this inline like the rest of our member variable declarations. We couldn’t do this because we wanted to rely on numCols and numRows, which we cannot reference until we’re inside a method.

# Populating the grid

> [action]
>
Now populate cells with each cell.

<!-- -->

> [solution]
>
We can do this by adding the following below the Cell instantiation inside the for loop already in `Start`:
>
```
cells[col,row] = cell;
```

Be sure your code still compiles! You shouldn't see any visible changes when you run the Scene, but if you save your code and navigate to Unity, once the code compiles, the Console will show you any warnings and/or errors it caught.

# Death cometh to cells

To see something visual, we're going to make all the dead cells small and gray and the our alive cells green and regular size. To do this, we'll need some way to tell if a cell is alive or dead.

> [action]
>
Open Cell and add a private bool named `isAlive` to it.

<!-- -->

> [solution]
>
Your added code should look like this:
>
```
private bool isAlive;
```

<!--  -->

> [info]
>
Since we’re never assigning this bool, it will always be `false`, because C# values default to `0` or `null` when initialized without assignment and C# interprets this as `false`.

## Visualizing life and death

Let's add code to makes this cell green and regular sized if it's alive or gray and small if it's dead. As a hint, you can set the size of a cell to be size `5` by saying:

```
transform.localScale = Vector3.one * 5;
```

and we've written a Utility function you should use to set the color:

```
Utilities.ChangeCellColor(this, color);
```

where color is of type `Color`. `Color.green` is a green color, and `Color.gray` is a gray color.

> [action]
>
Now in the `Update` method, add code to makes a cell green and regular sized if it's alive or gray and small if it's dead.

<!--  -->

<!-- FIXME: the Utilities.ChangeCellColor function is written in a rather hacky way that may cause performance issues; not a huge issue, but it should be changed  -->

> [solution]
>
Our code looks like this:
>
```
float scaleFactor = isAlive ? 1 : 0.5f;
transform.localScale = Vector3.one * scaleFactor;
Color color = isAlive ? Color.green : Color.gray;
Utilities.ChangeCellColor(this,color);
```

<!--  -->

> [info]
>
If the '?' notation is new to you, this is what’s known as `syntactic sugar,` or a prettier version of, the following:
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
Neither is any more correct than the other, one is just more concise. If you prefer the longer version because it is more clear, by all means **USE IT!** Code is meant to be readable!

## Check your work

> [action]
>
Save the components and run the Scene. The `Cubes` should appear smaller than before!
>
![The Cubes are smaller than before](../media/image56.png)

# Add some life!

But how can we be sure our code isn’t shrinking the `Cells` unconditionally? Let’s add some test code.

> [action]
>
Change Cell’s isAlive method from private to public:
>
```
public bool isAlive;
```
>
Then put the following at the bottom of the Start method of Grid:
>
```
cells[2,3].isAlive = true;
cells[2,4].isAlive = true;
cells[5,3].isAlive = true;
cells[5,4].isAlive = true;
cells[1,1].isAlive = true;
cells[6,1].isAlive = true;
cells[2,0].isAlive = true;
cells[3,0].isAlive = true;
cells[4,0].isAlive = true;
cells[5,0].isAlive = true;
```

When you save and run this, you should see this totally arbitrary and entirely randomly-chosen pattern appear:

![Smiley McSmileFace](../media/image53.png)

> [action]
>
Go ahead an remove the code you just added and change `isAlive` back to private.
