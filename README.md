# Levels---Networked---Matrix
A data syncing structure

<img width="1290" height="1610" alt="image" src="https://github.com/user-attachments/assets/605e53a0-d535-4744-a73d-89522aa24f2f" />

This project is about a Traits based networking of a 3D matrix. I wanted this system for an inventory System I am making, but has more uses.

Main concept is every point in the cube is a black box. We don't care what is in the black box, just how many things chnaged. We have counters at the ends of the rows collumns levels of the cube.
ChangeCount
ChangeFinder/Indices
ChangeCount/Flag

With these when requested we can push all the changed points in the cube, and reset counters.
We want to save those changes, laso them, and push them to a double buffer stream port.
The stream ports Write buffer size is based on the players network connection(the best packet size we can send, how often we can send it, and the quality of the connection).
The read buffer can either clear on send, or wait for connfirmation of recieving the packet from the host(so we hold the packet/frame in the buffer until it gets sent out).

Each point of the cube should also have a buffer of changes, and a snapshot feature(so we can set the entire cube state as a clean slate moment, before recording changes again),
So new players joining in, can load in that clean slate, and load in deltas as we go(alot like file backups with incremental change saving).

This will be a distributed, multi authority, vote based relay system. We use RPC calls to clients, and clients to hosts.

Hosts can either be selected, or choosen based on who has the best connection to the most number of players. To find best(X% of top packet test times, and then sum the difference from normal times/quantity of X% of top packet test times)

Likewise we can have service clients connect to as clients, such service clients could do stuff like keeping track of data, and iterating it with AI to determine
if players "actions" are within the relm of possibility for humans(think quantum mathmatics).

We can then also project this cube as we like, so for example in Unity we can keep the entire game in the cube, and project praghics states into the world.

This cube can also be spread across memory chips easily, and active data(aka it gets changed alot) can be moved to cubes that are spread over more ram chips(for higher performance cubes),
or if it is flagged as not changing alot it can be in the slower cubes(single chip cubes).

Authoratiative hosting patterns for data correction and what not will come later. Like wise predictive algorithms for values so we can interpret changes and correct for changes as we get them).

The hosting is kinda simular to proxmox if you know it, so we will need a min of 3 players to have data integrity from votes.

I am using a custom treats IoC DI system(one of my other repos), and a treats based MessageBus. So I can hook into the messages to set the cube points, or hook the cube points to networking on creation with Treats design patters(like share, or the other ones in my repos I have shared).

Like video encoding(or incremental file backup), we will take snapshots of teh cube also for networking, that then we can save changes as they come in, and after x number of changes take a snapshot, so when new players want to connect we can send the last snapshot and all the changes since. The number of change would be based on sending rate.

Also note alot of erlang/actor design patterns for data interaction.

Concept needing to be worked on: A skip so if the stream gets to far off, we can skip to packets with recent timestamped records, and slowly push the old packets so we can catch up.

<img width="2287" height="869" alt="image" src="https://github.com/user-attachments/assets/fcdfcf95-9dcd-410c-978c-77c52c694305" />
<img width="924" height="1667" alt="image" src="https://github.com/user-attachments/assets/f09a5425-9096-4b00-b47e-3d206fd7d13e" />
<img width="1853" height="866" alt="image" src="https://github.com/user-attachments/assets/8300d0de-db63-4b4d-8abb-8fbdb01d5504" />
<img width="1627" height="1612" alt="image" src="https://github.com/user-attachments/assets/c3248f49-2144-4d39-918f-129c534ebdc0" />
<img width="1260" height="1630" alt="image" src="https://github.com/user-attachments/assets/57cafa96-9faf-4e73-b535-0c040328be06" />


Networking out-line <br>
[Distributed Redundant Interconnected Server - DRIS - Matrix Mesh (Rust On IBM) - By Alexander Nava](https://chivalrous-stock-857.notion.site/Distributed-Redundant-Interconnected-Server-DRIS-Matrix-Mesh-Rust-On-IBM-28fb93133d2180c5842cc14037c6e6e9?pvs=73)
