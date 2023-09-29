# Digital Pheromones for Swarming Vehicle Control

![image](https://github.com/lawl2/swarm-pheromone-control/assets/105045290/c8889894-0b57-42bf-906d-d57a21921801)


## The aim of the project

This work aims to simulate the Digital Pheromones Model presented in the paper [Performance of Digital Pheromones for Swarming Vehicle Control](https://www.researchgate.net/publication/221455730_Performance_of_Digital_Pheromones_for_Swarming_Vehicle_Control) used to implement different swarming algorithms used by authors to support a range of military scenarios

## What about Digital Pheromones

Digital pheromones are stigmergic mechaninsm used by agents to coordinate with each other exchanging information and leaving signs in a shared environment.

- Simplicity of agents with respect to individually intelligent agents
- Scalability of stigmergic mechanism 
- Robustness of system’s performances against the loss of few individuals

The model mimics the behavior of chemical pheromones. 
Pheromones can be deposited in an area increasing the current amount of that flavor located at the place.
They can evaporate over time.
They can propagate from a place to nearby places. This leads to formation of pheromone gradients.

## Pheromone parameters

Each place agent store information about single pheromone flavors. It performs aggregation, evaporation and propagation
- 𝑃={𝑝_𝑖 } set of agents
- N :𝑃 →𝑃 neighbors relationship
- 𝑠(𝜙_𝑓, 𝑝, 𝑡) strenght of pheromone flavor f at place agent p at time t
- d(𝜙_𝑓, 𝑝, 𝑡) deposit of pheromone flavor f leaved by an agent
- g(𝜙_𝑓, 𝑝, 𝑡) propagated pheromone flavor
- 𝐸_𝑓∈(0,1) evaporation factor for flavor f
- 𝐺_𝑓∈(0,1) propagation factor for flavor f

## Pheromone equations

There are two equations describing how the digital pheromone evolves in time and how it propagates to nearby place agents

The first equation describes the evolution of the strenght of a single pheromone flavor at a given place agent

𝒔(𝝓_𝒇,𝒑,𝒕)= 𝑬_𝒇  ⌊((𝟏 − 𝑮_𝒇 )( 𝒔(𝝓_𝒇, 𝒑,𝒕−𝟏)+𝒅(𝝓_𝒇,𝒑,𝒕))+𝒈(𝝓_𝒇,𝒑,𝒕)⌋

The second equation describes the propagation received from direct neighbors
𝒈(𝝓_𝒇,𝒑,𝒕)= ∑_(𝒑^′  ∈ 𝑵(𝒑)) 〖𝑮_𝒇/|𝑵(𝒑^′ )| (𝒔(𝝓_𝒇,𝒑^′,𝒕−𝟏)+𝒅(𝝓_𝒇,𝒑^′,𝒕))〗

## Algorithms description

There are four pheromone flavors used in the algorithms Lawn〖(𝜙〗_𝑙), Visi𝑡𝑒𝑑〖(𝜙〗_𝑣), Tracking〖(𝜙〗_𝑡), NeedsID〖(𝜙〗_𝑛)
- At each update cycle, agents deposit pheromone flavors, pheromones start to propagate and evaporate.
- Agents defines their next move based on the interpreting equation 𝑉(𝑝) built in the specific algorithm, using a deterministic action choosing the maximum of the interpreting equation and moving towards one the direct neighbors place agents.
- Agents react depending on the task of the algorithm leaving or cancelling ‘signs’ over the pheromone map

## Surveillance & Patrol

The algorithm defines many Autonomous Surveillance Vehicles (ASVs) which task is to control an Area of Interest (AOI).
Place agents in AOI continually emit Lawn pheromone which propagates creating an attractive gradient. 
ASVs move using the interpreting equation emitting continously Visited pheromone:

𝑽(𝒑)=𝒔(𝝓_𝒍,𝒑)−𝒔(𝝓_𝒗,𝒑) 

This cause them to climb the gradient reaching the AOI.  Once ASV has visited the AOI the pheromone is removed the Lawn pump is stopped for a desired surveillance time defined as a parameter


## Target Aquisition 

The algorithm defines two kinds of ASVs for this scenario: 
ASVdet can detect a target thanks to the Tracking pheromone
ASVid will climb the gradient of the NeedID pheromone, identifing the target. 

This is the equation for ASVdet higly influenced by Tracking pheromone

𝑽(𝒑)=𝒔(𝝓_𝒍,𝒑)−𝒔(𝝓_𝒗,𝒑)+𝟏𝟎𝒔(𝝓_𝒕,𝒑)

This is the equations for ASVid highly influenced by NeedID pheromone

𝑽(𝒑)=𝒔(𝝓_𝒍,𝒑)−𝒔(𝝓_𝒗,𝒑)+𝟏𝟎𝒔(𝝓_𝒕,𝒑)+𝟐 〖𝟏𝟎〗^𝟏𝟏 𝒔(𝝓_𝒏,𝒑)


## Target Tracking

In this algorithm the Red avatar representing the target continually deposits Tracking pheromone, however ASV deposits a large amount of Visited pheromone, used to track it but also to inform the others that the target has been acquired.
The target now after a constant time moves forward from a place to another depositing again Tracking pheromone which will again attract ASVs establishing another contact to update it’s track.
The interpreting equation for ASVs

𝑽(𝒑)=𝒔(𝝓_𝒍,𝒑)−𝒔(𝝓_𝒗,𝒑)+𝟏𝟎𝒔(𝝓_𝒕,𝒑)




