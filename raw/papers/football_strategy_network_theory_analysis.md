# A network theory analysis of football strategies

Javier López Peña<sup>\*</sup> and Hugo Touchette<sup>†</sup>

November 27, 2024

## Abstract

We showcase in this paper the use of some tools from network theory to describe the strategy of football teams. Using passing data made available by FIFA during the 2010 World Cup, we construct for each team a weighted and directed network in which nodes correspond to players and arrows to passes. The resulting network or graph provides a direct visual inspection of a team’s strategy, from which we can identify play pattern, determine hot-spots on the play and localize potential weaknesses. Using different centrality measures, we can also determine the relative importance of each player in the game, the ‘popularity’ of a player, and the effect of removing players from the game.

## 1 Introduction

Graphs or networks arise in the study of a variety of problems, ranging from technological and transport issues to social phenomena and biological problems [1–3]. Their prevalence is such that a rich mathematical theory has been developed around them, notably by Euler, in relation to the Königsberg bridge problem, Erdös and many others.

In the world of sports, team sports involving passes between players provides one with interesting examples of networks. Our goal in this paper is to show how the mathematical theory of networks can be used to analyze statistical information of team sports and measure the performance of a team and its players. As a proof of concept, we apply our ideas to construct a network analysis of some of the teams participating in the football 2010 World Cup.

Arguably the most popular sport in the world, football (soccer for our American readers) has traditionally lagged behind other sports, such as baseball or basketball, in terms of statistical information made available after games. The unique nature of football games, with their constant ball flow and comparatively low scores compared to other sports, makes simple statistics such as assists or number of goals insufficient as measures of team and player performance.

Fortunately, it seems that the situation is changing. In recent years, starting with the UEFA 2008 Euro Cup, an unprecedented amount of statistical data has been made public after games. The release of significantly larger amounts of data opens up the way for building new and more detailed analyses of football. Some recent attempts in this direction can be found in [4–9].

Here we focus on finding a quantifiable representation of a team’s style using network theory. All renowned football teams in history have displayed a recognizable footprint in their game-play, which has always been thought of as something observed by football experts rather than described by game statistics. To reveal this footprint, we use the passing distribution of a team to construct a weighted and oriented network, with nodes corresponding to players and weighted arrows to the number of successful passes between players. By attaching each node to the tactical positioning of the team, we then obtain an immediate picture of the team’s style, which can profitably be used to observe overused and underused areas of the pitch or to detect potential performance problems between certain players. By computing certain network invariants, such as centrality measures, we can also analyze a team’s performance as well as the contributions of each of its players. These measures yield, as will be seen, a lot of useful information despite the relatively small size (11 players) of passing networks.

## 2 The network of a football team

We define the passing network of a team as the network containing the team’s players as nodes and connecting arrows between two players weighted by the successful number of passes completed between them. Although networks are, technically speaking, only topological in nature, we use the passing network as a tool for visualizing a team’s strategy by fixing its nodes in positions roughly corresponding to the players’ formation on the pitch (see Figure 1).

The passing network is by all means an oversimplification of a football game, as players do not remain in static positions during games. However, the network, with its arrows represented in various thickness and hue, does provides an immediate insight into a teams’ tactics. It can

flow_chart: Passing networks for the Netherlands and Spain

Figure 1: Passing networks for the Netherlands and Spain drawn before the final game, using the passing data and tactical formations of the semi-finals.

be used, for example, to determine areas of the pitch that are favored or neglected, whether the team tends to use or abuse short distance or long distance passes, and whether a player is not intervening enough in a game. The network can also be used by a team to detect under-performing players, fix weak spots, detect potential problems between teammates who are not passing the ball as often as their position dictates, as well as to detect weaknesses in rivals.

This basic visual analysis can be made more quantitative by computing global network invariants, which characterize a team as a whole, or local invariants, which provide insight about individual players. The computation of some of these invariants is described in the next section. They all rely on the (weighted) adjacency matrix $A$, having as entry $A_{ij}$ the number of passes from player $i$ to player $j$.

The weight (number of passes) will be used as a measure of the strength of an arrow in the network and also to define a notion of distance between players. This distance $d_{ij}$ is defined precisely as the geodesic distance given by the length of the shortest path connecting the nodes $i$ and $j$, where the length of a path is obtained by adding the lengths $l_{ij}$ of the arrows according to

$$l_{ij} = \begin{cases} 0 & \text{if } i = j \\ \frac{1}{A_{ij}} & \text{if } i \neq j. \end{cases}$$

The length of an arrow between two players is considered infinite if they do not pass the ball to each other. It is worth noting that our definition of distance does not need to be symmetric (i.e., one can have $d_{ij} \neq d_{ji}$), and is not necessarily correlated with the physical distance between the players in the field. For some computations we will also use the non-weighted adjacency matrix $\mathcal{E} = (\varepsilon_{ij})$, where

$$\varepsilon_{ij} = \begin{cases} 1 & \text{if } A_{ij} \neq 0 \\ 0 & \text{if } A_{ij} = 0. \end{cases}$$

An example of global invariant is the node connectivity, defined as the minimum number of nodes one needs to remove in a network to make it disconnected. By the very nature of football games, this is not a good invariant to consider because the passing network of a team is usually very close to be complete, and thus has a very high degree of node connectivity. More useful is the edge connectivity, defined as the minimum number of edges one needs to remove to make the network disconnected. This gives us a good measure of the game-play robustness, as it represents the smallest number of passes that need to be intercepted to interrupt a team’s ‘natural flow’ and to isolate a subset of its players, either by preventing the ball from reaching them or, if the edge connectivity is computed without the passes’ directions, by completely isolating them from the rest of their teammates.

## 3 Player performance

The individual contribution of a player in a team can be inferred from local network invariants of the passing network and, in particular, from centrality measures, which define the relevance or popularity of a player according to different parameters. We define in this section three of these

measures and discuss their meaning in the context of football.

## 3.1 Closeness

The closeness centrality or closeness score of a player $i$ is one of the simplest notion of node centrality, defined as the inverse of the average geodesic distance of that node in the network [2]:

$$C_i = \frac{20}{\sum_{j \neq i} d_{ij} + \sum_{j \neq i} d_{ji}}.$$

For simplicity, we are giving equal weight to outgoing and incoming passes in this measure, but this can be adjusted for by throwing in arbitrary weights into the equation:

$$C'_i = \frac{10}{w \sum_{j \neq i} d_{ij} + (1 - w) \sum_{j \neq i} d_{ji}}.$$

The closeness score provides a direct measurement on how easy it is to reach a particular player within a team. A high closeness score corresponds to a small average distance, indicating a well-connected player within the team.

## 3.2 Betweenness

A very different notion of centrality is betweenness centrality, which measures the extent to which a node lies on paths between other nodes [2]. This quantity is defined as the percentage of shortest paths that go through player $i$:

$$C_B(i) = \frac{1}{90} \sum_{j \neq k \neq i} \frac{n_{jk}^i}{g_{jk}}$$

where $n_{jk}^i$ is the number of geodesic paths from $j$ to $k$ going through $i$ and $g_{jk}$ is the total number of geodesic paths. The normalization factor 1/90 ensures $0 \leq C_B(i) \leq 1$.

Betweenness does not measure how well-connected a player is, but rather how the ball-flow between other players depends on that particular player $i$. It thus provides a measure of the impact of removing that player from the game, either by getting a red card or by being isolated by the rival's defense. A betweenness score of 0 means, in particular, that a player is not getting involved in the game, and so can be removed without much effect.

From a tactical point of view, a team should seek betweenness scores that are evenly distributed among players: concentrated betweenness scores that are on the high side indicate a high dependence on few, too important players, whereas well distributed, low betweenness scores are an indication of a well-balanced passing strategy.

Performance indicators based on betweenness centrality have been previously employed in the context of a team's activity [4]. Further details on the computation of betweenness in directed networks with weights can be found in [10].

## 3.3 Pagerank

Pagerank centrality, introduced in [11], is a recursive notion of 'popularity' or importance which follows the principle that 'a player is popular if he gets passes from other popular players'. Mathematically, pagerank centrality is defined by

$$x_i = p \sum_{j \neq i} \frac{A_{ji}}{L_j^{out}} x_j + q,$$

where $L_j^{out} = \sum_k A_{jk}$ is the total number of passes made by player $j$, $p$ is a heuristic parameter representing the probability that a player will decide to give the ball away rather than keep it and go for a shot himself, and $q$ is a parameter awarding a 'free' popularity to each player. Note that the pagerank score of a player depends on the scores of all his teammates. As a result, all pagerank scores in a team must be computed at the same time.

Pagerank centrality roughly assigns to each player the probability that he will have the ball after a reasonable number of passes has been made. If additional precision is required for this measurement, the probability $p$ can be replaced by player-dependent probabilities $p_i$, which would make more sense if certain players are more prone to keep the ball than others. In either case, the value of $p$ (or the $p_i$'s) does not come from the network alone, as it might in general be very different from one team to another, and should be determined by heuristics. As a proof of concept, in our analysis we will use a uniform value of $p = 0.85$ and $q = 1$ for all the teams studied.

# 4 Clustering and communities

An interesting aspect of football is how tightly players interact in a team. The notion of clustering tells us precisely that: it is a measure of the degree to which nodes in a network tend to cluster together.

The clustering coefficient of a node in a weighted network was originally defined in [12]; in our analysis we use a slight modification of that notion (see [13] for a comparison of the different definitions) given by

$$c_i^w = \frac{1}{u_i(u_i - 1)} \sum_{j,k} \frac{\sqrt[3]{A_{ij} A_{kj} A_{ki}}}{\max(A)},$$

where $u_i = \sum_j \epsilon_{ij}$ is the number of passes made by player $i$, also known as the vertex out-degree.

The clustering coefficient accounts, technically speaking, for the transitivity of the network by counting the percentage of all possible triangles containing the node $i$. To make this more precise, imagine that player $j$ wants to pass the ball to player $k$, but since this passing line is well defended, he has to go through player $i$ to reach player $k$, thus making the path $j \rightarrow i \rightarrow k$. If this is easy for them to do,

i.e., if there is a high number of passes following the path $j \rightarrow i \rightarrow k$, then this translates in a high clustering score for player $i$, the one acting as middle-man. Conversely, if there is a large unbalance between the amount of passes involved in team, then the clustering coefficient will be lower. The average of all these coefficients gives us the global *average clustering coefficient* for the team.

In addition to studying how clustered or fragmented a team is, we can compute the size of its *maximal clique*, where a clique is a sub-network in which all the nodes are linked by an arrow. A clique in a team represents a subset of players that are all pairwise-connected by direct passes. A well connected team will present a very large maximal clique, meaning that almost everybody gets to pass the ball to everybody else, whereas the size will be smaller for more fragmented teams. The analysis of cliques is the basis for finding communities within networks.

Our initial attempt at studying communities within football teams has not provided any useful information, as the high degree of connectivity paired with the small number of nodes usually results in the existence of a single community that includes every player. This said, one might be able to use some suitable variation of the notion of community to overcome this inconvenience. We will postpone the study of this problem for future work.

# 5 Results and analysis

We present in this section the results of the computation of the different measures presented in the previous sections for the teams that have participated in the 2010 FIFA World Cup. For reasons of space, we only analyze the teams that have made it to the knock-out phase, and focus especially on the final qualifiers, Spain vs the Netherlands, and the third place qualifiers, Germany vs Uruguay. The passing networks and analysis of other teams can be found on our website<sup>1</sup>.

## 5.1 Data

The passing data for the 2010 FIFA World Cup games was downloaded from the official FIFA website using a custom Python script. The passing networks were then constructed and analyzed using Sage [14] and NetworkX [15]. Graphics were created with Wolfram Research’s Mathematica.

As FIFA only provides the aggregate data over all the games, the passing networks were computed by dividing the number of passes by the total number of games played by each team. This introduces artifacts in some cases. This issue can be taken care of by conducting a per-game analysis, which was unfortunately not possible in our case.

<sup>1</sup> http://www.maths.qmul.ac.uk/~ht/footballgraphs/

<table>
  <thead>
    <tr>
        <th>Team</th>
        <th>P</th>
        <th>k</th>
        <th>ku</th>
        <th>cʷ</th>
        <th>$\overline{C_B}$</th>
        <th>Cq</th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>Argentina</td>
        <td>227</td>
        <td>4</td>
        <td>5</td>
        <td>27.9</td>
        <td>2.7</td>
        <td>8</td>
    </tr>
    <tr>
        <td>Brazil</td>
        <td><strong>321</strong></td>
        <td><strong>5</strong></td>
        <td><strong>7</strong></td>
        <td>26.2</td>
        <td>2.0</td>
        <td>8</td>
    </tr>
    <tr>
        <td>Chile</td>
        <td>120</td>
        <td>0</td>
        <td>1</td>
        <td>18.9</td>
        <td><strong>5.1</strong></td>
        <td>6</td>
    </tr>
    <tr>
        <td>England</td>
        <td>239</td>
        <td>2</td>
        <td>3</td>
        <td>28.0</td>
        <td>3.6</td>
        <td>7</td>
    </tr>
    <tr>
        <td>Germany</td>
        <td>220</td>
        <td>2</td>
        <td>2</td>
        <td>24.7</td>
        <td>4.6</td>
        <td>6</td>
    </tr>
    <tr>
        <td>Ghana</td>
        <td>184</td>
        <td>3</td>
        <td>4</td>
        <td>15.5</td>
        <td>3.5</td>
        <td>8</td>
    </tr>
    <tr>
        <td>Japan</td>
        <td>180</td>
        <td>1</td>
        <td>5</td>
        <td>28.9</td>
        <td>3.3</td>
        <td>8</td>
    </tr>
    <tr>
        <td>Korea Rep.</td>
        <td>227</td>
        <td>3</td>
        <td>5</td>
        <td>24.4</td>
        <td>2.6</td>
        <td>8</td>
    </tr>
    <tr>
        <td>Mexico</td>
        <td>225</td>
        <td>0</td>
        <td>0</td>
        <td>27.2</td>
        <td>1.9</td>
        <td>7</td>
    </tr>
    <tr>
        <td>Netherlands</td>
        <td>266</td>
        <td><strong>5</strong></td>
        <td><strong>7</strong></td>
        <td><strong>29.7</strong></td>
        <td>1.9</td>
        <td>8</td>
    </tr>
    <tr>
        <td>Paraguay</td>
        <td>103</td>
        <td>0</td>
        <td>2</td>
        <td>20.4</td>
        <td><strong>7.5</strong></td>
        <td>5</td>
    </tr>
    <tr>
        <td>Portugal</td>
        <td>175</td>
        <td>3</td>
        <td>4</td>
        <td>14.6</td>
        <td>4.1</td>
        <td>7</td>
    </tr>
    <tr>
        <td>Slovakia</td>
        <td>166</td>
        <td>3</td>
        <td>6</td>
        <td>18.5</td>
        <td>3.0</td>
        <td>7</td>
    </tr>
    <tr>
        <td>Spain</td>
        <td><strong>417</strong></td>
        <td>3</td>
        <td>5</td>
        <td><strong>30.0</strong></td>
        <td>1.9</td>
        <td><strong>9</strong></td>
    </tr>
    <tr>
        <td>USA</td>
        <td>160</td>
        <td>1</td>
        <td>4</td>
        <td>16.0</td>
        <td>4.6</td>
        <td>7</td>
    </tr>
    <tr>
        <td>Uruguay</td>
        <td>117</td>
        <td>2</td>
        <td>3</td>
        <td>14.3</td>
        <td>4.8</td>
        <td>6</td>
    </tr>
  </tbody>
</table>

Table 1: Data for the teams in the round of 16. $P$: average number of passes; $k$: edge connectivity; $k_u$: undirected connectivity; $c^w$: average clustering; $\overline{C_B}$: average betweenness; $C_q$: largest clique. The highest two values (except for clique) are highlighted.

## 5.2 Teams in the last 16

The centrality and clustering scores of the teams that made it to the last 16 stage are shown in Table 1. Note that the betweenness and clustering scores are expressed as percentage of the theoretical maximum.

The main point to note about these results is that Spain, the tournament winner and the team that arguably played the best football, has the highest number of passes, clustering and size of clique. It also has a high-end edge connectivity, while keeping a low betweenness score. All of this is a reflection of the ‘total football’ or ‘tiki-taka’ style of Spain, in which well-connected players constantly pass the ball around. This is also confirmed by the passing network (see Figure 1) and the individual players’ scores, discussed in the next subsection.

Other teams obtaining scores similar to Spain include the Netherlands (qualifying second in the tournament) and Brazil, followed by Argentina. At the lower end, Paraguay, with its low degree connectivity and and high betweenness, appears as a disconnected team relying too much on a few players.

## 5.3 Spain vs the Netherlands

Tables 2 to 3 show the closeness, betweenness, pagerank and clustering scores of the players of Spain and the Netherlands, respectively, in their formations used in the final.

Although there are some data artifacts due to the averaging of the data over several games (which again, would

<table>
  <thead>
    <tr>
        <th>Player</th>
        <th>Ci</th>
        <th>CB(i)</th>
        <th>xi</th>
        <th>cʷ<sub>i</sub></th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>Casillas</td>
        <td>16.52</td>
        <td>0.00</td>
        <td>3.29</td>
        <td>20.46</td>
    </tr>
    <tr>
        <td>Pique</td>
        <td>17.32</td>
        <td><strong>3.92</strong></td>
        <td>11.46</td>
        <td>30.70</td>
    </tr>
    <tr>
        <td>Puyol</td>
        <td>16.32</td>
        <td>2.86</td>
        <td>7.92</td>
        <td>27.12</td>
    </tr>
    <tr>
        <td>Iniesta</td>
        <td>14.60</td>
        <td>0.50</td>
        <td>8.54</td>
        <td>31.03</td>
    </tr>
    <tr>
        <td>Villa</td>
        <td>8.68</td>
        <td>0.50</td>
        <td>5.89</td>
        <td>23.96</td>
    </tr>
    <tr>
        <td>Xavi</td>
        <td><strong>18.28</strong></td>
        <td>1.19</td>
        <td><strong>14.66</strong></td>
        <td><strong>46.47</strong></td>
    </tr>
    <tr>
        <td>Capdevila</td>
        <td>16.54</td>
        <td><strong>6.12</strong></td>
        <td>10.56</td>
        <td>29.91</td>
    </tr>
    <tr>
        <td>Alonso</td>
        <td>17.11</td>
        <td>1.19</td>
        <td>12.31</td>
        <td><strong>41.69</strong></td>
    </tr>
    <tr>
        <td>Ramos</td>
        <td>16.45</td>
        <td>2.41</td>
        <td>9.02</td>
        <td>27.05</td>
    </tr>
    <tr>
        <td>Busquets</td>
        <td><strong>18.55</strong></td>
        <td>2.41</td>
        <td><strong>12.99</strong></td>
        <td>35.32</td>
    </tr>
    <tr>
        <td>Pedro</td>
        <td>3.42</td>
        <td>0.00</td>
        <td>3.35</td>
        <td>16.75</td>
    </tr>
  </tbody>
</table>

Table 2: Player scores for Spain. The two highest scores are highlighted.

be sorted out by performing a per-game analysis), the overall conclusion that we reach from these results is that there is a high correlation between high scores in closeness, pagerank and clustering, which tend to confirm the general perception of the players’ performance reported in the media at the time of the tournament. A remarkable example of this correlation is the high scores displayed by Xavi, arguably the leading player of the Spanish team.

On the Spanish side, one should also note that the betweenness scores are low and uniformly distributed – a sign of a well-balanced passing strategy – and consistently high clustering scores, showing that Spain is an extremely well-connected team, in which almost all players help each other by offering themselves as passing options. An exception is Pedro, whose low scores are explained by the fact that he is a forward and was normally not playing games for their entire duration. Incidentally, note that forwards can almost always be identified as those players having the lowest closeness, betweenness and pagerank, as they are isolated players waiting to receive passes, as well as players who get replaced more often.

The scores of the Dutch team are, overall, close to those of the Spanish team, particularly the clustering, but there are some notable differences. First, there is a clear difference in the density of passes, seen in Figure 1. Second, the Dutch players are not as close to each other (as measured by $C_i$) and have pageranks that are more evenly distributed, thus showing that none has a predominant role in the passing scheme. Finally, Figure 1 shows an unbalanced use of the pitch, giving a clear preference to the left side.

## 5.4 Germany vs Uruguay

Tables 4 to 5 shows the same data as in the previous subsection but now for Germany and Uruguay. The formations used in this case are those of the semifinals.

The results for these two teams point to a major differ-

<table>
  <thead>
    <tr>
        <th>Player</th>
        <th>Ci</th>
        <th>CB(i)</th>
        <th>xi</th>
        <th>cʷ<sub>i</sub></th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>Stekelenburg</td>
        <td><strong>16.34</strong></td>
        <td>0.32</td>
        <td>7.63</td>
        <td>28.35</td>
    </tr>
    <tr>
        <td>Van Der Wiel</td>
        <td>14.43</td>
        <td><strong>2.97</strong></td>
        <td>9.79</td>
        <td>31.39</td>
    </tr>
    <tr>
        <td>Heitinga</td>
        <td>16.23</td>
        <td>2.67</td>
        <td><strong>11.06</strong></td>
        <td>31.34</td>
    </tr>
    <tr>
        <td>Mathijsen</td>
        <td><strong>17.30</strong></td>
        <td>1.30</td>
        <td>10.84</td>
        <td>33.22</td>
    </tr>
    <tr>
        <td>V. Bronckhorst</td>
        <td>15.74</td>
        <td>1.12</td>
        <td>10.07</td>
        <td><strong>37.00</strong></td>
    </tr>
    <tr>
        <td>Van Bommel</td>
        <td>12.46</td>
        <td><strong>3.08</strong></td>
        <td><strong>11.19</strong></td>
        <td>32.36</td>
    </tr>
    <tr>
        <td>Kuyt</td>
        <td>7.97</td>
        <td>1.67</td>
        <td>9.02</td>
        <td>27.06</td>
    </tr>
    <tr>
        <td>De Jong</td>
        <td>10.95</td>
        <td>2.73</td>
        <td>9.28</td>
        <td>28.36</td>
    </tr>
    <tr>
        <td>Van Persie</td>
        <td>6.89</td>
        <td>2.92</td>
        <td>5.88</td>
        <td>20.13</td>
    </tr>
    <tr>
        <td>Sneijder</td>
        <td>10.91</td>
        <td>2.17</td>
        <td>10.32</td>
        <td><strong>33.77</strong></td>
    </tr>
    <tr>
        <td>Robben</td>
        <td>5.91</td>
        <td>0.16</td>
        <td>4.91</td>
        <td>23.91</td>
    </tr>
  </tbody>
</table>

Table 3: Player scores for the Netherlands.

<table>
  <thead>
    <tr>
        <th>Player</th>
        <th>Ci</th>
        <th>CB(i)</th>
        <th>xi</th>
        <th>cʷ<sub>i</sub></th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>Neuer</td>
        <td>7.58</td>
        <td>0.37</td>
        <td>4.74</td>
        <td>21.54</td>
    </tr>
    <tr>
        <td>Friedrich</td>
        <td>9.29</td>
        <td>3.55</td>
        <td>10.08</td>
        <td>24.99</td>
    </tr>
    <tr>
        <td>Khedira</td>
        <td>8.70</td>
        <td>10.58</td>
        <td>11.38</td>
        <td>26.31</td>
    </tr>
    <tr>
        <td>Schweinsteiger</td>
        <td>10.28</td>
        <td><strong>13.17</strong></td>
        <td><strong>17.32</strong></td>
        <td>27.35</td>
    </tr>
    <tr>
        <td>Özil</td>
        <td>7.54</td>
        <td>4.34</td>
        <td>10.05</td>
        <td>22.62</td>
    </tr>
    <tr>
        <td>Podolski</td>
        <td>4.91</td>
        <td>0.22</td>
        <td>6.66</td>
        <td><strong>30.21</strong></td>
    </tr>
    <tr>
        <td>Klose</td>
        <td>0.92</td>
        <td>0.00</td>
        <td>2.48</td>
        <td>14.34</td>
    </tr>
    <tr>
        <td>Trochowski</td>
        <td>3.00</td>
        <td>0.00</td>
        <td>2.85</td>
        <td><strong>33.02</strong></td>
    </tr>
    <tr>
        <td>Lahm</td>
        <td><strong>10.60</strong></td>
        <td><strong>11.83</strong></td>
        <td><strong>14.65</strong></td>
        <td>24.56</td>
    </tr>
    <tr>
        <td>Mertesacker</td>
        <td><strong>10.81</strong></td>
        <td>3.42</td>
        <td>13.27</td>
        <td>26.71</td>
    </tr>
    <tr>
        <td>Boateng</td>
        <td>6.85</td>
        <td>3.63</td>
        <td>6.52</td>
        <td>19.85</td>
    </tr>
  </tbody>
</table>

Table 4: Player scores for Germany.

ence in their connectedness: Germany, with its closeness scores and high clustering, is overall more connected than Uruguay. However, as its pagerank scores are not as spread out as those of Uruguay, it seems to be depending more on the efforts of a few players to pass the ball around. Lahm and Schweinsteiger, in particular, are playing a central role in their team, not dissimilar to Xavi’s.

## 6 Further work

The passing networks that we have presented provide an attractive visual summary or ‘snapshot’ of a football team’s style. The obvious limitation of these networks is of course that they are static. But, as we have seen, they can be complemented with the computation of centrality measures that provide useful information about the importance and connectedness of individual players, which might benefit coaches, sports journalists and their readers.

There are many additional features that could be added to the networks to obtain a more detailed analysis. An immediate one would be to add an extra node representing the opponent’s goal and consider shots instead of passes for ar-

<table>
  <thead>
    <tr>
        <th>Player</th>
        <th>Ci</th>
        <th>CB(i)</th>
        <th>xi</th>
        <th>c<sup>w</sup><sub>i</sub></th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>Muslera</td>
        <td>0.88</td>
        <td>1.98</td>
        <td>4.62</td>
        <td>9.96</td>
    </tr>
    <tr>
        <td>Godin</td>
        <td>1.80</td>
        <td>4.20</td>
        <td>8.37</td>
        <td>11.99</td>
    </tr>
    <tr>
        <td>Gargano</td>
        <td>0.76</td>
        <td>0.37</td>
        <td>2.98</td>
        <td>7.33</td>
    </tr>
    <tr>
        <td>Victorino</td>
        <td>1.75</td>
        <td>0.88</td>
        <td>5.59</td>
        <td>14.28</td>
    </tr>
    <tr>
        <td>Cavani</td>
        <td>1.61</td>
        <td>10.22</td>
        <td>10.22</td>
        <td>13.68</td>
    </tr>
    <tr>
        <td>Forlan</td>
        <td>2.08</td>
        <td>10.29</td>
        <td><strong>13.12</strong></td>
        <td>15.02</td>
    </tr>
    <tr>
        <td>A. Pereira</td>
        <td>1.90</td>
        <td>3.75</td>
        <td>9.01</td>
        <td>16.45</td>
    </tr>
    <tr>
        <td>Perez</td>
        <td><strong>2.36</strong></td>
        <td><strong>10.63</strong></td>
        <td><strong>15.25</strong></td>
        <td>19.12</td>
    </tr>
    <tr>
        <td>M. Pereira</td>
        <td>2.28</td>
        <td>1.51</td>
        <td>12.21</td>
        <td><strong>20.07</strong></td>
    </tr>
    <tr>
        <td>Arevalo</td>
        <td><strong>2.45</strong></td>
        <td>5.85</td>
        <td><strong>13.12</strong></td>
        <td><strong>19.83</strong></td>
    </tr>
    <tr>
        <td>Caceres</td>
        <td>1.34</td>
        <td>3.65</td>
        <td>5.52</td>
        <td>9.51</td>
    </tr>
  </tbody>
</table>

Table 5: Player scores for Uruguay.

rows directed at the goal. This concept, with one node for shots in target and one for wide shots, has been previously used in [4].

Another interesting aspect to consider would be to study the accuracy of passes by adding to each player a weight taking into account the probability for a pass coming from that player to be successful. There are different levels of complexity that one might want to get into here, as not all passes are equally likely to succeed or fail. But, as a first approximation, one might just want to use the percentage of completed passes as a measurement of accuracy.

Finally, let us mention that the defensive strength of a team could also be incorporated in the model by tracking passing interceptions and recovered balls.

## Acknowledgements

We thank J.J. Merelo Guervós for introducing us to the possibility of using network theory in the context of football, C. Clanet for inviting us to the Euromech Physics of Sports Conference and encouraging us to write down the results of our analysis as a paper, and L. Mahadevan for suggesting to study communities within football teams. We also thank FIFA for making the passing data publicly available during the duration of the World Cup and afterwards. The work of J. López Peña was supported by an EU Marie-Curie fellowship (PIEF-GA-2008-221519) and a Spanish MCIM grant (MTM2010-20940-C02-01).

## References

[1] T. G. Lewis. Network Science: Theory and Practice. John Wiley and Sons, 2009.

[2] M. E. J. Newman. Networks: An Introduction. Oxford University Press, 2010.

[3] S. Boccaletti, V. Latora, Y. Moreno, M. Chavez, and D.-U. Hwang. Complex networks: Structure and dynamics. Phys. Rep., 424:175–308, 2006.

[4] J. Duch, J. S. Waitzman, and L. A. N. Amaral. Quantifying the performance of individual players in a team activity. PloS One, 5(6):e10937, 2010.

[5] D. R. Brillinger. A potential function approach to the flow of play in soccer. J. Quant. Analys. Sports, 3(1), 2007.

[6] M. Hughes and I. Franks. Analysis of passing sequences, shots and goals in soccer. J. Sports Sci., 23(5):509–14, 2005.

[7] A. Joseph, N. E. Fenton, and M. Neil. Predicting football results using Bayesian nets and other machine learning techniques. Knowledge-Based Sys., 19(7):544–553, 2006.

[8] J. Luzuriaga. Mean free path in soccer and gases. Eur. J. Phys., 31(5):1071–1076, 2010.

[9] Y. Yamamoto and K. Yokoyama. Common and unique network dynamics in football games. PloS One, 6(12), 2011.

[10] T. Opsahl, F. Agneessens, and J. Skvoretz. Node centrality in weighted networks: Generalizing degree and shortest paths. Social Networks, 32(3):245–251, 2010.

[11] S. Brin and L. Page. The anatomy of a large-scale hypertextual Web search engine. Comp. Net. and ISDN Sys., 30(1-7):107–117, 1998.

[12] A. Barrat, M. Barthélemy, and R. Pastor-Satorras, A. Vespignani. The architecture of complex weighted networks. Proc. Nat. Acad. Sci. (USA), 101(11):3747–52, 2004.

[13] J. Saramäki, M. Kivelä, J.-P. Onnela, Kimmo Kaski, and J. Kertész. Generalizations of the clustering coefficient to weighted complex networks. Phys. Rev. E, 75(2), 2007.

[14] W. A. Stein et. al. Sage Mathematics Software (Version 5.0.1). 2012. [http://www.sagemath.org](http://www.sagemath.org).

[15] A. A. Hagberg, D. A. Schult, and P. J. Swart. Exploring network structure, dynamics, and function using NetworkX. In Proceedings of the 7th Python in Science Conference (SciPy2008), pages 11–15, Pasadena, CA USA, 2008.