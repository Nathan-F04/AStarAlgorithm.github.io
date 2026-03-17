## **AStarAlgorithm**
A report of the A* algorithm project created for a C++ module
Welcome to my C++ A* search algorithm project. My name is Nathan Ferry, and here I catalog the timeline of the cumulative work that led to my final code.

## **Core Content**

### **What is the A* Search Algorithm?*
Before I delve into the evolution of the code, I should explain what the project goal is. The A* search algorithm is an algorithm widely used to find the optimal path through obstacles. It has three main attributes, the g cost, h cost and f cost. In a co-ordinate system you may want to navigate in a straight line with no obstacles, or the farthest distance on your plane with many tiles you are unable to travel in. The algorithm uses the g cost or distance between the starting location and the current tile, the h cost or the distance from the current tile to the goal tile and the f cost which is the g and h costs combined. The optimal path is the lowest f cost as you are the shortest distance from the start or end at any given point. Given an example where you are on a tile and want to go straight three tiles without any obstacles the g cost would begin at 0 and the h cost 3 for an f cost of 3. The most optimal path will always be 3 as when you take a step forward, not only does g increase by 1, but h decreases by 1 leaving f to be a constant. These values are still estimates at the end of the day, as these costs to move are calculated using differing methods depending on whether you can move in just four directions, or also diagonal. [1] 

### **Creating the Algorithm Logic**
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

<img width="627" height="559" alt="image" src="https://github.com/user-attachments/assets/3e3ed065-1ff3-45b8-ae65-4ffaea624933" />

<sub> Fig 6 screenshot of referenced video </sub>

This was hard to wrap my head around logically at the start and I found it best to write in pseudocode (see Fig 7) with comments regarding the final structure I expected. There were a few changes I knew I would make to format the code for C++ but also work with my style and thought process. I found that it made more sense to have a struct for each node instead of individual pairs which were now accessed as a member of a struct. I edited my functions to accept what I called a node struct (see Fig 8). The struct had f,g and h as doubles initally. It also contained a pair for each co-ordinate but eventaully it grew which I will talk about in further detail when the relevant code appears. This was rather straight forward and only caused minor syntaxical errors. Once finished, I moved on, creating pairs instead of lists at the top of the tracepath function. I no longer passed arguments since the start and end position were members of the class and accessible via any function within the StarAlgorithm class. The rest of the programming initially was finding the C++ equivalent of the methods used in the C# code.

<img width="937" height="567" alt="image" src="https://github.com/user-attachments/assets/f90f9414-7f6b-464d-ae2b-c6a7a07a8e28" />

<sub> Fig 7 C++ pseudocode </sub>

<img width="331" height="188" alt="image" src="https://github.com/user-attachments/assets/513cabd9-a794-407d-af21-4d412705bb80" />

<sub> Fig 8 The final iteration of a node struct </sub>

An example of this would be iterating through the vector openList to erase the node that would be added to the ClosedList (see Fig 9). In the referenced code it was just using a .remove but in C++ I needed to employ the find and erase methods in a more complex way than using a simple remove. There are many exmples of logic such as this thoughout. The issue with using the find method is that it uses the  "==" operator on my created struct. This lead to the creation of the section is which I overloaded the operator to allow the find method to be used. 

<img width="443" height="41" alt="image" src="https://github.com/user-attachments/assets/b70f7b4f-22e8-4d0a-886c-f24a02f5bcf0" />

<sub> Fig 9 Locating a specific node </sub>

The main logic was complete at this point, but as previously mentioned, there were some functions which were sought in the referenced code but not shown. The logic to find the neighbours of a tile required a means of locating the co-ordinates a neighbour had (see Fig 10). The find_neighbours method contrained two loops to iterate through the co-ordinates a node's neighbours would be with several checks. Since the loops would iterate through a 3x3 grid to find the neighbours co-ordinates, the tile itself would eventually be found and a continue was used to ignore it. Otherwise a new node struct was created and the co-ordinates were set and the g and h values initialised so that they wouldn't cause issues with the mathematical logic of the algorithm. NOTE HERE!!!!!! The co-ordinates are then checked for obstacles and validity. For example, if the co-ordinate (0,0) is checked for neighbours, the first co-ordinate generated via the loop would be (-1, 1) which is not a valid co-ordinate. If all criteria are met, the neighbour would be added to the neighbourList which would be returned to the main algorithm. 

<img width="1244" height="596" alt="image" src="https://github.com/user-attachments/assets/e17f05d5-2989-408d-bf14-ec5616ea84ec" />

<sub> Fig 10 Locating a tile's neighbours </sub>

Interestingly, once the above had been complete I realised a key issue. There wasn't a means of returning the path the algorithm found. I referenced the video used for the inital pseudocode and found a final section revealing the method for tracing a path in the algorithm, or rather where the methods to trace said path were called (see Fig 11). The code to loop through the path was not the issue, the final piece of the puzzle was a means of tracing the found path via a connection method. This was the most difficult method to wrap my head around. 

<img width="510" height="280" alt="image" src="https://github.com/user-attachments/assets/172a12ce-84e7-460c-849c-645faa278e33" />

<sub> Fig 11 The missing logic </sub>

Initially when pondering how to create a connection method, I thought I needed to create a struct within a struct. This would store the information about the previous tile but the struct would have definition issues. I realised I would only require the co-ordinates for the path but fell into a the wrong path as I initally thought that with only the co-ordinates stored, if the algorithm took a wrong turn and recorrected, the list of co-ordinates would be wrong. This was of course incorrect but I prompted Claude as a means of finishing this section of the codebase, which set in motion a chain of debugging that confused me even more. Initially, the ai concluded that pointers were required to link the nodes and an additional member of a pointer would be used to connect each node. I edited my functions to accept a pointer to a struct and accessed the members using "->" instead of "." (see Fig 12). Note that smart pointers weren't even used which would have been better practice. Once this had concluded, issues began to crop up. Due to erasing from the closed list, the pointers were shifting in memory and pointing to garbage. Other similar errors appeared with this method and inevitably, I asked the ai if there was a way to simplify the system. This was when the cameFrom map was used in which the keys were the co-ordinates of a node and the node connecting it (see Fig 13). This method was much simpler and allowed me to use the inital code without the complexity of pointers. 

<img width="576" height="282" alt="image" src="https://github.com/user-attachments/assets/2e05f2ee-322c-4aa3-ab18-1ac65499509c" />

<sub> Fig 12 An example of pointer logic complicating the code </sub>

<img width="510" height="23" alt="image" src="https://github.com/user-attachments/assets/a3a3eecb-4732-438b-bdbd-690c44fc5daf" />

<sub> Fig 13 The map which simplified my code </sub>

The tracePath function passed the path found (or empty vector if no path was found) to the findPath method (see Fig 14) which called the tracePath method and used an if statement to check that a returned list wasn't empty. If it was, the screen would display that no path was found. If the path was found, a for loop was used to set the co-ordinates to two so that the switch case in the for loop would display them using "*" in the displayGrid method. The method consists of nested for loops with a switch case. Each co-ordinate is printed based on numbers. The start co-ordinate is "SC" and the end goal or end co-ordinate is "EC". I edited this code to be the only section that would print ASCII to the screen using color (see Fig 15 & 16). 

<img width="593" height="288" alt="image" src="https://github.com/user-attachments/assets/399fe64a-0018-4a5c-bff9-b8d330fce744" />

<sub> Fig 14 findPath which displays the found path </sub>

<img width="498" height="511" alt="image" src="https://github.com/user-attachments/assets/96bf0a82-98c5-437f-beaf-79fca8495553" />

<sub> Fig 15 An example output of an unsolved grid </sub>

<img width="490" height="468" alt="image" src="https://github.com/user-attachments/assets/c6c02b9d-a133-47f4-9509-a7eeb2dafc3c" />

<sub> Fig 16 An example output of a solved grid </sub>

### ** Finding the Final Product**
From this point on, testing the algorithm & adhering to best practices was of the utmost importance. Minute edits to clean the code were made such as using "_" at the end of variables, making them easily identifiable. I received notes during a code review during the development and had a few tweaks to make. Most importantly, I needed to split the tracePath function due to its bloated nature (see Fig 17). At this point, the focus had been to try and get a working algorithm but I was reminded that a method should focus on one task alone. This lead to the evolution in creating a modular series of functions that are called within the shortened tracePath algorithm. 

<img width="393" height="598" alt="image" src="https://github.com/user-attachments/assets/a98f891a-a766-4be2-b732-3f1e4f38a410" />

<sub> Fig 17 The bloated algorithm is showcased just in the number of brackets at the bottom </sub>

The tracePath function shortened immensely, calling a series of new methods to outsource the logic (see Fig 18). getBestNode replaces the for loop to sort the openList to search for the best node to add to the closedList (see Fig 19). PromoteToClosedList simply removes the chosen node from the openList after adding it to the closedList. The check to tell if the destination is executed next, whereby if passed the new isDestination method is called (see Fig 20). This method iterates through the path as the logic did before, retracing the path, pushing the co-ordinates to the path vector with two slight tweaks. The cameFrom map is used to trace the next co-ordinate at the bottom of the while loop and reverse is used to flip the co-ordinate order for printing the start to destination rather than vice versa. If the destination is not the current co-ordinate, the processNeighbours method is called (see Fig 21). The function is used to iterate through the selected tile's neighbours, calling the find_neighbours method before checking if the neighbour is in the closedList already and finding if the node is in the openList. The openList check is skipped in the event that the node is already in the closedList. The findInOpenList method returns a boolean and totalG is calculated via the currenNode g value plus the distance to a tile in this case 1. A final check is present as before, but updateNeighbour is a new function which sets the neighbour g value equal to the totalG calculated and sets the co-ordinates of the tile that neighbour is set to, to be the key for the currentNode. This means that the co-ordinate of (1, 1) may have the neighbour of (1,2) and this logic is merely stating that the key of (1,2) has the node of (1,1) so that when the path is traced it can be ascertain the next co-ordinate and work backwards. If the node was not in the openList then the euclidean value for the h attribute and the f values are set for the node, otherwise to avoid pointing to garbage or the default, the node is set to have the same g and f values as the node it matched with ie (1,1) to (1,1). H is not copied as it never changes or updates, the euclidean distance is fixed.

<img width="501" height="218" alt="image" src="https://github.com/user-attachments/assets/69c94acd-2d75-4a70-87c2-fbf72fa829f6" />

<sub> Fig 18 The new algorithm method </sub>

<img width="1144" height="624" alt="image" src="https://github.com/user-attachments/assets/17daa6e7-5c84-4299-88b3-5a5a61b63ce6" />

<sub> Fig 19 The new algorithm methods </sub>

<img width="1363" height="188" alt="image" src="https://github.com/user-attachments/assets/83e2febe-328e-40af-9673-904f5f6f48f9" />

<sub> Fig 20 isDestination </sub>

<img width="1196" height="280" alt="image" src="https://github.com/user-attachments/assets/b6325c9c-cfa6-4cb1-8b5a-afbdb3d31ede" />

<sub> Fig 21 processNeighbours</sub>

<img width="582" height="533" alt="image" src="https://github.com/user-attachments/assets/06724193-b014-4296-996a-87e38f152073" />

Another note that required rectifying was the header file for the code. The header was hard to read due to the number of methods, variables and the struct (see Fig 22). The plan to fix this issue was to define the struct above the class definition for better readability. I employed a namespace to prevent collisions between similar struct or method names and to avoid making the struct global which would be bad practice (see Fig 23).

<sub> Fig 22 A busy class in the header </sub>

<img width="548" height="527" alt="image" src="https://github.com/user-attachments/assets/eec75fb9-8a3e-4759-b95a-f5756128277e" />

<sub> Fig 23 Cleaner header with namespace implementation </sub>

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


