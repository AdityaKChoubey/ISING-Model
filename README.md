# ISING-Model
This repository contains a Python-based simulation of the 2D Ising model, exploring phase transitions and magnetization behavior under different algorithms. The goal is to compare the efficiency and accuracy of various methods used to simulate spin dynamics in statistical physics.

You'll find implementations of several popular algorithms used to simulate the Ising model:

  Metropolis algorithm – The classic Monte Carlo method.

  Wolff algorithm – A cluster-based method that works well near criticality.

  Gibbs sampling (Heat Bath) – Spins are updated based on their neighbors' influence.


Features:
 Visualizations of the spin lattice as it evolves

 Plots for magnetization, energy, specific heat, and susceptibility

 Customizable parameters: lattice size, temperature, number of steps

 Clean code structure for extending or tweaking simulations


1. 🟠 Metropolis Algorithm (Single Spin Flip)
📌 Idea:

Update the system one spin at a time using probabilistic rules that obey detailed balance and ergodicity, ensuring convergence to the Boltzmann distribution.
🧠 Physical Motivation:

This simulates thermal fluctuations: a spin flips if it lowers the energy, or sometimes if it raises it — modeling entropy-driven disorder at finite temperatures.
🔁 Algorithm Steps:

    Choose a random spin Si,jSi,j​.

    Compute energy change ΔEΔE if the spin is flipped.

    If ΔE≤0ΔE≤0, accept the flip.

    If ΔE>0ΔE>0, accept with probability:
    P=exp⁡(−ΔEkBT)
    P=exp(−kB​TΔE​)

    Repeat this L² times per sweep (each sweep ≈ 1 Monte Carlo step).

🟡 Pros:

    Simple to implement.

    Works well far from criticality.

🔴 Cons:

    Critical slowing down near TcTc​: correlations grow, but the spin updates are too local and slow to reflect large-scale fluctuations.

2. 🔵 Wolff Algorithm (Single Cluster Flip)
📌 Idea:

Instead of flipping one spin at a time, flip a cluster of aligned spins. This reduces correlations more efficiently, especially near the critical point.
🧠 Physical Motivation:

At TcTc​, spins form large correlated domains. Flipping whole domains mimics the physical behavior and improves convergence.
🔁 Algorithm Steps:

    Choose a random seed spin.

    Grow a cluster of same-spin neighbors by adding each with probability:
    Padd=1−e−2J/kBT
    Padd​=1−e−2J/kB​T

    Use a stack/queue to recursively explore the neighbors.

    Once the cluster is built, flip all its spins.

🟢 Pros:

    Very efficient near criticality.

    Dramatically reduces critical slowing down.

🔴 Cons:

    Slightly more complex than Metropolis.

    One cluster flip is usually faster than a full sweep in Metropolis, but not necessarily one-to-one in dynamics.
3. 🟣 Swendsen–Wang Algorithm (Multiple Cluster Flip)
📌 Idea:

Construct all clusters in the lattice simultaneously and flip each with 50% probability. This is more global and parallel than Wolff.
🧠 Physical Motivation:

Decomposes the spin system into independent clusters using Fortuin–Kasteleyn representation of the Ising model — a reformulation using percolation theory.
🔁 Algorithm Steps:

    For each pair of neighboring spins:

        If they are aligned: form a bond with probability
        Pbond=1−e−2J/kBT
        Pbond​=1−e−2J/kB​T

    Identify connected components (clusters).

    Flip each cluster independently with 50% probability.

🟢 Pros:

    Very fast near TcTc​.

    Well-suited for parallel implementations.

🔴 Cons:

    More complex bookkeeping (cluster labeling).

    Less intuitive for dynamic processes (one update changes the whole lattice).
