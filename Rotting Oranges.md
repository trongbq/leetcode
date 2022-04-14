# Rotting Oranges

- Problem: https://leetcode.com/problems/rotting-oranges/
- Level: Medium

### Explanation

To check for fresh oranges when each minute passes, we can go through all cells that contains rotten oranges and check its adjacent cells to see that if those cells contain fresh oranges, then we mark them as rotten oranges. We stop until there is no new fresh orange found.

```go
// List of fresh cells that will be rotten
// We stores them in there and used that to modify `grid` later
// Modifying `grid` directly leads to invalid state
// We also can create a clone of grid but that's more expensive
var cells []cell
for i := 0; i < m; i++ {
    for j := 0; j < n; j++ {
        if grid[i][j] == 2 {
            for _, c := range adjs {
                if isFresh(grid, i + c.x, j + c.y, m, n) {
                    cells = append(cells, cell{i+c.x, j+c.y})
                }
            }
        }
    }
}
// Update `grid` with rotten oranges
for _, c := range cells {
    grid[c.x][c.y] = 2
}
```

The algorithm works fine, but we can optimize it. Every minute passes, we have to check adjacent cells all of current rotten oranges, even though that some rotten oranges won't have any new fresh oranges in its adjacent cells. We are only intested in new rotten oranges to expand our rotten area.

Instead of using two loops to scan entire cells, we can just use a list to keep track of new rotten oranges to check for adjacent fresh oranges.

### Code

```go
func orangesRotting(grid [][]int) int {
    m := len(grid)
    n := len(grid[0])
    
    type cell struct {
        x int
        y int
    }
    
    // List of rotten oranges that might have fresh orange nearby
    rotten := list.New()
    // Number of fresh oranges remaining
    fresh := 0

    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if grid[i][j] == 2 {
                rotten.PushBack(cell{i,j})
            } else if grid[i][j] == 1 {
                fresh++
            }
        }
    }
    
    // 4-directionally adjacent
    adjs := []cell{ {-1, 0}, {1, 0}, {0, -1}, {0, 1} }
    
    c := 0
    // Continue check for rotten oranges when we still have new discovered rotten oranges
    // and fresh oranges.
    for rotten.Len() > 0 && fresh > 0 {
        // Check for new fresh oranges adjacented to current list of rotten oranges
        rn := rotten.Len()
        for i := 0; i < rn; i++ {
            // Get one rotten orange out of queue
            cnode := rotten.Front()
            rotten.Remove(cnode)
            c := cell(cnode.Value.(cell))
            
            for _, adj := range adjs {
                if isFresh(grid, c.x + adj.x, c.y + adj.y, m, n) {
                    // Mark this orange as rotten and push it to the queue for next loop search
                    grid[c.x+adj.x][c.y+adj.y] = 2
                    fresh--
                    
                    rotten.PushBack(cell{c.x+adj.x, c.y+adj.y})
                }
            }
        }
        c++
    }
    
    if fresh > 0 {
        return -1
    }
    return c
}

func isFresh(grid [][]int, i, j, m, n int) bool {
    if i < 0 || i >= m {
        return false
    }
    if j < 0 || j >= n {
        return false
    }
    return grid[i][j] == 1
}
```

### Complexity

- Time: ?
- Space: O(m*n) due to the list or rotten oranges
