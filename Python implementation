class PuzzleNode:
   def __init__(self, n, state):
       self.n = n
       self.tablesize = n**2
       self.goal = []
       self.state = state

   def __str__(self, state):
   #Visualizes the table
       t = " | "
       s = " ----"
       print s * (self.n)
       for i in self.state:
           counter , u = 0, t
           while counter < len(i):
               u += str(i[counter]) + t
               counter += 1
           print u
           print s * (self.n)

def place_heuristic(state):
   #A heuristic function which is based on the number of tiles in their misplaced place.
   #It first checks the type of data which the input comes in(string or list of list), and then works with it accordingly.
   if type(state) == str:
       state = eval(state)
   elif type(state) == list:
       pass

   # We know an element is misplaced when the index of that element is not equal to that element
   flat_statelist = [item for sublist in state for item in sublist]
   misplacedcounter = 0
   for element in flat_statelist:
       if flat_statelist.index(element) != element:
           misplacedcounter += 1
   return misplacedcounter

def Manhattan_heuristic(state):
   #A heuristic function which is based on the manhattan distance of a tile from it's ideal position
   #Again, it first checks the type of data which the input comes in(string or list of list), and then works with it accordingly.
   if type(state) == str:
       state = eval(state)
   elif type(state) == list:
       pass
   flat_statelist = [item for sublist in state for item in sublist]
   flat_goallist = range(0, len(flat_statelist))
   mandistance = 0

   #We use it's modular arthmetic to get it's x and y coordinates in the grid. The manhattan distance is the sum of all these x
   #and y coordinates for each of the tiles
   for element in flat_statelist:
       distance = abs(flat_goallist.index(element) - flat_statelist.index(element))
       xcoord, ycoord = distance//len(state[0]), distance%len(state[0])
       mandistance += xcoord + ycoord
   return mandistance

def myheuristic(state):
   #This is my attempt at the linear conflict heuristic function. It is a type of heuristic that incorporates the manhattan distance and increases it based
   #on whether or not a condition is satisfied. It's better than the Manhattan heuristic in principle because it incorporates it in it's design, and thus can't be
   #worse than it.
   #Again, it first checks the type of data which the input comes in(string or list of list), and then works with it accordingly.
   if type(state) == str:
       state = eval(state)
   elif type(state) == list:
       pass
   flat_statelist = [item for sublist in state for item in sublist]
   flat_goallist = range(0, len(flat_statelist))
   mydistance = 0

   #It checks for two tiles whether or not they are in the same row in the goal state, and the present state, and
   # adds 2 to the manhattan distance if they are across each other as they would have been in the goal state
   for i in range(len(state[0])):
       for j in state[i]:
           for k in state[i]:
               if j and k in goalstate(state)[i] and (flat_goallist.index(j) - flat_statelist.index(j) > 0 and flat_goallist.index(k) - flat_statelist.index(k) < 0) or (flat_goallist.index(j) - flat_statelist.index(j) < 0 and flat_goallist.index(k) - flat_statelist.index(k) > 0):
                   mydistance += 2
   return mydistance/2 + Manhattan_heuristic(state)


#Creating list heuristics which is a pointer to both heuristics created
heuristics = [place_heuristic, Manhattan_heuristic, myheuristic]

def goalstate(state):
   flat_statelist = [item for sublist in state for item in sublist]
   flat_goallist = range(0, len(flat_statelist))
   goal = []
   glstcounter = 0
   for j in range(len(state[0])):
       goal.append(range(glstcounter, glstcounter + len(state[0])))
       glstcounter += len(state[0])
   return goal

def moves(inputs, n):
   #Move generator, takes in a state and the different possible next moves

   storage  =  []
   inputs = str(inputs)
   move = eval(inputs)

   i = 0
   while 0 not in move[i]: i += 1
   j = move[i].index(0);  # blank space (zero)

   # Sets boundary for the topmost cells. Allows for downward movement of adjacent cells if they aren't at the topmost edge.
   if i > 0:
       move[i][j], move[i - 1][j] = move[i - 1][j], move[i][j];
       storage.append(str(move))
       move[i][j], move[i - 1][j] = move[i - 1][j], move[i][j];

   # Sets boundary for the bottommost cells. Allows for upward movement of adjacent cells if they aren't at the bottommost edge.
   if i < n-1:
       move[i][j], move[i + 1][j] = move[i + 1][j], move[i][j]
       storage.append(str(move))
       move[i][j], move[i + 1][j] = move[i + 1][j], move[i][j]

   # Sets boundary for the rightmost cells. Allows for upward movement of adjacent cells if they aren't at the rightmost edge.
   if j > 0:
       move[i][j], move[i][j - 1] = move[i][j - 1], move[i][j]
       storage.append(str(move))
       move[i][j], move[i][j - 1] = move[i][j - 1], move[i][j]

   # Sets boundary for the leftmost cells. Allows for upward movement of adjacent cells if they aren't at the leftmost edge.
   if j < n-1:
       move[i][j], move[i][j + 1] = move[i][j + 1], move[i][j]
       storage.append(str(move))
       move[i][j], move[i][j + 1] = move[i][j + 1], move[i][j]

   return storage

def Astar(start, finish, heuristic):
   #The A star part of the algorithm
   n = len(start)
   start , finish = str(start), str(finish)
   pathstorage = [[heuristic(start), start]]  # optional: heuristic_1
   expanded = []
   expanded_nodes = 0
   while pathstorage:
       i = 0
       for j in range(1, len(pathstorage)):
           if pathstorage[i][0] > pathstorage[j][0]:
               i = j
       path = pathstorage[i]
       pathstorage = pathstorage[:i] + pathstorage[i + 1:]
       finishnode = path[-1]
       if finishnode == finish:
           break
       if finishnode in expanded: continue
       for b in moves(finishnode, n):
           if b in expanded: continue
           newpath = [path[0] + heuristic(b) - heuristic(finishnode)] + path[1:] + [b]
           pathstorage.append(newpath)
           expanded.append(finishnode)
       expanded_nodes += 1
   return expanded_nodes,  len(path), path


def solvePuzzle(n, state, heuristic, prnt):
   flat_statelist = [item for sublist in state for item in sublist]
   flat_goallist = range(0, len(flat_statelist))
   #Part that returns the error code -1
   #It checks if the length of the input is a perfect square, and then if all the leements int
   #  he goal state is in the start sstate.
   if len(flat_statelist) != n**2:
       steps, frontierSize, err = 0, 0, -1
   elif True in [i not in flat_statelist for i in range(0,n**2-1)]:
       steps, frontierSize, err = 0, 0, -1
   else:
       steps, frontierSize, solutions = Astar(state,goalstate(state), heuristic)
       err = 0

   #Prints the solutions if the prnt = True
   if prnt == True:
       for i in solutions[1:]:
           t = PuzzleNode(n, eval(i))
           t.__str__(i)
           print "The next step is :"


   return "The frontier size, number of steps and error code are :" , steps, frontierSize, err,

puzzle =  [[7,0,8],[4,6,1],[5,3,2]]
print solvePuzzle(3, puzzle, heuristics[2], True)
