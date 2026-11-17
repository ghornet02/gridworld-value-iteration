# Value Iteration on a Stochastic Grid World

Value iteration and policy extraction for a 4x4 grid world where moves are unreliable. The project looks at how the optimal policy changes as the agent's control over its movement drops.

The agent picks one of four directions. The move goes the way it meant with probability `p`, and otherwise slips to one of the two perpendicular directions with probability `(1 - p) / 2` each.

```
y=3   WALL    .     -1    +1
y=2    .      .      .     .
y=1    .     WALL    .    WALL
y=0    .      .      .     .
      x=0    x=1    x=2   x=3
```

| Parameter | Value |
|---|---|
| Step cost | -0.04 |
| Discount factor | 0.95 |
| Convergence threshold | 1e-4 |
| Terminal rewards | +1 at (3, 3), -1 at (2, 3) |

## What's in the notebook

[`value_iteration.ipynb`](value_iteration.ipynb):

1. Models the grid and its transitions (walls and the edge of the grid block movement).
2. Runs value iteration with Bellman updates until the values converge, then picks the best action in each state.
3. Solves the grid for `p = 0.7` and prints the utilities after every sweep. It converges in 23 sweeps.
4. Sweeps `p` from 0.1 to 0.9 and prints the optimal policy for each value.
5. Explains why the policy changes.

## Results

At `p = 0.7` the agent heads straight for the goal: it goes right along the bottom row, up the `x = 2` column, then right and up into the `+1`.

At low `p` the best action is often one the agent doesn't expect to make. Its chosen move is unlikely to happen, so it picks the action whose likely sideways slips land somewhere safe. For example, next to the `-1` it walks into a wall, because that move can't slip into the penalty. As `p` rises, these states switch one at a time to the direct move, with most of the changes between `p = 0.4` and `p = 0.7`.

## Running it

```sh
pip install -r requirements.txt
jupyter notebook value_iteration.ipynb
```

The notebook only uses the Python standard library. Jupyter is the only thing to install.
