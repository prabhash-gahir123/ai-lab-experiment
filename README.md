# ai-lab-experiment
ai lab experiment
Experiment 1 – Breadth-First Search (BFS)
Question: Write a Python program to perform BFS traversal of a graph.
Python Code:
from collections import deque
 
def bfs(graph, start):
    visited = {start}
    queue = deque([start])
    while queue:
        node = queue.popleft()
        print(node, end=" ")
        for neighbour in graph[node]:
            if neighbour not in visited:
                visited.add(neighbour)
                queue.append(neighbour)
 
graph = {'A': ['B', 'C'], 'B': ['D', 'E'], 'C': ['F'],
         'D': [], 'E': ['F'], 'F': []}
bfs(graph, 'A')


Experiment 2 – Depth-First Search (DFS)
Question: Write a Python program to perform DFS traversal of a graph.
Python Code:
def dfs(graph, node, visited=None):
    if visited is None:
        visited = set()
    if node not in visited:
        print(node, end=" ")
        visited.add(node)
        for neighbour in graph[node]:
            dfs(graph, neighbour, visited)
 
graph = {'A': ['B', 'C'], 'B': ['D', 'E'], 'C': ['F'],
         'D': [], 'E': ['F'], 'F': []}
dfs(graph, 'A')


Experiment 3 – Hill Climbing
Question: Write a Python program to demonstrate hill climbing.
Python Code:
def objective(x):
    return -(x - 5) ** 2 + 25
 
def hill_climb(start):
    current = start
    step = 0.1
    while True:
        candidates = [current, current - step, current + step]
        best = max(candidates, key=objective)
        if objective(best) <= objective(current):
            return current, objective(current)
        current = best
 
point, value = hill_climb(0)
print("Best point:", round(point, 2))
print("Objective value:", round(value, 2))


Experiment 4 – A* Algorithm
Question: Write a Python program to find a shortest path using A*.
Python Code:
import heapq
 
def astar(graph, heuristic, start, goal):
    queue = [(heuristic[start], 0, start, [start])]
    best = {start: 0}
    while queue:
        _, cost, node, path = heapq.heappop(queue)
        if node == goal:
            return path, cost
        if cost > best.get(node, float("inf")):
            continue
        for nxt, edge in graph[node].items():
            new_cost = cost + edge
            if new_cost < best.get(nxt, float("inf")):
                best[nxt] = new_cost
                heapq.heappush(queue, (new_cost + heuristic[nxt],
                                       new_cost, nxt, path + [nxt]))
    return None, float("inf")
 
graph = {'A': {'B': 1, 'C': 4}, 'B': {'C': 2, 'D': 5},
         'C': {'D': 1}, 'D': {}}
h = {'A': 4, 'B': 3, 'C': 1, 'D': 0}
print(astar(graph, h, 'A', 'D'))


Experiment 5 – Minimax Algorithm
Question: Write a Python program to implement the Minimax algorithm.
Python Code:
def minimax(tree, node, maximizing):
    if isinstance(tree[node], (int, float)):
        return tree[node]
    values = [minimax(tree, child, not maximizing) for child in tree[node]]
    return max(values) if maximizing else min(values)
 
tree = {'A': ['B', 'C'], 'B': ['D', 'E'], 'C': ['F', 'G'],
        'D': 3, 'E': 5, 'F': 2, 'G': 9}
print("Best value:", minimax(tree, 'A', True))


Experiment 6 – Alpha-Beta Pruning
Question: Write a Python program to implement Minimax with Alpha-Beta pruning.
Python Code:
def alphabeta(tree, node, maximizing, alpha, beta):
    if isinstance(tree[node], (int, float)):
        return tree[node]
    if maximizing:
        value = float("-inf")
        for child in tree[node]:
            value = max(value, alphabeta(tree, child, False, alpha, beta))
            alpha = max(alpha, value)
            if alpha >= beta:
                break
        return value
    value = float("inf")
    for child in tree[node]:
        value = min(value, alphabeta(tree, child, True, alpha, beta))
        beta = min(beta, value)
        if alpha >= beta:
            break
    return value
 
tree = {'A': ['B', 'C'], 'B': ['D', 'E'], 'C': ['F', 'G'],
        'D': 3, 'E': 5, 'F': 2, 'G': 9}
print(alphabeta(tree, 'A', True, float("-inf"), float("inf")))


Experiment 7 – 8-Queens Problem
Question: Write a Python program to solve the 8-Queens problem using backtracking.
Python Code:
N = 8
board = [-1] * N
 
def safe(row, col):
    for r in range(row):
        c = board[r]
        if c == col or abs(c - col) == abs(r - row):
            return False
    return True
 
def solve(row=0):
    if row == N:
        return True
    for col in range(N):
        if safe(row, col):
            board[row] = col
            if solve(row + 1):
                return True
            board[row] = -1
    return False
 
if solve():
    for r in range(N):
        print(" ".join("Q" if board[r] == c else "." for c in range(N)))


Experiment 8 – Water Jug Problem
Question: Write a Python program to solve the 4-litre and 3-litre water jug problem and measure 2 litres.
Python Code:
from collections import deque
 
def water_jug(cap_a=4, cap_b=3, target=2):
    start = (0, 0)
    queue = deque([(start, [start])])
    visited = {start}
    while queue:
        (a, b), path = queue.popleft()
        if a == target or b == target:
            return path
        ab = min(a, cap_b - b)
        ba = min(b, cap_a - a)
        states = [(cap_a, b), (a, cap_b), (0, b), (a, 0),
                  (a - ab, b + ab), (a + ba, b - ba)]
        for state in states:
            if state not in visited:
                visited.add(state)
                queue.append((state, path + [state]))
    return None
 
for state in water_jug():
    print(state)


Experiment 9 – Tic-Tac-Toe Game
Question: Write a Python program for a two-player Tic-Tac-Toe game.
Python Code:
def show(board):
    for i in range(0, 9, 3):
        print(" | ".join(board[i:i+3]))
        if i < 6:
            print("--+---+--")
 
def winner(board, mark):
    lines = [(0,1,2),(3,4,5),(6,7,8),(0,3,6),
             (1,4,7),(2,5,8),(0,4,8),(2,4,6)]
    return any(all(board[i] == mark for i in line) for line in lines)
 
board = list("123456789")
player = "X"
while True:
    show(board)
    move = input(f"Player {player}, choose cell 1-9: ")
    if not move.isdigit() or not 1 <= int(move) <= 9:
        print("Enter a number from 1 to 9.")
        continue
    i = int(move) - 1
    if board[i] in ("X", "O"):
        print("Cell occupied. Try again.")
        continue
    board[i] = player
    if winner(board, player):
        show(board)
        print("Player", player, "wins!")
        break
    if all(c in ("X", "O") for c in board):
        show(board)
        print("Draw!")
        break
    player = "O" if player == "X" else "X" 


Experiment 10 – Travelling Salesman Problem (TSP)
Question: Write a Python program to find the minimum tour using brute force.
Python Code:
from itertools import permutations
 
def tsp(distance):
    cities = list(distance)
    start = cities[0]
    best_route, best_cost = None, float("inf")
    for order in permutations(cities[1:]):
        route = (start,) + order + (start,)
        cost = sum(distance[route[i]][route[i+1]]
                   for i in range(len(route)-1))
        if cost < best_cost:
            best_route, best_cost = route, cost
    return best_route, best_cost
 
distance = {
    'A': {'A': 0, 'B': 10, 'C': 15, 'D': 20},
    'B': {'A': 10, 'B': 0, 'C': 35, 'D': 25},
    'C': {'A': 15, 'B': 35, 'C': 0, 'D': 30},
    'D': {'A': 20, 'B': 25, 'C': 30, 'D': 0}
}
route, cost = tsp(distance)
print("Shortest tour:", " -> ".join(route))
print("Minimum cost:", cost)
