Our hybrid implementation builds on the works of ‘Learning to Simulate Complex Physics with Graph Networks’ by Sanchez-Gonzalez et al. (2020) and borrows the concept of hierarchical modeling from DPI-Nets by Li et al. (2018).
Our Graph Network operates on a heterograph. DGL supports the creation of heterographs, which are graphs with multiple node and edge types. Instead of creating a radius graph between all the particles and carrying out message-passing between them through multiple GNN layers for each time-slice, we have introduced the concept of hierarchical modeling in our approach. Here, we keep the original radius graph between the particles (leaf nodes). We also create particle clusters of a nominal size (3 particles) using scikit-learns K-Means algorithm and get the cluster centers which act as the hierarchical nodes (root nodes) for the clusters. Leaf-to-Root propagation generates messages from the child particle nodes and passes them to each cluster centre root node. A radius graph is created between all root nodes with a radius of connectivity of 0.020 (the radius of connectivity for the Leaf-to-Leaf graph between all particles is 0.015, which is lower than 0.020). Root-to-Root propagation occurs between the root nodes. This is the hierarchical layer. The hierarchical layer is similar to the particle layer in terms of message-passing. Both these layers are Interaction Networks with pair-wise message-passing between all nodes. Root-to-Leaf propagation occurs between the root nodes and the leaf nodes to propagate information from the hierarchical root nodes to the child particle nodes in the clusters. Finally, Leaf-to-Leaf propagation occurs between all the particle nodes. The particle nodes now have an additional hierarchical feature, which is concatenated with the original features and aggregated messages from its neighbours during Leaf-to-Leaf propagation and summed to get the updated messages in the nodes. This step is repeated for the number of message-passing steps. In our approach, we have reduced the number of message-passing steps from 10 to 4. In the original paper by Sanchez-Gonzalez et al. (2020) they use 10 steps of message-passing to gather information from 10-hop neighbours. Since, we use hierarchical nodes, it is interesting to see the effect of reducing this step to 4 and observe how our model performs. The model is able to learn well.

Special thanks to Shiyu Li and Peng Chen for assisting me over LinkedIn. Their Medium post with the PyG implementation based on the original paper helped me immensely.

Here are the links:
‘Learning to Simulate Complex Physics with Graph Networks’ by Sanchez-Gonzalez et al. (2020)
https://arxiv.org/pdf/2002.09405v1

(DPI-Nets)
‘Learning Particle Dynamics for Manipulating Rigid Bodies, Deformable Objects, and Fluids’ by Li et al. (2018)
https://groups.csail.mit.edu/robotics-center/public_papers/Li18a.pdf

Medium post By Haochen Shi, Peng Chen, Shiyu Li as part of the Stanford CS224W course project:
Simulating Complex Physics with Graph Networks: Step by Step
https://medium.com/stanford-cs224w/simulating-complex-physics-with-graph-networks-step-by-step-177354cb9b05
