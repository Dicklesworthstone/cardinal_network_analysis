# Cardinal Network Analysis

This project provides an interactive visualization and analysis of the Vatican's cardinal network using complex network science techniques. The approach is based on a study by Leonardo Rizzo, Beppe Soda, and Alessandro Iorio which demonstrated how network analysis can predict papal elections by modeling the ecclesiastical power structure.

**GitHub Repository**: [https://github.com/Dicklesworthstone/cardinal_network_analysis/tree/main](https://github.com/Dicklesworthstone/cardinal_network_analysis/tree/main)

## Overview

This application reconstructs a multilevel model of the Vatican network and applies network science algorithms to identify influential cardinals within the ecclesiastical hierarchy. The visualization demonstrates how cardinals' positions in the network correlate with their potential to be elected as Pope.

## Methodology

Our approach mirrors the study by Rizzo, Soda, and Iorio by:

1. **Constructing a Multilevel Network Model** using three primary sources:
   - Official co-memberships (Roman curia dicasteries, commissions, councils, academies)
   - Lines of episcopal consecration
   - Informal relationships

2. **Analyzing Network Centrality** through various metrics that capture different aspects of prominence and influence

3. **Visualizing the Cardinal Network** with interactive elements that reveal the underlying power structure

## Network Metrics Explained

The analysis relies on several network science metrics, each providing unique insights into a cardinal's position and influence:

### 1. Status (Eigenvector Centrality)

**What it is**: Eigenvector centrality measures a node's influence based on the influence of its connections. It rewards cardinals who are connected not only to many others, but specifically to the most influential ones.

**How it's calculated**: Mathematically, eigenvector centrality is defined as the principal eigenvector of the adjacency matrix of the network. For a cardinal i, the eigenvector centrality is:

x_i = (1/λ) ∑_j (A_ij × x_j)

Where:
- λ is the largest eigenvalue of the adjacency matrix A
- A_ij represents the connection between cardinals i and j
- x_j is the eigenvector centrality of cardinal j

**Intuition**: Think of it as measuring "connectedness to power" – a cardinal with high eigenvector centrality is well-connected to other influential cardinals, creating a power multiplier effect.

**Why it's predictive**: In the ecclesiastical hierarchy, status is a crucial factor. The Rizzo et al. study found that the elected Pope consistently scored highest in eigenvector centrality, suggesting that the College of Cardinals values connections to influential figures when selecting a new pontiff.

### 2. Information Control (Betweenness Centrality)

**What it is**: Betweenness centrality identifies nodes that act as bridges between different groups. It highlights cardinals who serve as information brokers or connectors between otherwise separate communities.

**How it's calculated**: For a cardinal v, betweenness centrality is calculated as:

BC(v) = ∑_(s≠v≠t) (σ_st(v) / σ_st)

Where:
- σ_st is the total number of shortest paths from node s to node t
- σ_st(v) is the number of those paths that pass through v

**Intuition**: Cardinals with high betweenness centrality act as bridges rather than pillars – they connect different factions or groups within the Church. They often have unique access to diverse information flows and can mediate between different ecclesiastical communities.

**Why it's predictive**: A cardinal who can bridge theological or geographical divides may be seen as capable of unifying the Church. The ability to navigate between different Church factions proves valuable in papal elections, especially when consensus is needed.

### 3. Coalition Building Ability (Composite Index)

**What it is**: A composite metric that combines multiple centrality measures to assess a cardinal's ability to form alliances and build support networks.

**How it's calculated**: The coalition index is a weighted combination of:
- 40% Eigenvector centrality
- 40% Betweenness centrality
- 20% Degree centrality (number of direct connections)

**Intuition**: This metric captures a cardinal's ability to mobilize support through a combination of status, strategic position, and direct connections. It measures political savvy within the ecclesiastical network.

**Why it's predictive**: Papal elections often require coalition building, especially when no single cardinal has overwhelming support initially. Cardinals who excel at forming alliances across different Church factions may have an advantage in the conclave process.

### 4. Age-Adjusted Index

**What it is**: The coalition index adjusted to account for the age of cardinals.

**How it's calculated**: The age adjustment applies a modest penalty to older cardinals using:

Age_adjusted_index = Coalition_index × (1 - 0.2 × (Age - Min_age) / Age_range)

**Intuition**: While experience is valued, extremely elderly cardinals might be seen as transitional figures rather than long-term leaders. This adjustment slightly favors younger cardinals while still primarily weighing network position.

**Why it's predictive**: Historical papal elections show a preference for cardinals who are experienced but not at the extreme end of the age spectrum. The College often seeks a balance between wisdom and vitality.

## Network Visualization Details

The interactive visualization represents:

- **Nodes**: Individual cardinals
- **Node Size**: Proportional to eigenvector centrality (status)
- **Node Color**: Representing ideological leaning (Liberal, Soft Liberal, Moderate, Soft Conservative, Conservative)
- **Edge Width**: Strength of connection between cardinals
- **Highlighted Node**: The elected Pope (highest eigenvector centrality)

## Implications for Papal Elections

The Rizzo, Soda, and Iorio study demonstrated that network analysis can provide remarkable insights into papal succession. Their research showed that:

1. The cardinal ultimately elected as Pope consistently ranked first in eigenvector centrality (status)
2. Information brokers (high betweenness) often play crucial roles in the conclave process
3. The combined metrics provide a robust framework for understanding ecclesiastical power dynamics

This approach reveals that beyond theological considerations, the social and organizational structure of the Church significantly influences papal elections. The College of Cardinals tends to select members who are already central to the ecclesiastical network, reinforcing institutional continuity.

## Technical Implementation

This implementation uses:

- **PyOdide**: For running Python-based NetworkX calculations in the browser
- **D3.js**: For the interactive force-directed network visualization
- **React**: For the user interface components

The simulation:

1. Generates a realistic cardinal network based on common ecclesiastical relationship patterns
2. Calculates all relevant centrality metrics using the NetworkX library
3. Renders an interactive visualization allowing exploration of the network structure
4. Provides detailed metrics and rankings for analysis

## Conclusion

This cardinal network analysis demonstrates the power of computational social science in understanding complex institutional structures. By modeling the Vatican's hierarchical network, we gain insights into how influence flows through ecclesiastical channels and how this impacts leadership selection.

The correlation between network centrality and papal election outcomes suggests that beyond spiritual and theological considerations, organizational network structure plays a crucial role in the Church's highest leadership decisions.

---

*Note: This simulation is based on the research by Leonardo Rizzo, Beppe Soda, and Alessandro Iorio but uses generated data for educational purposes. For the original research and actual Vatican network analysis, please refer to the original study.*
