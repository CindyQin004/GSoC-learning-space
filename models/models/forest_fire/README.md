# Forest Fire Model (Extended Version)

## Summary

This is an extended version of the classic Forest Fire model. The forest is a grid of cells, and fire starts from the leftmost column and spreads until there are no burning trees left.

Compared to the original, I made three modifications: wet trees, a firebreak column, and changing the fire spread direction from 8 to 4.

## How to Run

Open and run all cells in:
```
forest_fire_model.ipynb
```

## Three Modifications

**1. Wet Trees**
Some trees start out wet and are completely immune to fire. The proportion is controlled by `wet_probability`.

**2. Von Neumann Neighborhood (4 directions)**
The original model spreads fire in 8 directions. I changed it to only spread up, down, left, and right. Experiments showed that significantly more trees survive under 4-directional spread (99 surviving vs. 0), because the fire path is narrower and easier to block.

**3. Firebreak Column**
One column in the grid is left completely empty. Fire cannot jump over it, so trees on the right side of the firebreak survive.

## Parameters

- `width` (default 100): grid width
- `height` (default 100): grid height
- `density` (default 0.65): probability of a cell having a tree
- `wet_probability` (default 0.1): probability of a tree being wet
- `firebreak_column` (default 5): which column to leave empty as a firebreak

## What I Learned

**The most interesting part** was the firebreak logic. At first I had no idea how to implement it because I didn't fully understand how fire spreads. Once I realized that fire can only reach adjacent cells, the solution became obvious — just skip placing trees in one column. The implementation ended up being a single `if` statement inside the tree-placement loop: if the cell's x coordinate equals `firebreak_column`, skip it.

**Where I got stuck** was the wet tree probability check. I originally wrote `== 0.1`, but the chance of a random number being exactly 0.1 is essentially zero, so wet trees never appeared. Changing it to `< 0.1` fixed it — any random number between 0 and 0.1 counts as a wet tree, giving exactly 10% probability.

**What surprised me** was how big the difference is between 4-directional and 8-directional spread. With 4 directions, 99 trees survived. With 8 directions, none survived. The reason is that 8-directional spread allows fire to travel diagonally, which lets it go around obstacles — almost no tree can escape.

## What I Would Do Next

- Add a Solara visualization to watch the fire spread in real time
- Test different firebreak widths to find the minimum effective width
- Vary `wet_probability` to find the threshold where wet trees meaningfully slow the fire
