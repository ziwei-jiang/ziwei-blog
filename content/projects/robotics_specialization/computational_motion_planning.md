---
title: "Robotics Specialization: Computational Motion Planning"
date: 2016-03-15T11:30:03+00:00
weight: 5
# aliases: ["/first"]
tags: ["Robotics"]
author: "Me"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: true
draft: true
hidemeta: false
comments: true
description: "Motion and path planning for robots. "
# canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: true
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: false
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: false
math: true
cover:
    # image: "<image path/url>" # image path/url
    image: "/projects/robotics_specialization/imgs/course2/motion_planning.png"
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: true # when using page bundles set this to true
    hidden: false # only hide on current single page
    hiddenInList: false
    hiddenInSingle: false
editPost:
    URL: "https://github.com/<path_to_repo>/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

Here's the assignments for the *Robotics Specialization: Computational Motion Planning* offered by UPenn on Coursera. 

Throughout this course, we have gained knowledge on different techniques for planning robot motions. These include graph-based methods, randomized planners, and artificial potential fields.

# Graph-based Methods

Graph-based methods tackle the challenge of planning routes for robots in environments with discrete positions, such as grids. To address this problem, we can represent such scenarios as graphs, where nodes signify grid locations, and edges indicate the routes between neighboring grid cells. In this course, we learned and implemented two graph-based algorithms, Dijkstra and the A* algorithm.

## Dijkstra Algorithm
Dijkstra algorithm is a greedy algorithm for finding shortest path in weighted graphs. Invented by Dutch computer scientist Edsger W. Dijkstra in 1956, Dijkstra's algorithm has a wide range of applications in computer science, from network routing protocols to GPS nevigation. The algorithm works by exploring all possible paths from a starting node to a target node, keeping track of the distance traveled along each path. It then selects the shortest path and marks it as the optimal route. Here is a pseudocode for the Dijkstra algorithm.


    start_node start
    for each node v:
        v.dist = infinity
    start.dist = 0
    list = [start]
    while list is not empty:
        current = node in list with smallest distance
        list.del(current)
        for n in current.neighbor:
            if n.dist > current.dist + dist(n, current):
                n.dist = current.dist + dist(n, current)
                n.parent = current
                if n not in list:
                    list.append(n) 


When the algorithm visits a node, it updates the distances to its neighbors based on the shortest path found so far. Since any new paths to the neighbors would have to go through the current node, the loop invariant is true after updating the distances to its neighbors. When the algorithm terminates, all nodes in the graph have been visited and the loop invariant holds for all nodes. Therefore, the algorithm has found the shortest path from the start node to all other nodes in the graph. With naive implementation, the Dijkstra algorithm has time complexity $\mathcal{O}(|V|^2)$. Using priority queue, this can be reduced to $\mathcal{O}((|V|+|E|)\log(|V|))$.

Belowing is a demo result of Dijkstra algorithm implemented in Matlab. The green and yellow block are the start and target position. The blue blocks are the nodes in the list. The red blocks are the nodes with the shortest path have been found and removed from the list.
{{< figure align=center src="/projects/robotics_specialization/imgs/course2/dijkstra.gif" width="100%" title="Dijkstra Algorithm">}}

The Dijkstra algorithm can be computationally expensive on large graphs. Since the algorithm treats all nodes equally and does not consider any heuristic information about the target node, it may explore unnecessary paths. This makes it less suitable for real-time applications.

To overcome these limitations, the A* algorithm was developed by Peter Hart, Nils Nilsson and Bertram Raphael of Stanford Research Institute in 1968[ref]. 
A* algorithm enhances Dijkstra's algorithm by introducing a heuristic function, which provides an estimate of the remaining distance from a particular node to the target node. 

The heuristic function needs to satisfy the two criteria:
- $H(goal) = 0$
- For any $x,y\in V$, $H(x)\leq H(y) + d(x,y)$

By those two properties, for any node $v\in V$, we have
$$H(v) \leq \text{length of shortest path from v to goal}$$
So the heuristic function $H(v)$ is an underestimate for the distance between the node $v$ and the goal. With the heuristic function, the algorithm chooses the node that is most likely to be on the shortest path between start and goal node.

Here is a pseudocode for the A* algorithm.

    start_node start, goal_node goal
    for each node v:
        v.dist = infinity, v.f = infinity
    start.dist = 0, start.f = H(start)
    list = [start]
    while list is not empty:
        current = node in list with smallest f value
        list.del(current)
        if current == goal:
            return success
        for n in current.neighbor:
            if n.dist > current.dist + dist(n, current):
                n.dist = current.dist + dist(n, current)
                n.f = n.g+H(n)
                n.parent = current
                if n not in list:
                    list.append(n) 

Belowing is a demo result of A* algorithm implemented in Matlab.
{{< figure align=center src="/projects/robotics_specialization/imgs/course2/a_star.gif" width="100%" title="Dijkstra Algorithm">}}
