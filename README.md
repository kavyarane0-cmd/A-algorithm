# 8-Puzzle Problem using A* Algorithm

This project implements the A* search algorithm to solve the 8-puzzle problem.

## Algorithm
A* uses the function:
f(n) = g(n) + h(n)

Heuristic used: Manhattan Distance.

## How to Run
python a_star_8_puzzle.py

## Tools
- Python
- Google Colab

```python
import heapq

goal_state=((1,2,3),
            (4,5,6),
            (7,8,0))

def manhattan(state):
  dist=0
  for i in range(3):
    for j in range(3):
      value=state[i][j]
      if value!=0:
        goal_x=(value-1)//3
        goal_y=(value-1)%3
        dist+=abs(i-goal_x)+abs(j-goal_y)
  return dist


def find_zero(state):
  for i in range(3):
    for j in range(3):
      if state[i][j]==0:
        return i,j

def get_neighbours(state):
  neighbours=[]
  x,y=find_zero(state)
  moves=[(1,0),(-1,0),(0,1),(0,-1)]

  for dx,dy in moves:
    nx,ny=x+dx,y+dy
    if 0<=nx<3 and 0<=ny<3:
      new_state=[list(row) for row in state]
      new_state[x][y],new_state[nx][ny]=new_state[nx][ny],new_state[x][y]
      neighbours.append(tuple(tuple(row)for row in new_state))

  return neighbours

def a_star(start):
  pq=[]
  heapq.heappush(pq,(0+manhattan(start),0,start,[]))
  visited=set()

  while pq:
    f,g,current,path=heapq.heappop(pq)
    if current==goal_state:
      return path+[current]
    if current in visited:
      continue
    visited.add(current)

    for neighbour in get_neighbours(current):
      if neighbour not in visited:
        heapq.heappush(pq,(g+1+manhattan(neighbour),g+1,neighbour,path+[current]))
  
start_state=((1,2,3),
             (0,4,6),
             (7,5,8))

solution=a_star(start_state)
if solution is None:
  print("No solution exists for this puzzle!")
else:
  print("Steps to solve:")
  for step in solution:
    for row in step:
      print(row)
    print("------")
