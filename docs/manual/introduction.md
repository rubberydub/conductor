## Introduction

In the 22nd century, computers have vastly changed. There are far less in existence
then there are today. They are built in centralised locations with public access through
the internet for _any_ user to build and run software.  

This was the result of the implementation of quantum computing, for the following reasons:  
Firstly, for efficiency. Quantum computing and other hardware advancements resulted
in computer performance allowing vast amounts of users to concurrently employ a single
system. Secondly, and more importantly, for security. Quantum computing rendered
computer cryptography obsolete. The problems faced by the sudden absence of security
and identity on the internet was addressed by forcing users to share large, restricted
and regulated user spaces, the details of which are the contents of this introduction.  

The use of private computers is now rare.  

## Overview of the _Shared User Space Topology_

To allow a intuitive representation of large, shared user spaces the
the _Shared User Space Topology (SUST)_ was developed. The
SUST is a vast graph data structure where user processes _occupy_ the vertices.  

Every vertex is incident to _exactly 6_ other vertices.  

A system has a single SUST - the processes of _every_ user on the entire system
(except the root) occupy it concurrently. 
The data structure of the SUST is in kernel space, hence user processes (and
hence the user themselves) can only access and mutate the state of the SUST using
a limited API of system calls, discussed throughout this manual.

It's size dynamically scales with the amount of processes in the user space. There 
is a built-in upper limit, which differs between systems, but it has never been
reached.  

Restrictions on how processes may interact with each other are governed by the
state of the SUST.  

#### Characterisation

The intention of the design of every vertex being incident to exactly 6
other vertices is so that the SUST has the _standard characterisation_.
It's complete mathematical definition is non-trivial and omitted - a sufficient
one is as follows:  

Define a map between the SUST and tessellation of hexagons of side-length
1 in the real plane by the following:  

For all hexagons _h_, the 6 hexagons sharing an edge with _h_ are the images of
the 6 vertices incident with the pre-image of _h_. This map is a bijection.  

The image of this map is the _standard characterisation_. The SUST will now be
described using it.  

#### Single Occupant

A process occupies exactly 1 hex in and a hex either has exactly 1 occupant
process or is empty. Many hexes are _blocked_ and cannot be occupied.  

#### Distances

The distances between hexes is measured with two different metrics. The 
_path metric_ is the number of steps in the shortest path between two hexes
(it can not contain a blocked vertex).
The _line metric_ is the Euclidean distance between the centres of the hexes.
Consequently, it is a real number.

#### Times

The kernel will mutate the state of the SUST in intervals of 1 seconds. This
is allow the rules of the resolution of any conflicts between the requests
of different processes to be simple and to slow the rate of change in the
SUST to a rate suitable for human observation.  

#### Movement

A process may move to a unoccupied neighbouring hex every second.  
A request to move into an occupied hex is ignored.  
If 2 processes request to move into the same hex in the same second,
both are ignored.  

#### Vision

Processes (and consequently their owners) will be informed of the location
in the SUST of other processes if they have the same owner or are _visible_.
A process is visible to another process if the distance between them, using
the line metric, is smaller than the processes' _sight range_.
Sight range is managed by the kernel and is 24 by default. 

#### Pipes

Similar to vision, a process can only open a pipe to another process if the
distance between them, using the line metric, is smaller than the processes'
_pipe range_. The pipe range is also managed by the kernel but is 16 and 24 by
default. 

#### Access

Users may connect to a computer using cheap and widely available client terminal
hardware. Such terminals are small, specialised computers that run a client program to
access public computers through the internet via the _Shared User Space Access Protocol
(SUSAP)_. A SUSAP client application access to the state of the SUST and the ability
to send data and signals to the user's processes.  

#### The Master

When a SUSAP client connects to a public computer, the kernel will start a _Master User
Controller_ process (colloquially: the _master_) which has a pipe with which it
may communicate with the client with the kernel as the intermediate.  

The master is responsible for forking (at least the first) other processes and 
forwarding data to them.  

When it is created, the kernel will change the master's owner to the user. As such,
the master occupies a vertex in the SUST just like any other user process.  

#### Lack of Identity

The loss of cryptography resulted in vast changes to the topology of the internet.
While the service of public information was unaffected, and remains similar to
the present, connections requiring authentication and encryption rapidly changed.  

Internetwork security is now facilitated largely in the domain of the hardware.
Wireless connections are not used. Connections between every individual client terminals
computers are separate. In their entirety or in lengths, connections are usually
built within environments secured from physical intrusion and policed by private companies.  

of user identity, the standard practice of identity validate by direct conversation
between process arose. Such conversations typically involve a one party assessing
the claimed identity of the other with a series of questions. However, the lack


#### Master Control Context

A master's _control context_ is specifies the mode is which the client is controlling the
client. It has three possible values:
- _Manual_ - the user is actively monitoring the SUSP and commanding the master.
- _Client Scripted_ - the client is controlled by a script.
- _Server Scripted_ - the master is controlled by a script with the client monitoring.

The client must specify the context when connection to a computer. The kernel will also
check for events that indicate that the context has changed.  


The control context of a master is the central tenant of the _policies dynamically
determined determining
the treatment of a master, the processes, and the interactions in the _region_ of the SUSP
it occupies.

#### Gateways

A gateway is a section of the SUSP where masters are placed on creation. Every
gateway has a unique identifier.  
A master must clear the gateway

#### Sockets

Some hexes are permanently occupied by a _socket_. A process may read and write
to a socket triggering certain events depending on the sockets type. Only
processes adjacent to the socket may do so. Specification of socket types 
are found within the sections of this manual pertinent to domain of their 
effects.
#### Process Amount Limit.

A user may only 
#### Fork Socket

At _Process Fork Access Socket_, colloquial known as a _fork socket_,
a process may write the a users id (usually it's owners) to the socket.

required to fork a new child process.  






#### Resource Socket

At a _Resource Request Socket_, colloquial known as _resource socket_,
the kernel listens for _Resource Request API_ calls which provide limited
configuration of the resource limits imposed on a user's processes.  
A process must keep the the handle for resource socket open, and hence remain
adjacent to the socket, for the configuration to remain in effect.  
If more than 1 process opens a handle for a resource socket, all
configuration at that socket is ignored.  




#### Security issues and the openness of the SUST
a unique identifier and a limit on the amount of users that may inhabit it. Individual 
users working by themselves will typically occupy a subspace limited to one user, while
a team of users working will require a larger space, etc.
As there is no way for a user
to be securely identified by a password 
Public services with
authentication requirements, which where formally addressed with web applications, now
require a user
