# actor-connection-finder
A shortest-path solver that finds connections between actors through movies using breadth-first search (BFS) on a graph of Hollywood data.

🎬 The Problem
Given two actors, find the shortest path of connections between them where each connection is a movie they both appeared in. This implements the "Six Degrees of Kevin Bacon" game.

How it works:

text
Kevin Bacon → "Mystic River" → Sean Penn → "Milk" → James Franco
🕸️ Graph Structure
The system builds a graph with:

Nodes - Actors and movies (two types)

Edges - Connect actors to movies they appeared in

text
Actor 1 ---- Movie ---- Actor 2
(Emma Watson)  (Harry Potter)  (Daniel Radcliffe)
🔍 Search Algorithm
Breadth-First Search (BFS)
Finds shortest path (minimum degrees of separation)

Explores neighbors level by level

Guarantees optimal solution

Why BFS over DFS?
BFS finds shortest path (DFS might not)

Unweighted graph → BFS is ideal

More efficient for separation degrees

🛠️ Implementation Details
Data Structures
python
class Node:
    def __init__(self, state, parent, action):
        self.state = state      # Actor/movie ID
        self.parent = parent    # Previous node
        self.action = action    # Movie name (for connections)
BFS Algorithm Steps
python
def shortest_path(source, target):
    # Initialize frontier with source actor
    # Track explored nodes
    # While frontier not empty:
    #   - Remove node from frontier
    #   - If node is target, follow path back
    #   - Add neighbors (movies and actors)
    #   - Mark as explored
Neighbor Exploration
python
def neighbors_for_person(person_id):
    # Returns (movie_id, actor_id) pairs
    # For movies person acted in
    # Then actors in those movies
📁 Files
degrees.py - Main program

util.py - Stack, Queue, Node classes

small/ - Small dataset (test files)

large/ - Large IMDb dataset

🚀 Usage
bash
# Run with command line arguments
python degrees.py large
python degrees.py small

# Program prompts for two actors
python degrees.py large
Loading data...
Data loaded.
Name: Kevin Bacon
Name: Emma Watson
3 degrees of separation.
1: Kevin Bacon and Emma Watson starred in "Harry Potter"
📊 Sample Output
text
Loading data...
Data loaded.
Name: Tom Hanks
Name: Emma Watson

3 degrees of separation:
1: Tom Hanks and Emma Watson starred in "The Polar Express"
2: Emma Watson and Chris Cooper starred in "The Polar Express"
text
Name: Brad Pitt
Name: Meryl Streep

2 degrees of separation:
1: Brad Pitt and Meryl Streep starred in "The Devil's Own"
🔧 Key Functions to Implement
load_data(directory)
Loads actors, movies from CSV files

Builds adjacency structures

shortest_path(source, target)
BFS search implementation

Returns list of (movie, actor) connections

person_id_for_name(name)
Maps actor names to IDs

neighbors_for_person(id)
Returns all connected (movie, actor) pairs

💡 BFS Implementation Template
python
def shortest_path(source, target):
    start = Node(state=source, parent=None, action=None)
    frontier = Queue()
    frontier.add(start)
    explored = set()
    
    while not frontier.empty():
        node = frontier.remove()
        
        if node.state == target:
            # Reconstruct path
            path = []
            while node.parent is not None:
                path.append((node.action, node.state))
                node = node.parent
            path.reverse()
            return path
        
        explored.add(node.state)
        
        for movie_id, actor_id in neighbors_for_person(node.state):
            if actor_id not in explored:
                child = Node(state=actor_id, parent=node, action=movie_id)
                frontier.add(child)
    
    return None
🎯 Optimization Tips
Use sets for explored - O(1) lookup

Store neighbors efficiently - Pre-process during loading

Avoid revisiting nodes - Check explored before adding

Use Queue from util.py - BFS FIFO behavior

⚠️ Common Pitfalls
Forgetting to track both movies and actors - Both are nodes

Infinite loops - Not marking explored correctly

Incorrect path reconstruction - Reversing order

Not handling unreachable actors - Return None

Slow neighbor lookups - Inefficient data structures

🧪 Test Cases
Source	Target	Expected Result
Kevin Bacon	James Franco	2 degrees
Emma Watson	Daniel Radcliffe	1 degree
Tom Hanks	Tom Cruise	Connection exists
Random Actor	Random Actor	Possibly None
📈 Complexity
Time - O(V + E) for BFS

Space - O(V) for frontier and explored

Large dataset: ~250MB memory, few seconds for query
