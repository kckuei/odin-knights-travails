# Knights Travails
* Implementation of `knight_moves` function using a breath-first approach with `ruby`. This is a modified implementation of the example given/disscused [here](https://www.techiedelight.com/chess-knight-problem-find-shortest-path-source-destination/)).

### Assignment
Build a function `knight_moves` that shows the shortest possible way to get from one square to another by outputting all squares the knight will stop on along the way.

Think of the board as having 2-dimensional coordinates. The function would therefore look like:

```ruby
knight_moves([0,0],[1,2]) == [[0,0],[1,2]]
knight_moves([0,0],[3,3]) == [[0,0],[1,2],[3,3]]
knight_moves([3,3],[0,0]) == [[3,3],[1,2],[0,0]]
```

1. Put together a script that creates a game board and a knight.
2. Treat all possible moves the knight could make as children in a tree. Don’t allow any moves to go off the board.
3. Decide which search algorithm is best to use for this case.
4. Use the chosen search algorithm to find the shortest path between the starting square (or node) and the ending square. Output what that full path looks like, e.g.:
```ruby
  > knight_moves([3,3],[4,3])
  => You made it in 3 moves!  Here's your path:
    [3,3]
    [4,5]
    [2,4]
    [4,3]
```