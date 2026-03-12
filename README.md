## **AStarAlgorithm**
A report of the A* algorithm project created for a C++ module
Welcome to my C++ A* search algorithm project. My name is Nathan Ferry, and here I catalog the timeline of the cumulative work that led to my final code.

## **Core Content**

### **What is the A* Search Algorithm?**
Before I delve into the evolution of the code, I should explain what the project goal is. The A* search algorithm is an algorithm widely used to find the optimal path through obstacles. It has three main attributes, the g cost, h cost and f cost. In a co-ordinate system you may want to navigate in a straight line with no obstacles, or the farthest distance on your plane with many tiles you are unable to travel in. The algorithm uses the g cost or distance between the starting location and the current tile, the h cost or the distance from the current tile to the goal tile and the f cost which is the g and h costs combined. The optimal path is the lowest f cost as you are the shortest distance from the start or end at any given point. Given an example where you are on a tile and want to go straight three tiles without any obstacles the g cost would begin at 0 and the h cost 3 for an f cost of 3. The most optimal path will always be 3 as when you take a step forward, not only does g increase by 1, but h decreases by 1 leaving f to be a constant. These values are still estimates at the end of the day, as these costs to move are calculated using differing methods depending on whether you can move in just four directions, or also diagonal. [1] 

### **Code Evolution**
Upon staring the project, my goal was to create small but meaningful helper functions that would be useful in the final product. I knew I would have to generate a random grid for example, so I found no reason to program with a grid  that I created by hand (see Fig 1 as an example) if I was going to eventually rewrite it. This meant slower progress in the short term but less time wasted in the long run. The first week of development saw me create functions to generate a random grid (see Fig 2) and a means of checking if a given co-ordinate is blocked (see Fig 3) or is valid. The grid is generated using a 1 or 0 in each tile to signify a blocked or unblocked tile respectively. This grid is stored in a 2d vector that is a private member function of the StarAlgorithm class. The logic for the blocked and valid methods are more or less the same, both returning a boolean based on an if statement for the passed co-ordinate. Each co-ordinate was decided to be a pair that would be passed to functions since it was a container that made the most sense to easily store co-ordinates. The validity check is in case a co-ordinate is passed that is outside of the given grid size. This was used later for logic that would check the neighbours of a tile when moving. 

The final functions I created before tackling the main logic were a function to display the grid, a means of creating a start and end co-ordinate and calculating the distance between the current co-ordinate and the end. The grid display method was straightforward, only printing the grid by iterating over the 2d vector and using a switch case to print symbols instead of numbers as I believed the grid would look cleaner this way (see Fig 4). The random number generation for choosing a start and end co-ordinate were eerily similar to the code for generating a random grid. The only difference being that the co-ordinated were in the range of the rows in the grid -1 and columns -1 (given that the co-ordinate system started at 0,0 and hence an 8x8 grid wouldn't have a co-ordinate of 8,8 for example). Once the co-ordinates were generated, the start and end co-ordinate (which were pairs and private members of the class) were set to the genrated values. The 

Euclidean distance is calulated via the following formula: h = sqrt((current x - end goal x)^2 - (current y - end goal y)^2). I touched on distances in the ealier section on the algorithm itself, but to summarise, it is a means of estimating the distance to the end from the current tile. It is optimistic in nature as the calculation is for the displacement and doesn't take into account that an obstacle may divert the path. Using the Euclidean distance, the path taken can be in all directions i.e. not North South East and West but also North-West, North-East, South-East and South-West. A more simple means of calculating distance is Manahttan in which the algorithm is limited to four directions [2]. 

<sub> Fig 1 A hand inserted grid </sub>

<img width="478" height="174" alt="image" src="https://github.com/user-attachments/assets/a94f905f-c1de-46d3-aa9e-f8a9483220e9" />

<sub> Fig 2 Method to generate a random grid </sub>

<img width="369" height="103" alt="image" src="https://github.com/user-attachments/assets/a6f08f8e-d1f9-4cd9-bbe2-360752dbfd26" />

<sub> Fig 3 isblocked </sub>

<img width="332" height="216" alt="image" src="https://github.com/user-attachments/assets/10c4edc3-7ed3-4b03-b3e3-fdf086c3ab7d" />

<sub> Fig 4 grid displayed by symbols </sub>

<img width="755" height="82" alt="image" src="https://github.com/user-attachments/assets/035eb7ba-3b1f-4277-a0d9-29e98f65fe29" />

<sub> Fig 5 Euclidean distance </sub>

I referenced a video on the A* algorithm that was recommended by our lecturer to build the main algorithm. The code referenced was written in C# (see Fig 6) and therefore I converted it into C++ by understanding what each logical component did on the video, and what methods were required in order to achieve it that weren't shown. I began by breaking the reference into subtasks. Starting with the arguments passed, the start and end positions are passed. Two lists are then defined, toSearch known in A* generally as open and closed lists are called toSearch and processed. They hold the tiles or nodes that are chosen to search already (processed) or may be searched (in the toSearch list). A while loop encapsulates the entire algorithm which runs while the open list still has nodes to cycle through. The algorithm would return the path if successfull, but if not, it would need to let the user know that a path was not found. A current variable is set to the top of the list and a for loop checks for the most optimal tile to search next, comparing the total distances between the tiles and the distance to the end. The reason there are two checks within the if is the fact that DOUBLE CHECK!!!!!!!. If either case is true, the current tile is set to the more optimal one and added to the processed list since it has then been chosen to search. It is removed from toSearch and the neighbours of this new tile are selected. A new foreach loop is used to check all neighbours of the tile that are not obstructed, out of bounds or already processed. If a tile meets none of the critera above a check is perfomed to see if the tile is already in the toSearch list. The new g cost is calculated given the total distance from the start thus far and the added distance of moving to this new tile. Finally two if statements finish the logic. Initially, the boolean checking if the tile is already in the toSearch list is checked to be false or the G value is compared with the tiles current G. The g cost may need to be revaluated at some point. For example, the path is traced and due to an obstacle the g cost was not what is should have been initially. DOUBLE CHECK!!!! The distance from the start is set and the connection to the neighbour tile to the current tile is created. This connection is used to trace the path if a viable path is found at the end. The H cost is set here too, letting the tile know the distance it is from the end.

This was hard to wrap my head around logically at the start and I found it best to write in pseudocode (see Fig 7) with comments regarding the final structure I expected. There were a few changes I knew I would make to format the code for C++ but also work with my style and thought process. I found that it made more sense to have a struct for each node instead of individual pairs which were now accessed as a member of a struct. I edited my functions to accept what I called a node struct (see Fig 8). The struct had f,g and h as doubles initally. It also contained a pair for each co-ordinate but eventaully it grew which I will talk about in further detail when the relevant code appears. This was rather straight forward and only caused minor syntaxical errors. Once finished, I moved on to creating pairs instead of lists at the top of the tracepath function, I no longer passed arguments since the start and end position were members of the class and accessible via any function. The rest of the programming initially was finding the C++ equivalent of the methods used in the C# code.

<img width="627" height="559" alt="image" src="https://github.com/user-attachments/assets/3e3ed065-1ff3-45b8-ae65-4ffaea624933" />

<sub> Fig 6 screenshot of referenced video </sub>

<img width="937" height="567" alt="image" src="https://github.com/user-attachments/assets/f90f9414-7f6b-464d-ae2b-c6a7a07a8e28" />

<sub> Fig 7 C++ pseudocode </sub>


<sub> Fig 8 The final iteration of a node struct </sub>

The screenshot above aided me to finally push my understanding of the algorithm from hazy to a deep understanding. 

### **The Final Product**
Reference header for final code

Dive into the main algorithm in its final phase referencing other methods by proxy

### **Design Decisions**
Why a namespace

Why the struct

Why testing like this

Why a map

### **Potential Improvements**
How a heap could improve the code

How unit testing & Exceptions could be added

Truer randomness, explain the limitations of the current one now.

## **Project planning**
I used OneNote for project planning throughout the development of this project. Each day I worked on this project, I noted tasks that were completed. I made certain all sources (including the date they were accessed) were transcibed in a section as well as the relevant section of code they pertained to, or knowledge I gained from them. This was a useful means of lightening the workload for this report, since it allowed me to check what information I gained from where, as well as how the code was developed over time. This made citations a quick and easy task for this report thanks to my ealier forethought. Within the log I kept screenshots of my algorithm's evolution over time enabling the history of the project to be shown in the core content section. The log made it simpler to track the tasks yet to complete, with a subsection dedicated to features I wanted to add. The section meant that jotting down ideas was simple as well as easy to locate once the current feature in development was completed. Within programming labs, I kept my lecturer up to date on the current status on the project and voiced new ideas and optimisations in the log and backlog of tasks. I would ask questions pertaining to next steps if all my features were complete and my code was reviewed several times, leading to a polished final product with renewed guidence each week and goals to work towards by the next session. 

## **Reflective element**
Biggest problems would be the linking of the structs to trace the path
What I would do next time being the heap and unit testing and maybe making the grid a separate class

### **References**
[1] Tarodev, “Pathfinding - Understanding A* (A star),” YouTube. Nov. 16, 2021. [Online]. Available: https://www.youtube.com/watch?v=i0x5fj4PqP4  (accessed Feb. 10, 2026).
[2] GeeksforGeeks, “A* Search Algorithm,” GeeksforGeeks, Jun. 16, 2016. https://www.geeksforgeeks.org/dsa/a-search-algorithm/  (accessed Feb. 07, 2026).  


