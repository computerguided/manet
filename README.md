# Routing algorithm
_This repository demonstrates a simple routing algorithm for a mobile ad-hoc network (MANET). Using a simulation    environment, the demonstration shows how nodes can communicate with each other even when they are not connected to the same fixed network._


# Mobile Ad-Hoc Network (MANET) Simulation with Routing Algorithm

## Overview
_This repository contains a browser-based simulation of a Mobile Ad-Hoc Network (MANET) implemented using JavaScript and HTML5 Canvas. It demonstrates the routing algorithm for dynamic message routing in a decentralized, wireless ad-hoc network. The nodes in this network are mobile, and the simulation includes routing functionalities, visualization of node movement, and communication between nodes. This README describes the underlying algorithm in scientific detail and explains the design of the simulation implementation._

**The simulation algorithm is implemented in JavaScript and HTML5. Click [here](https://htmlpreview.github.io/?https://raw.githubusercontent.com/computerguided/manet/refs/heads/main/simmanet.html) to run the simulation in your browser.**

## Routing Algorithm
### Mobile Ad-Hoc Networks (MANETs)
A Mobile Ad-Hoc Network (MANET) is a collection of mobile nodes that communicate wirelessly without relying on a fixed infrastructure. Nodes in MANETs have the ability to self-organize and create routes to facilitate data transmission. Due to mobility, nodes dynamically join and leave the network, leading to changes in the connectivity graph over time.

### Routing Algorithm Description
The routing algorithm implemented in this simulation is a distance-vector based dynamic routing protocol, which enables nodes to discover and maintain paths to designated destination nodes. The algorithm achieves efficient route establishment and maintenance by exchanging routing messages across connected neighbors.

The two primary message types used by nodes are:
1. **RouteIndication Message**: Used to establish and update routes to destination nodes.
2. **RouteLost Message**: Used to remove routes when connectivity to a relaying neighbor is lost.

#### Algorithm Rules
The algorithm is designed to achieve the following properties:
1. **Automatic Route Setup**: When a node is designated as a destination, it initiates a route setup process by broadcasting `RouteIndication` messages to its neighbors every 5 seconds.
2. **Route Maintenance**: Nodes use hop count and message counters to update or establish new routes to destination nodes, ensuring the shortest available path is maintained.
3. **Dynamic Topology Adaptation**: Nodes periodically move, join, and leave the network. Connectivity between nodes changes over time, which affects routes. The algorithm updates routing tables by processing both `RouteIndication` and `RouteLost` messages.

#### Routing Messages
- **RouteIndication**: Contains a `destination`, `relayer`, `hopCount`, and `counter` to inform connected neighbors about routes. A node forwards `RouteIndication` if the counter value is higher than previously seen, thereby enabling the propagation of updated routing information.
- **RouteLost**: Issued when a route to a destination becomes unavailable, typically when a neighbor disconnects. It informs other neighbors to remove routes from their routing tables.

#### Routing Table Maintenance
Each node maintains a routing table containing:
- **Destination**: The target node to be reached.
- **Relayer**: The neighbor node that relays packets toward the destination.
- **Hop Count**: The number of hops to reach the destination.
- **Counter**: The last message counter received for this destination.

The routing table is updated based on the following rules:
1. When a **new route** is received (via `RouteIndication`) with a **lower hop count** than the existing route, it replaces the previous entry.
2. **RouteLost** messages propagate route removal to all neighbors dependent on the lost relayer.
3. Routes are maintained as long as nodes remain within the transmission range of each other.

#### Random Walk and Mobility
The simulation includes a **random walk** option for nodes, enabling them to move autonomously. Random mobility allows for a changing connectivity graph and tests the algorithm's adaptability in real time. Node velocity can also be controlled by the user, allowing flexibility in the simulation speed.

### Key Characteristics of the Algorithm
- **Proactive Routing**: The destination node periodically broadcasts route updates, allowing nodes to have up-to-date routing information to destinations.
- **Decentralized Control**: There is no centralized controller; every node is autonomous, and decisions are based on local knowledge.
- **Adaptation to Network Dynamics**: The algorithm can adapt to changes in topology, enabling routing table updates when links break or new links are formed.

## Implementation Design
### User Interface and Controls
The HTML-based simulation provides a user interface to control network parameters:
1. **Number of Nodes**: The total number of nodes participating in the network.
2. **Transmission Range**: Defines the maximum range within which nodes can communicate.
3. **Random Walk and Node Speed**: Toggles mobility and sets the velocity of nodes in pixels per frame.
4. **Show Connections**: Enables or disables visualization of network links.

The simulation starts with a **control panel** where users can adjust parameters, and a **Canvas** element that visualizes the MANET.

### Implementation Details
#### HTML and CSS
The **HTML** provides the structure, including input fields and buttons for controlling the simulation. The **CSS** handles the styling, ensuring the layout is intuitive and elements are visually distinct.

#### JavaScript and Canvas
- **Node Class**: Represents each network node. It maintains properties such as position, neighbors, routing table, and message queues.
- **Routing Logic**: Each node can process incoming messages, send messages to its neighbors, and maintain its routing table according to the routing rules.
- **Canvas Rendering**: Nodes and their connections are rendered on the HTML5 canvas. Routes are shown as blue lines, while regular connections are white. Nodes are drawn as circles, with the destination node highlighted in red.
- **Mobility Handling**: Nodes move in a random direction unless **random walk** is disabled. Collisions with canvas edges cause nodes to bounce back, simulating movement within bounded space.

#### Simulation Control
The simulation is controlled by the `animate()` function, which serves as the main loop. During each iteration:
1. **Routing Updates**: Nodes send messages and update routes.
2. **Mobility Update**: If random walk is enabled, nodes change their position.
3. **Rendering**: The canvas is cleared and redrawn with updated node positions and connection lines.

### Event Handling
The simulation provides several event handlers to facilitate real-time user interactions:
- **Node Dragging**: Users can manually drag nodes to reposition them. Drag events allow observing how routing updates when nodes move in and out of range.
- **Start Button**: Reinitializes the simulation using the specified parameters.
- **Real-Time Updates**: Changes to transmission range and random walk status take effect in real time, affecting ongoing routing and connectivity.

## Conclusion
This simulation demonstrates a simplified but insightful implementation of a routing protocol for MANETs. The use of periodic `RouteIndication` messages and `RouteLost` messages enables nodes to autonomously maintain connectivity in a constantly changing environment. The decentralized nature of the algorithm, combined with the dynamic topology adaptation, makes it suitable for illustrating the fundamental behavior of MANETs, including automatic route formation, maintenance, and disconnection handling.

