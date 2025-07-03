# NIC Teaming

NIC Teaming is a feature that allows to join multiple physical network cards into a single logical network card.

Why may you need to combine multiple network adapters into a NIC Team?

- **Increase throughput.** For example, by joining two 1GB network cards into a NIC Team, you will get a 2Gbit/s bandwidth on a logical adapter;
- **Manage network card load balancing.** You can balance the network traffic across active NICs.
- **Fault tolerance.** If any of your network cards in a NIC Team fails, the rest cards take their functions and the connection with the server is not interrupted. Reduce single point of failure.