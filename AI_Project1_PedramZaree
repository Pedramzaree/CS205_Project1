import heapq

# This class provide us an object which is a node in the search tree space
class Puzzle_node:
    def __init__(self, current_state, goal_state, N = 3, h_type = 'Misplaced Tile', parent = None, mov = None, parent_g_cost = -1):
        self.current_state = current_state
        self.goal_state = goal_state
        self.N = N
        self.mov = mov
        self.h_type = h_type
        self.g = parent_g_cost + 1
        self.parent = parent
        self.heuristic_func(self.h_type)
        self.cost = self.g + self.h

    # This function compares the current node with another node based on the cost function (f(n) = g(n) + h(n))
    def __lt__(self, other):
        return self.cost < other.cost

    # This function calculate the heuristic function of current node based on teh given method ('Misplaced Tile' or 'Manhattan' or 'zero')
    # For the 'zero' method which apply uniform cost search method, the output of function is encoded to zero.
    # For 2 first method, we should first find the position of hole and then calculate the heuristic distance to the goal state.
    def heuristic_func(self, type = 'Misplaced Tile'):
        counter = 0
        if (type == 'Misplaced Tile'):
            for i in range(self.N):
                for j in range(self.N):
                    if (self.current_state[i][j] != self.goal_state[i][j] and self.goal_state[i][j] != 0):
                        counter += 1
        elif (type == 'Manhattan'):
            for i in range(self.N):
                for j in range(self.N):
                    if (self.current_state[i][j] != self.goal_state[i][j] and self.goal_state[i][j] != 0):
                        item = self.goal_state[i][j]
                        for m in range(self.N):
                            for n in range(self.N):
                                if(self.current_state[m][n] == item):
                                    row = m
                                    column = n
                                    break
                        counter += (abs(i-m) + abs(j-n))
        elif (type == 'zero'):
            counter = 0       
        self.h =  counter


    # This function find the posible children for current node. First find the position of hole and then find possible children by moving hole to (Left, Right, Uo, Down).
    # Note that in some cases all of the directions aren't possible. For example, when the hole is in the last row, we don't have Down direction.              
    def create_child(self):
        children = []
        for m in range(self.N):
            for n in range(self.N):
                if(self.current_state[m][n] == 0): # The position of 0 element in the current state
                    row_0 = m
                    column_0 = n
                    break      

        if (column_0 != 0): # Left
            new_child = [row[:] for row in self.current_state]
            new_child[row_0][column_0-1], new_child[row_0][column_0] = new_child[row_0][column_0], new_child[row_0][column_0-1]
            children.append(Puzzle_node(current_state = new_child, goal_state = goal_state, N = self.N, h_type = self.h_type, parent=self, mov = (row_0, column_0, row_0,column_0-1), parent_g_cost = self.g))

        if (column_0 != (self.N-1)): # right
            new_child = [row[:] for row in self.current_state]
            new_child[row_0][column_0+1], new_child[row_0][column_0] = new_child[row_0][column_0], new_child[row_0][column_0+1]
            children.append(Puzzle_node(current_state = new_child, goal_state = goal_state, N = self.N, h_type = self.h_type, parent=self, mov = (row_0, column_0, row_0, column_0+1), parent_g_cost = self.g))

        if (row_0 != 0): # Up
            new_child = [row[:] for row in self.current_state]
            new_child[row_0-1][column_0], new_child[row_0][column_0] = new_child[row_0][column_0], new_child[row_0-1][column_0]
            children.append(Puzzle_node(current_state = new_child, goal_state = goal_state, N = self.N, h_type = self.h_type, parent=self, mov = (row_0, column_0, row_0-1, column_0), parent_g_cost = self.g))

        if (row_0 != (self.N-1)): # Down
            new_child = [row[:] for row in self.current_state]
            new_child[row_0+1][column_0], new_child[row_0][column_0] = new_child[row_0][column_0], new_child[row_0+1][column_0]
            children.append(Puzzle_node(current_state = new_child, goal_state = goal_state, N = self.N, h_type = self.h_type, parent=self, mov = (row_0, column_0, row_0+1, column_0), parent_g_cost = self.g))      
        return children
    




# This function is main part of algorithms, and it will explore the search space tree to find the solution
def general_search(initial_state, goal_state):
    nodes = []
    num_explored_nodes = 1
    heapq.heapify(nodes) # Convert the list to a heap queue which is desired for us
    heapq.heappush(nodes, initial_state) # Put the initial node in the queue
    #visited = set()
    visited = set()

    # while the queue is not empty
    while nodes: 
        current_state = heapq.heappop(nodes) # Pop the current node from queue to process
        visited.add(tuple(map(tuple, current_state.current_state)))

        if current_state.current_state == goal_state: # We have reached to the solution
            path = [] # find the path from initia node to solution
            while current_state.parent:
                path.append(current_state.mov)
                current_state = current_state.parent
            path.reverse()
            return path, num_explored_nodes

        for child in current_state.create_child():
            if tuple(map(tuple, child.current_state)) not in visited:
                heapq.heappush(nodes, child)
                num_explored_nodes += 1
                visited.add(tuple(map(tuple, child.current_state)))

    return None, num_explored_nodes

##**********************************++++++++++++++++++++++---------------------------------****************************************

# Main code (In this part I will get essential inputs from user):

N = int(input('Please Enter the length of board for puzzle problem. For example, enter 3 for 8-puzzle or enter 4 for 15-puzzle:   ')) # determine the size of puzzle (8-puzzle or ...)
hardcode = int(input('Enter 1 for pre-determined problem or enter 0 for your own problem:   ')) # user select to run the hardcode problom or his own problem

if (hardcode == 0):
    init_string = input('Enter your initial state for the problem. For example enter 1 2 3 4 5 6 7 0 8 for 8-puzzle problem. Note that enter 0 for hole element:   ') # Enter the initial state for the problem
    initial_state = []
    index = 0
    temp = init_string.split(' ')
    for i in range(N):
        l = []
        for j in range(N):
            l.append(int(temp[index]))
            index += 1
        initial_state.append(l)
else:
    initial_state = []
    element = 1
    for i in range(N):
        l = []
        for j in range(N):
            l.append(element)
            element += 1
        initial_state.append(l)   
    initial_state[N-2][N-2], initial_state[N-1][N-2], initial_state[N-1][N-1] = 0, initial_state[N-2][N-2], initial_state[N-1][N-2]


goal_state = [] 
element = 1
for i in range(N):
    l = []
    for j in range(N):
        l.append(element)
        element += 1
    goal_state.append(l)
goal_state[N-1][N-1] = 0


# determine the type of heuristic function
heuristic_type = input('Please enter the type of heuristic function: zero for uniform cost search or Misplaced Tile or Manhattan:   ')

# Create the initial state node
initial_state_node = Puzzle_node(current_state = initial_state, goal_state = goal_state, N = N, h_type = heuristic_type)
# Solve the problem to find path and number of explored nodes
solution, num_explored_nodes = general_search(initial_state_node, goal_state)

# Print the output
current_state = initial_state.copy() 
if solution != None:
    print(f'\nSolution found. We need {len(solution)} steps to reach the goal state. Also it explored {num_explored_nodes} nodes. Below graph shows the different step to reach the goal state:\n')
    for index in solution:
        next_step = [row[:] for row in current_state]
        next_step[index[2]][index[3]], next_step[index[0]][index[1]] = next_step[index[0]][index[1]], next_step[index[2]][index[3]]
        print(f'Current state = {current_state} -> Next state = {next_step} -> The hole is replaced with {current_state[index[2]][index[3]]}')
        current_state = next_step.copy()
else:
    print("\nNo solution for this problem.")









