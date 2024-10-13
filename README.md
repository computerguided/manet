# Routing algorithm
_This repository demonstrates a simple routing algorithm for a mobile ad-hoc network (MANET). Using a simulation    environment, the demonstration shows how nodes can communicate with each other even when they are not connected to the same fixed network._

**The simulation algorithm is implemented in JavaScript and HTML5. Click [here](https://htmlpreview.github.io/?https://raw.githubusercontent.com/computerguided/manet/refs/heads/main/simmanet.html) to run the simulation in your browser.**

## Networking

Before discussing the routing algorithm, it is important to understand the networking aspect of the simulation.

For the purpose of this simulation, the networking is simplified and follows these rules:

* **Node**: A node is a device that can communicate with other nodes.
* **Neighbor**: A neighbor is a node that has a connection to the node because their distance is equal to or less than the _transmission range_.
* **Connection**: A connection allows the two nodes to communicate with each other.

Nodes are mobile and can be moved around freely (using the mouse in the simulation). For the simplified networking implementation used here, the movement of nodes is considered to be random and nodes automatically connect - i.e. form a connection - when there distance becomes equal to or less than the _transmission range_. The transmission range is a parameter of the network and can be changed during the simulation.

When the distance becomes larger than the transmission range, the nodes automatically disconnect.

## Basic description

In the routing algorithm, certain nodes can be designated as _destinations_. The routing algorithm automatically sets-up and maintains routes to these nodes.

Once the route is set-up between two nodes, the intermediate nodes - the _relayers_ - automatically forward messages back and forth and hence the two nodes can communicate. The number of relays is denoted as the _hop-count_.

For the routing algorithm there are two messages that are exchanged between nodes:

* `RouteIndication`
* `RouteLost`

The `RouteIndication` contains the following fields.

| Field | Type | Description |
| ----- | ---- | ----------- |
| `counter` | `integer` | The counter of the message. |
| `destination` | `integer` | The destination node. |
| `relayer` | `integer` | The neighbor through which the destination can be reached. |
| `hopCount` | `integer` | The hop count to the destination. |

The `RouteLost` message contains the following fields.

| Field | Type | Description |
| ----- | ---- | ----------- |
| `destination` | `integer` | The destination node. |
| `counter` | `integer` | The counter of the last `RouteIndication` message received for this route. |

The routing algorithm is implemented by a basic set of rules:

1. A _destination_ node by definition has a 'route' to itself with a `hopCount` of 0.
2. Every 5 seconds, a destination node will send a `RouteIndication` to its connected neighbors with an incremented `counter` field.
3. When a node receives a `RouteIndication` containing a new route to a destination, it add this to its administration.
4. When a node receives a `RouteIndication` from one of its connected neighbors containing a route to a destination already in the administration, the node will replace this route if the new `hopCount` is lower than the current `hopCount`.
5. When a node receives a `RouteIndication`, it will first process the information as described in the previous rules. If the `counter` value is larger than the last one received for this route, then the node will forward this to all its connected neighbors and update the `counter` value for this route.
6. When the connection to the relaying neighbor is lost, a node will send a `RouteLost` message to all its connected neighbors.
7. When a node receives a `RouteLost` message from one of its connected neighbors and the node has a route to that destination that goes through the neighbor that sent the `RouteLost` message, the node will remove the route to the destination from its administration when the and in turn send a `RouteLost` message to its other connected neighbors.

This simple set of rules implements a powerful dynamic routing which also handles the loss of nodes and balances the load.

## Route administration

Every node has an administration of routes to destinations. The administration contains a list of routes with the following fields.

| Field | Type | Description |
| ----- | ---- | ----------- |
| `destination` | `integer` | The destination node. |
| `relayer` | `integer` | The neighbor through which the destination can be reached. |
| `hopCount` | `integer` | The hop count to the destination. |
| `counter` | `integer` | The counter of the last `RouteIndication` message received for this route. |

The following methods are required to manage the route administration:

* `addRoute(destination, relayer, hopCount, counter)`\
This method adds a route to the administration.
* `removeRoute(destination)`\
This method removes a route from the administration.
* `getRoute(destination)`\
This method returns a route to the specified destination if it exists.

Further more the following methods are required to actually implement the routing algorithm:

* `handleRouteIndication(destination, relayer, hopCount, numNeighbors, counter)`\
This method is called when a `RouteIndication` message is received.
<br>
* `handleRouteLost(destination)`\
This method is called when a `RouteLost` message is received.
<br>
* `connect(neighbor)`\
This method is called when a neighbor is connected.
<br>
* `disconnect(neighbor)`\
This method is called when a neighbor is disconnected.
