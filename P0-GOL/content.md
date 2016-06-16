---
title: "What's the Game of Life?"
slug: whats-the-game-of-life
---

In this tutorial you are going to familiarize yourself with C\# by making both a 2D and 3D version of Conway's Game of Life in Unity. If you've never heard of Conway's Game of Life, [*Wikipedia*](http://en.wikipedia.org/wiki/Conway%27s_Game_of_Life) has a great article. Nearly every programmer has written a version of this game at some point in their careers and wasted lots of time staring at cool shapes morphing. Consider this your initiation :p

###TL;DR

There is a grid of cells. A cell is either alive or dead. If a cell has less than two live neighbors, it dies. If it has more than three neighbors, it dies. If a live cell has exactly two or three neighbors, it stays alive and if a dead cell has exactly three neighbors, it comes to life.

Try placing a few live cells and then hitting the next button to run one round. The Wikipedia article has some great examples of common patterns that produce cool effects. Press the animate button to continuously run the game. Play around with it a little and come back when you're ready.

You can check it out [*here*](https://jsfiddle.net/makeschool_dion/zose7rv3/embedded/result/).

In order to focus on C\# rather than Unity, we’ve created a base project that has visual pieces hooked into some code. Together, we’re going to be implementing the code that makes the game work!
