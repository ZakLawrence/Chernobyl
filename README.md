# Chernobyl

Project Repository for the Chernobyl Project for the course LPHYS1201.

# Simulating a Nuclear Reactor

## Objectives

In this project, you will simulate the typical chain reaction of a nuclear reactor. To do this, you will solve the differential equations numerically and compare your results to measurements taken in existing power plants.

Next, you will analyze the Chernobyl disaster and attempt to reproduce the initial steps that led to the dramatic consequences that you know.

The Wikipedia pages [Chernobyl nuclear disaster](https://fr.wikipedia.org/wiki/Catastrophe_nucl%C3%A9aire_de_Tchernobyl) and [xenon poisoning](https://fr.wikipedia.org/wiki/Empoisonnement_au_x%C3%A9non) are a good starting point for understanding the events.

## Overview and Potential Difficulties

Modeling the processes will require you to understand (at a high level) how a nuclear power plant works. You will need to be able to use numerical solutions of differential equations and model the problem correctly.

Depending on your progress in the project, additional steps may be proposed.

## Steps

1. Reading the Wikipedia page [xenon poisoning](https://fr.wikipedia.org/wiki/Empoisonnement_au_x%C3%A9non) will have taught you about the role played by Xenon in the Chernobyl disaster. We will therefore start by simulating the abundance of this element in a reactor. The Xenon production/elimination chain is illustrated below:

![chaine_xenon](uploads/f5e0acf6340cf29ea358a498dad3f73c/chaine_xenon.png)

In summary, neutrons colliding with the fuel produce iodine, which decays into xenon with a half-life of 6.5 hours. This xenon is a good neutron absorber: its cross-section (probability of interaction) is high.

In a stable state, the xenon produced by the decay of iodine is eliminated by absorbing neutrons and undergoing further decay (half-life of 9 hours and 10 minutes). The amount of xenon remains constant. However, if the reactor is stopped, the iodine decays into xenon more rapidly than the xenon decays in turn, and the concentration of xenon increases significantly: this is the "poisoning" effect.

If one attempts to restart the reactor at this stage, the xenon will absorb a significant number of neutrons, and the chain reaction will not start until the xenon has sufficiently decayed.

The equations that govern the abundance of iodine and xenon are as follows:
```math
\begin{align*}
\frac{dI}{dt} & = \text{production} - \text{désintégration} - \text{absorbtion}\\
& = \gamma_i \Sigma_f \Phi(t) - \lambda_i I - \sigma_i I \Phi(t)\\
\frac{dX}{dt} & = \gamma_x \Sigma_f \Phi(t) + \lambda_i I - \sigma_x X \Phi(t) - \lambda_x X
\end{align*}
```
- $λ_x$ and $λ_i$ are the disintegration constants
- $γ_x$ and $γ_i$ are the production probabilities through interaction with the fuel
- $\sigma_x$ and `$\sigma_i$ are the cross-sections for neutron interactions (probability of absorbing a neutron)
- $\Phi$ is the neutron flux
- $\Sigma$ is the total interaction probability with the fuel

You can find the values of these physical constants on the Wikipedia page [xenon poisoning](https://fr.wikipedia.org/wiki/Empoisonnement_au_x%C3%A9non).

1. Use differential equation solvers to simulate the evolution of the values of $I$ and $X$ for a flux of $3\times 10^{13}$. Observe the final stable values (after 2 days). Compare them to the expected values when the derivatives are set to 0.
2. What happens if the initial amount of xenon is $2\times 10^{15}$?
3. What happens if, after 3 days, the flux suddenly drops to 0? Compare again to the expected values.
4. We will simulate the neutron flux using the following equation:
```math
\frac{d\Phi}{dt} = \frac{\Phi}{\tau} k \left( \Sigma_u - \Sigma_x - \Sigma_b \right)
```
Where:
- $\Sigma_u = 0.1355$ is the neutron cross-section with uranium
- $\Sigma_x = \sigma_xX$ is the neutron cross-section with xenon, which depends on the amount of xenon
- $\Sigma_b$ is the neutron cross-section with the slowing rods
- $k$ and $\tau$ are fixed constants at 3 and 1000 respectively.

It is assumed that the rods are raised to the safety standard at $\Sigma_b = 0.1$. They are pushed down to the maximum at $\Sigma_b = 0.2$.

Write a function that guides the movement of the rods between these extremes. The system should be able to stabilize the flux at $10^{15}$ with a starting flux of $10^{10}$.

5. When the flux is stabilized, wait for 24 hours and ask the system to reduce the flux to 1% of the normal flux. Observe the effect of poisoning.
6. Use [tkinter](https://docs.python.org/3.10/library/tkinter.html) to program a graphical interface for managing the nuclear power plant. In a first version, display a graph of the flux value over the past 24 hours with a button to start a 100-hour simulation. You can advance the time by 1 hour per second.
7. Add a button to stop the simulation at any time.
8. Add buttons to raise and lower the rods without stopping the simulation.
