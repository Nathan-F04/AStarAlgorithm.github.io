## **AStarAlgorithm**
Welcome to my C++ A* search algorithm project. My name is Nathan Ferry, and here I catalog the timeline of the cumulative work that led to my final code along with the lessons learned along the way about best practices, how I optimised my code, why I designed it the way I did and how the overall project was a success in my opinion.

## **Core Content**

### **What is the A* Search Algorithm?*
Before delving into the evolution of the code, I should explain what the project goal is. The A* search algorithm is an algorithm widely used to find the optimal path through obstacles. It has three main attributes, the *g* cost, *h* cost and *f* cost. In a co-ordinate system, you may want to navigate in a straight line with no obstacles, or navigate the farthest distance on your plane with many tiles in which you are unable to travel through. The algorithm uses the *g* cost or distance between the starting location and the current tile, the *h* cost or the distance from the current tile to the goal tile and the *f* cost, the sum of *g* and *h* costs. The optimal path is the lowest *f* cost as you are the shortest distance from the start or end at any given point. Given an example where you are on a tile and want to go straight three tiles without any obstacles, the *g* cost would begin at 0 and the *h* cost 3 for an *f* cost of 3. The most optimal path will always be 3 as when you take a step forward, not only does *g* increase by 1, but *h* decreases by 1 leaving f to be a constant. *F* may not be a constant in cases where an obstacle is in the optimal path as the path will then divert. These values are still estimates at the end of the day, and the *h* cost is calculated using differing methods depending on whether you can move in just four directions, or also diagonal. [1] 

### **Creating the Algorithm Logic**
Upon starting the project, my goal was to create small but meaningful helper functions that would be useful in the final product. I knew I would have to generate a random grid for example, so I found no reason to program with a grid that I created by hand if I was going to eventually rewrite it (see Fig 1). This meant slower progress in the short term, but less time wasted in the long run. The first week of development saw me create functions to generate a random grid (see Fig 2) and a means of checking if a given co-ordinate is blocked using the *isBLocked* function (see Fig 3) or is valid using the *isValid* function. The grid is generated using a 1 or 0 in each tile to signify a blocked or unblocked tile respectively. This grid is stored in a 2D vector that is a private member function of the *StarAlgorithm* class. The logic for the blocked and valid methods are more or less the same, both returning a boolean based on an if statement for the passed co-ordinate. Each co-ordinate is stored as a pair that is passed to functions as a pair makes the most sense to easily store co-ordinates, in my opinion. The validity check is in case a co-ordinate is passed that is outside of the given grid size. This was added with logic that would check the neighbours of a tile when moving which I programmed later. 

The final functions I created before tackling the main logic were a function to display the grid, a means of creating a start and end co-ordinate, and a function for calculating the distance between the current co-ordinate and the end. The grid display method was straightforward, only printing the grid by iterating over the 2D vector and using a switch case to print symbols instead of numbers. I believe the grid looks cleaner this way (see Fig 4). The random number generation for choosing a start and end co-ordinate are eerily similar to the code for generating a random grid. The only difference being that the co-ordinates are in the range of the rows in the grid minus one and columns minus one. This is done as given that the co-ordinate system starts at (0,0) an 8x8 grid wouldn't have a co-ordinate of (8,8). Once the co-ordinates are generated, the start and end co-ordinate (which are pairs and private members of the class) are set to the generated values. 

The Euclidean distance is calculated via the following formula: 
*h* =  $\sqrt((current x - end goal x)^2 - (current y - end goal y)^2)$. [2]
I touched on distances in the earlier section on the algorithm itself, but to summarise, it is a means of estimating the distance to the end from the current tile. It is optimistic in nature as the calculation gets displacement and doesn't take into account that an obstacle may divert the path. Using the Euclidean distance (see Fig 5), the path taken can be in all directions i.e. not just North South East and West but also North-West, North-East, South-East and South-West. A more simple means of calculating distance is Manhattan in which the algorithm is limited to four directions [3]. 

<img width="426" height="68" alt="image" src="https://github.com/user-attachments/assets/6b8a99c1-7e03-4151-b846-31fb4fc3abaf" />

<sub> Fig 1 A hand inserted grid </sub>

<img width="478" height="174" alt="image" src="https://github.com/user-attachments/assets/a94f905f-c1de-46d3-aa9e-f8a9483220e9" />

<sub> Fig 2 A method to generate a random grid </sub>

<img width="369" height="103" alt="image" src="https://github.com/user-attachments/assets/a6f08f8e-d1f9-4cd9-bbe2-360752dbfd26" />

<sub> Fig 3 The *isblocked* function </sub>

<img width="332" height="216" alt="image" src="https://github.com/user-attachments/assets/10c4edc3-7ed3-4b03-b3e3-fdf086c3ab7d" />

<sub> Fig 4 A grid displayed by symbols </sub>

<img width="755" height="82" alt="image" src="https://github.com/user-attachments/assets/035eb7ba-3b1f-4277-a0d9-29e98f65fe29" />

<sub> Fig 5 Euclidean distance </sub>

THe main algorithm references a video recommended by my lecturer. The code is written in C# (see Fig 6) and I converted it into C++ by understanding what each logical component did on the video, and what methods were required in order to achieve it that weren't shown. I began by breaking the reference into subtasks. Starting with the arguments passed, the start and end positions are passed. Two lists are defined, known in A* generally as the *open* and *closed* lists in the video they are called *toSearch* and *processed*. They hold the tiles or nodes that were chosen to search (processed) or may be searched (the *toSearch* list). A while loop encapsulates the entire algorithm which runs while the *open* list still has nodes to cycle through. The algorithm returns the path if successful, but if not, it lets the user know that a path was not found. A current variable is set to the top of the list and a for loop checks for the most optimal tile to search next, comparing the *f* costs between the tiles and if the *f* cost is the same, the distance to the end. The reason there are two checks within the if is that both tiles may have the same *f* cost, but one may be closer to the end and have a lower *h* cost. The second half of the if allows you to always pick the tile that is closer to the end. If either case is true, the current tile is set to the more optimal one. If it was the last tile in the for loop and a more optimal one doesn't exist it will be added to the *processed* list. After being chosen to search, the tile is removed from *toSearch* and the neighbours of this new tile are selected. A new foreach loop is used to find all of the neighbours of the tile that are not obstructed, out of bounds or already processed. If a tile meets the criteria above, a check is performed to see if the tile is already in the *toSearch* list. The new *g* cost is calculated given the total distance from the start thus far and the added distance of moving to this new tile. 

Finally, two if statements finish the logic. Initially, the boolean checking if the tile is already in the *toSearch* list is checked or the *g* value is compared with the tiles current *g* cost. The *g* cost may need to be revaluated at some point. For example, if the path is traced and due to an obstacle the *g* cost was not what it should have been initially or if the path taken to the tile was less direct than it could have been, then the *g* cost is revaluated and lowered. The distance from the start is set to the new *g* cost in this case or set initially if the tile was not in the *toSearch* list, and the connection to the neighbour tile to the current tile is created if within the if statement. This connection is used to trace the path if a viable path is found at the end. The *h* cost is set in the second if statement as while a tile may need to be revaluated for its *g* cost, the *h* cost is only set if it isn't already in the *toSearch* list. This is done as the *h* cost is constant for a given tile. The tile is then added to the list.

<img width="627" height="559" alt="image" src="https://github.com/user-attachments/assets/3e3ed065-1ff3-45b8-ae65-4ffaea624933" />

<sub> Fig 6 A screenshot of the referenced video </sub>

The reference was hard to wrap my head around logically at the start and I found it best to write in pseudocode (see Fig 7) with comments regarding the final structure I expected. This made a sort of checklist of logical blocks I needed to have in place for the algorithm to be finished. There were a few changes I knew I would make to format the code for C++ but also to work with my style and thought process. It made more sense to have a struct for each node instead of individual pairs which were now accessed as members of the struct. I edited my functions to accept what I called a *node* struct (see Fig 8). The struct has *f*, *g* and *h* (as doubles initially but now they are floats as they require less memory). The struct contained a pair for each co-ordinate along with the costs, but eventually it grew, which I will talk about in further detail when the relevant code appears. Once finished, I moved on, creating pairs instead of lists at the top of the *tracePath* function. I no longer passed the start and end nodes as arguments since the start and end position were members of the class and accessible via any function within the *StarAlgorithm* class. The rest of the programming initially focused on finding the C++ equivalent of the methods used in the C# code. My classmate Shine Sujith [4] pointed out that the *find* method existed in C++ for example.

<img width="937" height="567" alt="image" src="https://github.com/user-attachments/assets/f90f9414-7f6b-464d-ae2b-c6a7a07a8e28" />

<sub> Fig 7 C++ pseudocode </sub>

<img width="331" height="188" alt="image" src="https://github.com/user-attachments/assets/513cabd9-a794-407d-af21-4d412705bb80" />

<sub> Fig 8 The final iteration of a node struct </sub>

An example of C# logic I converted to C++ would be iterating through the vector *openList* to erase the node that would be added to the *ClosedList* (see Fig 9). In the referenced code it was just using .remove but in C++ I needed to employ the *find* and *erase* methods in a more complex way than using a simple remove. There are many examples of logic such as this throughout my code. The issue with using the *find* method is that it uses the  *==* operator on my created struct. This led to the creation of the section in which I overloaded the operator to allow the *find* method to be used without issue. It was good experience overloading operators on my own struct and taught me a realistic example of overloading an operator on a project. 

<img width="443" height="41" alt="image" src="https://github.com/user-attachments/assets/b70f7b4f-22e8-4d0a-886c-f24a02f5bcf0" />

<sub> Fig 9 Locating a specific node </sub>

The main logic was complete at this point, but as previously mentioned, there were some functions which were sought in the referenced code but not shown. The logic remains relatively unchanged from my first iterations of the unshown functions. The method to find the neighbours of a tile requires a means of locating the co-ordinates a neighbour has (see Fig 10). The *find_neighbours* method contains two loops to iterate through the co-ordinates of a node's neighbours with several checks. Since the loops iterate through a 3x3 grid to find the neighbour's co-ordinates, the tile itself will eventually be found and a continue is used to ignore it. Otherwise, a new *node* struct is created and the co-ordinates set with the *g* and *h* values initialised. The co-ordinates are then checked for obstacles and validity. For example, if the co-ordinate (0,0) is checked for neighbours, the first co-ordinate generated via the loop would be (-1, 1) which is not a valid co-ordinate. If all criteria are met, the neighbour is added to the *neighbourList* which is returned to the main algorithm. 

<img width="835" height="486" alt="image" src="https://github.com/user-attachments/assets/25d86eba-a1ae-4535-953d-b0360d9bd666" />


<sub> Fig 10 Locating a tile's neighbours </sub>

Interestingly, once the above methods had been completed, I realised a key issue. There wasn't a means of returning the path the algorithm found. I referenced the video used for the initial pseudocode and found a final section revealing the method for tracing a path in the algorithm, or rather where the methods to trace said path were called (see Fig 11). The code to loop through the path was not the issue, the final piece of the puzzle was a means of tracing the found path via a connection method. This was the most difficult method to understand as I had to think in terms of multiple iterations of neighbour and tile navigation. I checked the github repository to see how it was done but it seemed very confusing only setting the connection via another variable that I could not find in the repository. [5]

<img width="510" height="280" alt="image" src="https://github.com/user-attachments/assets/172a12ce-84e7-460c-849c-645faa278e33" />

<sub> Fig 11 The missing logic </sub>

Initially when pondering how to create a connection method, I thought I needed to create a struct within a struct. This would store the information about the previous tile, but I knew the struct would have definition issues. I realised I would only require the co-ordinates for the path but did not pursue this, as initially I thought with only the co-ordinates stored, if the algorithm took a wrong turn and rectified, the list of co-ordinates would be the full path rather than the optimised one. This was of course incorrect, but I prompted Claude [6] as a means of finishing this section of the codebase, which set in motion a chain of debugging that confused me even more. Initially, the artificial intelligence (AI) concluded that pointers were required to link the nodes and an additional member of the class would be a pointer used to connect each node. I edited my functions to accept a pointer to a struct and accessed the members using "->" instead of "." (see Fig 12). Note that smart pointers weren't even suggested which would have been better practice. Once this had concluded, issues began to crop up. Due to erasing from the closed list, the pointers were shifting in memory and pointing to garbage. Other similar errors appeared with this method and inevitably, I asked the AI if there was a way to simplify the system. This was when the *cameFrom* map was used in which the keys were the co-ordinates of a *node* and the *node* connecting it (see Fig 13). This method was much simpler and allowed me to use the initial code without the complexity of pointers. I later optimised this further as I realised the rest of the *node* was not being used, the only storage required was the pair. I edited the map to hold two pairs as the key value pair.

<img width="576" height="282" alt="image" src="https://github.com/user-attachments/assets/2e05f2ee-322c-4aa3-ab18-1ac65499509c" />

<sub> Fig 12 An example of pointer logic complicating the code </sub>

<img width="510" height="23" alt="image" src="https://github.com/user-attachments/assets/a3a3eecb-4732-438b-bdbd-690c44fc5daf" />

<sub> Fig 13 The map which simplified my code </sub>

The *tracePath* function passed the path found (or empty vector if no path was found) to the *findPath* method (see Fig 14) which called the *tracePath* method and used an if statement to check that a returned list wasn't empty. If it was, the screen would display that no path was found. If the path was found, a for loop was used to set the co-ordinates to the integer two so that the switch case in the for loop would display them using "*" in the *displayGrid* method. The method consists of nested for loops with a switch case. Each co-ordinate is printed based on integers. The start co-ordinate is "SC" and the end goal or end co-ordinate is "EC". I edited this code to be the only section that would print ASCII to the screen using color (see Fig 15 & 16). 

<img width="593" height="288" alt="image" src="https://github.com/user-attachments/assets/399fe64a-0018-4a5c-bff9-b8d330fce744" />

<sub> Fig 14 *findPath* which displays the path found </sub>

<img width="498" height="511" alt="image" src="https://github.com/user-attachments/assets/96bf0a82-98c5-437f-beaf-79fca8495553" />

<sub> Fig 15 An example output of an unsolved grid </sub>

<img width="490" height="468" alt="image" src="https://github.com/user-attachments/assets/c6c02b9d-a133-47f4-9509-a7eeb2dafc3c" />

<sub> Fig 16 An example output of a solved grid </sub>

### **Finding the Final Product**
From this point on, testing the algorithm & adhering to best practices was of the utmost importance. Minute edits to clean the code were made such as using "_" at the end of variables, making them easily identifiable. I received notes during a code review during the development and had a few tweaks to make. Most importantly, I needed to split the *tracePath* function due to its bloated nature (see Fig 17). At this point, the focus had been to try and get a working algorithm but I was reminded that a method should focus on one task alone. This led to the evolution in creating a modular series of functions that are called within the shortened *tracePath* algorithm. 

<img width="393" height="598" alt="image" src="https://github.com/user-attachments/assets/a98f891a-a766-4be2-b732-3f1e4f38a410" />

<sub> Fig 17 The bloated algorithm is showcased just in the number of brackets at the bottom </sub>

The *tracePath* function shortened immensely, calling a series of new methods to outsource the logic (see Fig 18). *getBestNode* replaces the for loop to sort *openList* to search for the best *node* to add to *closedList* (see Fig 19). *PromoteToClosedList* simply removes the chosen *node* from *openList* after adding it to *closedList*. The check to tell if the destination is found is executed next, whereby if passed, the *isDestination* method is called (see Fig 20). This method iterates through the path as the logic did before, retracing the path, pushing the co-ordinates to the *path* vector with two slight tweaks. The *cameFrom* map is used to trace the next co-ordinate at the bottom of the while loop and *reverse* is used to flip the co-ordinate order for printing the start to destination rather than vice versa. If the destination is not the current co-ordinate, the *processNeighbours* method is called and *isDestination* is skipped for now (see Fig 21). The function is used to iterate through the selected tile's neighbours, calling the *find_neighbours* method before checking if the neighbour is in *closedList* already and finding if the node is in *openList*. The open list check is skipped in the event that the *node* is already in *closedList*. The *findInOpenList* method returns a boolean and *totalG* is calculated via the *currentNode* *g* value plus the distance to a tile (in this case 1). A final check is present as before, but *updateNeighbour* is a new function which sets the *neighbour g* value equal to the *totalG* calculated, and sets the co-ordinates of the tile that the neighbour is set to, to be the key for *currentNode*. This means that the co-ordinate of (1, 1) may have the neighbour of (1,2) and this logic is merely stating that the key of (1,2) has the co-ordinate of (1,1) so that when the path is traced it can ascertain the next co-ordinate and work backwards. The *updateNeighbour* method also checks if the *node* is not in *openList*. If it isn't, the euclidean value for the *h* and the *f* values are set for the *node*, otherwise to avoid pointing to garbage or the default, the *node* is set to have the same *g* and *f* values as the *node* it matched with i.e. (1,1) to (1,1). *H* is not copied as it never changes or updates, since the euclidean distance is fixed. The reason for the second if statement checking if the *node* is in *openList* within the *updateNeighbour* function is due to the fact that *updateNeighbour* could be called since the *g* value needed to be updated similar to the original code logic i.e. the if statement is passed not because of the *inSearch* boolean being false but because the *totalG* is less than the neighbour's *g* cost (see Fig 21).

<img width="501" height="218" alt="image" src="https://github.com/user-attachments/assets/69c94acd-2d75-4a70-87c2-fbf72fa829f6" />

<sub> Fig 18 The new algorithm method </sub>

<img width="1144" height="624" alt="image" src="https://github.com/user-attachments/assets/17daa6e7-5c84-4299-88b3-5a5a61b63ce6" />

<sub> Fig 19 The new algorithm methods </sub>

<img width="1363" height="188" alt="image" src="https://github.com/user-attachments/assets/83e2febe-328e-40af-9673-904f5f6f48f9" />

<sub> Fig 20 isDestination </sub>

<img width="1196" height="280" alt="image" src="https://github.com/user-attachments/assets/b6325c9c-cfa6-4cb1-8b5a-afbdb3d31ede" />

<sub> Fig 21 processNeighbours</sub>

Another issue I found was the random generation being seeded and therefore the same grid and start and end points were selected for a specific grid size. This meant that there wasn't a true random (although no such thing exists). With the addition of time(0) as an argument to the random default engine, the grid now has different blocked and unblocked tiles for the same grid size, and different start and end co-ordinates. This seeded randomness leads to different outputs for the same grid size, similar to how in C you use the current time or the noise on a pcb to seed randomness.

Another note that required rectifying was the header file for the code. The header was hard to read due to the number of methods, variables and the struct (see Fig 22). The plan to fix this issue was to define the struct above the class definition for better readability. I employed a namespace to prevent collisions between similar struct or method names and to avoid making the struct global which would be bad practice (see Fig 23). There was also a point in which many unused members were cut from the header and .cpp file for example a variable *h*, the node length and the diagonal node distance. The members were also changed to include "_" as previously mentioned and the new functions to debloat the algorithm function were included.*Rows_* and *Cols_* were also moved from public to private as most variables should be private to avoid outside access. I want my grid to be variable but only by me and wouldn't want a user to edit them from main. 

I also wanted all functions to be in camel case and altered the name of *find_neighbours* to *findNeighbours* to keep a consistent format.

<img width="582" height="533" alt="image" src="https://github.com/user-attachments/assets/06724193-b014-4296-996a-87e38f152073" />

<sub> Fig 22 A busy class in the header </sub>

<img width="845" height="522" alt="image" src="https://github.com/user-attachments/assets/3e0d55c2-5fd2-4e95-9f42-45d8e67bc0cd" />


<sub> Fig 23 Cleaner header with namespace implementation </sub>

### **Testing the Algorithm**
To close out the project, testing was added in the form of a new .h and .cpp file (see Fig 24, 25 & 26 respectfully). Generally, common or best practice would be to implement unit testing to confirm the logic of each function and their response to invalid arguments or data. In essence you want to check that two things are happening. 1) The function works as intended and 2) The function fails gracefully as intended. The header consists of the function signature for *RunTests* and the library to access the *StarAlgorithm* class. The test source file consists of the function body and the libraries for the algorithm and the test header.*RunTests* creates an object from the *StarAlgorithm* class and then calls three functions from the class. The first is *fillGrid* which generates the grid and the start and end co-ordinates.*displayGrid* is called to display the initial grid with the start and end co-ordinate before the algorithm finds a path. Finally, the *findPath* function is the most catalogued one here, where the algorithm is used before the path is traced and the new grid displayed with the path chosen (given there is a valid path). The main file consists of a try catch which runs the test function. If there is an issue the catch should print the error and return -1, otherwise 0 is returned if the tests run successfully.  

<img width="355" height="203" alt="image" src="https://github.com/user-attachments/assets/87c7265d-f1c1-41d0-b20a-894843f3d5ec" />

<sub> Fig 24 The header file for testing the system </sub>

<img width="366" height="242" alt="image" src="https://github.com/user-attachments/assets/65a09c31-a6bf-4f04-9eb2-91d155432890" />

<sub> Fig 25 The source file for testing the system</sub>

<img width="480" height="295" alt="image" src="https://github.com/user-attachments/assets/1c18af3d-7fd7-4e1f-a4d9-bb61e04b2ae0" />

<sub> Fig 25 The main file</sub>

### **Design Decisions**
There are certain design decisions that were used that are worth clarifying. One such decision is the struct being in the format it is in. I feel it is not only necessary to store the relevant attributes for the algorithm such as the *g*, *h* and *f* values per tile but it also makes the most sense to me personally. With the struct formatted this way, checks comparing the most accurate tiles are easier to make. Due to the nature of comparing nodes, the overloading of an operator is also implemented. There are reasons why you would avoid using the namespace implemented in the header file but as previously mentioned, it was decided that the definition of the node struct outside of the class is better for readability. The namespace avoids making the struct global while improving the readability by defining the struct outside of the class.

Testing in this project is also limited in implementation. Due to the nature of the time allocated for this project, the scope is not to facilitate full unit testing, but rather a system test where the overall algorithm is tested in place of individual functions. This is done due to the fact that the Visual Studio unit testing framework has a steep learning curve that would cost precious time over the course of the project.  

### **Potential Improvements**
There are a number of aspects of the code in its current state that are not optimal. The most inefficient part of the code at this point is the section that calculates the best current node at the beginning of the algorithm. This is O(n) as it is a linear search. With a heap, the code could be whittled down to O(log(n)) which is an improvement that would compound with the growth of the dataset, in this case *openList*. The heap is sorted and this is how it is more efficient than the linear search in the code as it stands. There are some caviats as pushing to the back of a vector is more efficient than adding data to the heap and hence it only makes sense to implement a heap given a large dataset [7].

Another major overhaul that I would implement if I continued on this project would be using the Visual Studio unit testing framework. I would implement full unit testing for the member functions of the class in terms of graceful fails and successful runs. I would also implement exceptions for said tests in order to fail gracefully.

A final improvement would be to uncouple the grid generation from the algorithm class. This would involve using the namespace set up in both classes. The namespace would be in a separate class for the functions using the grid, and I would remove the functions to generate the grid and start and end co-ordinates from the original class. 

## **Project planning**
I used OneNote for project planning throughout the development of this project. Each day I worked on this project, I noted tasks that were completed. I made certain all sources (including the date they were accessed) were transcribed in a section as well as the relevant section of code they pertained to, or knowledge I gained from them. This was a useful means of lightening the workload for this report, since it allowed me to check what information I gained from where, as well as how the code was developed over time. This made citations a quick and easy task for this report thanks to my earlier forethought. 

Within the log I kept screenshots of my algorithm's evolution over time enabling the history of the project to be shown in the core content section. The log made it simpler to track the tasks yet to complete, with a subsection dedicated to features I wanted to add. The section meant that jotting down ideas was simple as well as easy to locate once the current feature in development was completed. Within programming labs, I kept my lecturer up to date on the current status of the project and voiced new ideas and optimisations in the log and backlog of tasks. I would ask questions pertaining to next steps if all my features were complete and my code was reviewed several times, leading to a polished final product with renewed guidance each week and goals to work towards by the next session. 

The project evolved over time in incremental steps leading to the final product today. Early forethought meant that the grid generation didn't have to be edited much to get to the seeded final product and *isBlocked* and *isValid* were useful functions to create at the start. Using the reference video as a checklist made a lot of sense as it gave me the overall logic and a list of methods to create that I could focus on. I listened to notes from my code reviews, making *tracePath* a smaller overall function, leading to the creation of smaller helper methods. I cleaned my header file too, including the namespace and then focused on naming conventions such as adding "_" at the end of variables and using camel case for variable names. I moved the *ROWS_* and *COLS_* variables to private members for security and finalised the project with testing and code cleanup. I removed unnecessary comments, focusing on key function descriptions and focused on creating a system test with a new source and header file.

## **Reflective element**
The project has taught me best practices and style for C++. It also taught me the basics of the A* algorithm from the ground up. I reformatted code to incrementally optimise the code, making it much more readable, removing comments and unused code and following the philosophy of one function performing one action. Examples of improving readability would be including the namespace and adding "_" to variables, enforcing camel case for all functions, using the namespace and adding comments to explain what each function does at a high level. Unused functions were trimmed and improved where it was possible. The differences between C and C++ were readily apparent by the end of this project, including using *const expressions* to complete arithmetic at compile time, object oriented programming principles, c++ functions and containers such as *vectors*. The *map* used to trace the path of the algorithm initially had the key pair of a *pair* and the *struct* it came from. This was unnecessary as only the co-ordinates were needed to trace the path. Changing the map to include two *pairs* saved storing whole *nodes*. Of course, it goes without saying that I would focus on the number of improvements if given the chance such as unit testing, adding a heap and decoupling the algorithm class from the grid functions to make two classes. 

### **References**
[1] Tarodev, “Pathfinding - Understanding A* (A star),” YouTube. Nov. 16, 2021. [Online]. Available: https://www.youtube.com/watch?v=i0x5fj4PqP4  (accessed Feb. 10, 2026).

[2]“Writing mathematical expressions,” GitHub Docs. https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions (accessed Mar. 20, 2026).‌

[3] GeeksforGeeks, “A* Search Algorithm,” GeeksforGeeks, Jun. 16, 2016. https://www.geeksforgeeks.org/dsa/a-search-algorithm/  (accessed Feb. 07, 2026). 

[4] Shine Sujith, spoken conversation. (accessed Feb. 10, 2026). 

[5] Matthew-J-Spencer, “GitHub - Matthew-J-Spencer/Pathfinding: Related to this video: https://www.youtube.com/watch?v=i0x5fj4PqP4,” GitHub, 2025. https://github.com/Matthew-J-Spencer/Pathfinding (accessed Mar. 2, 2026).

[6] Anthropic, “Claude,” claude.ai, 2025. https://claude.ai/new (accessed Feb. 27,2026).

[7] GeeksforGeeks, “Heap in C++ STL,” GeeksforGeeks, Jul. 21, 2016. https://www.geeksforgeeks.org/cpp/cpp-stl-heap/ (accessed Mar. 21, 2026).
‌
‌

### ***Sources of information for code*
To avoid overciting and control the flow of the report above, the sources of functions and other minute details relating to code creation are shown below. An example of which would be the documentation on the sqrt() function. 

* F. Deak, “Initializing a two-dimensional std::vector,” Stack Overflow, Jul. 15, 2013. https://stackoverflow.com/questions/17663186/initializing-a-two-dimensional-stdvector (accessed Feb. 05, 2026). 

* user3413646, “Storing (x, y) Coordinates with set,” Stack Overflow, Feb. 02, 2019. https://stackoverflow.com/questions/54494503/storing-x-y-coordinates-with-set  (accessed Feb. 05, 2026). 

* GeeksforGeeks, “2D Vector in C++,” GeeksforGeeks, Oct. 09, 2017. https://www.geeksforgeeks.org/cpp/2d-vector-in-cpp-with-user-defined-size/ (accessed Feb. 05, 2026). 

* “std::sqrt, std::sqrtf, std::sqrtl - cppreference.com,” Cppreference.com, 2023. https://en.cppreference.com/w/cpp/numeric/math/sqrt.html (accessed Feb. 07, 2026). 

* “std::pow, std::powf, std::powl - cppreference.com,” Cppreference.com, 2024. https://en.cppreference.com/w/cpp/numeric/math/pow.html (accessed Feb. 07, 2026). 

* “std::abs(float), std::fabs, std::fabsf, std::fabsl - cppreference.com,” Cppreference.com, 2025. https://en.cppreference.com/w/cpp/numeric/math/fabs.html (accessed Feb. 07, 2026). 

* “Struct declaration - cppreference.com,” Cppreference.com, 2024. https://en.cppreference.com/w/c/language/struct.html  (accessed Feb. 10, 2026). 

* Cplusplus.com, 2025. https://cplusplus.com/reference/algorithm/count/ ‌(accessed Feb. 10, 2026). 

* “C++ Break and Continue,” www.w3schools.com https://www.w3schools.com/cpp/cpp_break.asp  (accessed Feb. 10, 2026). 

* Faken, “how do I initialize a float to its max/min value?,” Stack Overflow, Apr. 21, 2010. https://stackoverflow.com/questions/2684603/how-do-i-initialize-a-float-to-its-max-min-value  (accessed Feb. 10, 2026). 

* “How to compare Class struct - C++ Forum,” Cplusplus.com, 2026. https://cplusplus.com/forum/beginner/130111/ (accessed Feb. 17, 2026). 

* “W3Schools.com,” W3schools.com, 2026. https://www.w3schools.com/cpp/ref_vector_empty.asp (accessed Feb. 25, 2026). 

* “The entire table of ANSI color codes.,” Gist. https://gist.github.com/JBlond/2fea43a3049b38287e5e9cefc87b2124  (accessed Feb. 25, 2026). 

* “std::map - cppreference.com,” Cppreference.com, 2025. https://en.cppreference.com/w/cpp/container/map.html  (accessed Mar 11, 2026). 
