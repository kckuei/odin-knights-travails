# Knights Travails
* Implementation of `knight_moves` function using a breath-first approach with `ruby`. This is a modified implementation of the example given/disscused [here](https://www.techiedelight.com/chess-knight-problem-find-shortest-path-source-destination/)). Another interesting solution [here](https://github.com/qpongratz/knights-travails/blob/main/knight_moves.rb). 

* Conceptually how this algorithm works:
  * The knight has 8 possible permutation movements that it can move relative to a starting point
  * Each position on the board can be represented by a node with row/column indices x, y, and a distance and path attribute
  * The distance and path attributes keep track of the number of steps, and previous board positions leading up to that point
  * Use a set to keep track of squares that have already been visited
  * Begin by populating the queue with the first node (starting point), and initializing the set
  * Until the queue is empty
    * Get the next node in the queue
    * Check if it is the node we're looking for, and return that node if it is
    * Otherwise if this is a previously unvisited node
      * Loop over all possible relative moves
      * For those moves that are inside the board, add a new node to the queue with updated distance and path
* Key issue with this impelmentation is needing to resort to deep copies for the path attributes (list objects), otherwise, mayhem happens.


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

### Implementation

```ruby
require 'set'

# Node class
class Node
  attr_accessor :x, :y, :dist, :path

  def initialize(x, y, dist = 0, path = [])
    @x = x
    @y = y
    @dist = dist
    @path = path
  end
end

# returns true if pos=[x,y] is inside board
def valid_pos(x, y, size)
  !(x.negative? || y.negative? || x >= size || y >= size)
end

def deep_copy(obj)
  Marshal.load(Marshal.dump(obj))
end

# returns shortest distance by knight from start to dest node using breadth-first-search traversal
def knight_moves(start, dest, size)
  # relative movements in horizontal/vertical possible by knight
  row = [2, 2, -2, -2, 1, 1, - 1, -1]
  col = [-1, 1, 1, -1, 2, -2, 2, -2]

  # initialize start and end nodes
  start_node = Node.new(start[0], start[1], dist = 0, path = [start])
  dest_node = Node.new(dest[0], dest[1])

  # set to check if the matrix cell is visited before or not
  visited = Set.new

  # create a queue and enqueue the first node
  queue = []
  queue.push(start_node)

  # loop till queue is empty
  until queue.empty?

    # dequeue front node and process it
    node = queue.shift

    x = node.x
    y = node.y
    dist = node.dist
    path = deep_copy(node.path)

    # if the destination is reached, return distance
    return node if x == dest_node.x && y == dest_node.y

    # skip if the location is visited before
    next if visited.include?(node)

    # mark the current node as visited
    visited.add(node)
    (0..row.length - 1).each do |i|
      x1 = x + row[i]
      y1 = y + col[i]
      next unless valid_pos(x1, y1, size)

      next_path = deep_copy(path) << [x1, y1]
      next_node = Node.new(x1, y1, dist + 1, next_path)
      queue.push(next_node)
    end
  end
end

def print_results(node)
  puts "You made it in #{node.dist} moves! Here's your path:"
  node.path.each { |loc| p loc }
end
```

### Example Input Output

```
size = 8
start = [0, 7]
dest = [7, 0]
final_node = knight_moves(start, dest, size)
print_results(final_node)

You made it in 6 moves! Here's your path:
[0, 7]
[2, 6]
[4, 5]
[6, 4]
[4, 3]
[6, 2]
[7, 0]
```