<page_header style="transform: rotate(-90deg); transform-origin: left top; position: absolute; left: 29px; top: 701px;">
arXiv:2102.07545v2 [cs.AI] 28 Feb 2021
</page_header>

Paper:

# Data-driven Analysis for Understanding Team Sports Behaviors

**Keisuke Fujii**

Nagoya University, Furocho 1, Nagoya, Aichi, 464-8603, JAPAN
E-mail: fujii@i.nagoya-u.ac.jp

**Understanding the principles of real-world biological multi-agent behaviors is a current challenge in various scientific and engineering fields. The rules regarding the real-world biological multi-agent behaviors such as team sports are often largely unknown due to their inherently higher-order interactions, cognition, and body dynamics. Estimation of the rules from data, i.e., data-driven approaches such as machine learning, provides an effective way for the analysis of such behaviors. Although most data-driven models have non-linear structures and high prediction performances, it is sometimes hard to interpret them. This survey focuses on data-driven analysis for quantitative understanding of invasion team sports behaviors such as basketball and football, and introduces two main approaches for understanding such multi-agent behaviors: (1) extracting easily interpretable features or rules from data and (2) generating and controlling behaviors in visually-understandable ways. The first approach involves the visualization of learned representations and the extraction of mathematical structures behind the behaviors. The second approach can be used to test hypotheses by simulating and controlling future and counterfactual behaviors. Lastly, the potential practical applications of extracted rules, features, and generated behaviors are discussed. These approaches can contribute to a better understanding of multi-agent behaviors in the real world.**

Keywords: Human behavior, Machine learning, Dynamical systems, Sports, Interpretability

## 1. Introduction

The development of measurement technologies has made possible the measurement and analysis of the movements of various organisms. For example, they have enabled an understanding of the behaviors of wild animals and athletes from data. Specifically, recent advances in sports-related measurement technologies have been reviewed by many researchers such as in [1, 2, 3]. Based on the advances, it is now possible to obtain a better understanding of the principles of real-world biological multi-agent behaviors, which is a fundamental problem in various scientific and engineering fields. The rules underlying real-world biological multi-agent behaviors are of-

ten largely unknown because the elements are not physically connected. Mathematical models based on simple rules are used to directly understand the multi-agent movements. For example, models based on social forces are widely applied, in which a force is assumed to be acting among individuals [4]. In a limited number of situations, these models are also applied to more complicated behaviors such as sports [5, 6, 7]. However, modeling the general multi-agent behaviors of living organisms in the real world (e.g., team sports) can be mathematically difficult due to their inherently higher-order social interactions, cognition, and body dynamics [8]. Therefore, to obtain a better understanding of these behaviors, a data-driven and model-free (or equation-free) approach [9, 10] is needed.

Data-driven modeling is a powerful approach such as for extracting information and making a prediction using complex real-world data. For example, learning models with complex nonlinear structures such as neural networks, are actively studied in the field of machine learning. Although these nonlinear models are often effective in terms of obtaining higher expressiveness and predictive performance, they are sometimes difficult to interpret. Hence, this study aims to bridge the gap between rule-based (or traditional sports sciences) and data-driven approaches, for which there is a trade-off between interpretability and expressiveness (or predictability). The next questions must be: what kind of nonlinear data-driven model will enable a better quantitative understanding? In a discussion of this issue regarding the relationship between cognitive science and deep neural network models [11], the authors mentioned that such models would have value if they could predict and explain phenomena, which could serve as a starting point for the establishment of new theories. In the case of complex multi-agent behaviors, existing rule-based models are too simple. To obtain a better understanding, indirect techniques using nonlinear data-driven models are required: e.g., (i) extracting the mathematical structure behind the motions, (ii) visualizing the learned representations, and (iii) modeling the components and generating plausible motions. If this requirement can be satisfied, even the results are based on a nonlinear data-driven model, it will be possible to contribute to the understanding of complex multi-agent behaviors.

In this paper, data-driven analyses for team sports behaviors are introduced, especially in invasion sports such as basketball and football, which show complex interac-

tive behaviors. A range of related surveys or dissertations have addressed the spatio-temporal aspects of this issue [12, 13, 14] with a focus on football [15], and have discussed prediction approaches via machine learning [16, 17, 18] including match outcome prediction, tactical decision making, player investments, fantasy sports, and injury prediction. The contribution of this paper is to review data-driven analyses that interpret team sports behaviors (e.g., based on the trajectory and action data of the players and ball as defined in Section 2), rather than simply performing clustering, classification, and prediction via black-box learning-based models. After the preliminary explanation of terms in Section 2, examples of data-driven approaches to extract features and rules are introduced in Section 3, including the visualization of learned representations and extraction of mathematical structures underlying the behaviors. In Section 4, an approach for testing hypotheses by simulating and controlling plausible future behaviors by generating future and counterfactual behaviors is introduced. Lastly, in Section 5, the potential for the practical application of these estimated rules, features, and generated predictions is discussed.

engineering_drawing: An example of multi-agent trajectory data in basketball showing player movements and ball path.

Fig. 1. An example of multi-agent trajectory data in basketball (illustration from [19]). The colored triangles, gray circles, and the black circle represent the defenders, attackers, and ball, respectively.

engineering_drawing: An example of a player action sequence with a ball in soccer, showing passes and shots with associated values.

Fig. 2. An example of a player action sequence with a ball in soccer (illustration from [20]).

# 2. Preliminary

The term agent is used to denote a dynamic object of interest such as a player or the ball in team sports. A single-agent trajectory $P$ of length $m$ is a sequence of $m$ features $P = (p_1, p_2, \dots, p_m)$, where $p_i \in \mathbb{R}^d$ is a features with $d$ dimensions. For example, as a feature, the $d$-dimensional coordinate is a simple case. Multi-agent trajectories $P_K$ (e.g., a team, both teams, or with the ball, such as in Figure 1) with $K$ agents comprise a sequence of $m$ features $P_K = (p_{K,1}, p_{K,2}, \dots, p_{K,m})$, where $p_{K,i} = [p_{i,1}, p_{i,2}, \dots p_{i,K}] \in \mathbb{R}^{K \times d}$. A sequence of relations in a multi-agent system $R_K$ is defined as $R_K = (R_1, R_2, \dots, R_m)$, where $R_i \in \mathbb{R}^{K \times K}$. $R_i$'s component $R_{i,k,l}$ represents the relation between agents $k$ and $l$ at each $i$, as computed by $R_{i,k,l} = h(p_{i,k}, p_{i,l})$ (e.g., $h$ is a distance function [21, 9] or a Gaussian kernel [22, 23]). In the following, actions indicate discrete behaviors such as dribble, pass, and shot, as shown in Figure 2. The objective of this paper is to present a method for obtaining a better understanding of team sports behaviors, including continuous trajectories and discrete actions.

# 3. Extracting features and rules from data

This learning-based approach is used to extract features and rules despite the availability of little prior knowledge. In this section, conventional rule-based approaches are firstly introduced, followed by unsupervised and supervised learning approaches, with particular regard to their interpretation. The unsupervised and supervised approaches comprise two of the three main categories of machine learning (the third is reinforcement learning, which is introduced in Section 4).

## 3.1. Conventional rule-based approaches

In conventional methods without learning-based approaches, researchers in various fields have evaluated the characteristics of multi-agent behaviors based on their experience and established theories. For example, based on hypotheses, researchers have calculated the distances and relative phases of two athletes (e.g., [24, 25, 8]), the speeds of movements (e.g., [26]), the frequencies and angles of actions (e.g., shots [27] and passes [28, 29, 30]), and their representative values (e.g., average and maximum values). Measurement systems with greater spatiotemporal resolution (e.g., motion capture systems and force platforms) can analyze skillful maneuvers [31, 32] in terms of their cognition [33, 34], force [35], and torque [36]. After obtaining representative values, specific hypotheses have been tested (e.g., [8, 37]) sometimes by statistical analysis. For example, in order to quantify the flexible teamwork of basketball defense (i.e., 5-vs-5), evaluation of the defensive cooperation against team attacks called screen-plays, which block the movements of a defender, was performed [8]. The results showed that the defender flexibly changes the frequency of the four

roles (i.e., switching, overlapping, ignoring, global-help) according to the level of the emergency. This traditional quantitative approach remains powerful, is applicable to small datasets, and is the easiest to interpret in a range of fields (e.g., a particular sport) because it allows for the direct test of the hypothesis.

Representative values have also been computed using more mathematically sophisticated approaches. Pioneering work was conducted in which each player’s area of control in actual soccer games was evaluated as a Voronoi diagram [38]. Other studies, for example, have analyzed the connection of passes based on network theory [39], the self-similarity hidden in a time series of the front position of the team [40], and the breaking of spatiotemporal symmetry using group theory in a 3-vs-1 ball possession task [41]. In a recent work [42], a probabilistic physics-based model was developed to quantify off-ball scoring opportunities. However, in order to represent cooperative/competitive interactions in a more detailed or practical manner, more flexible modeling would be needed. A wide variety of data-driven methods such as using machine learning have basically been developed to achieve clear objectives such as automatic feature extraction, classification, and regression. In the following subsection, unsupervised and supervised learning, which are used in the field of machine learning [43] are introduced, and examples of researches using player position data in team sports, which can be readily interpreted, are presented.

## 3.2. Unsupervised learning

Unsupervised learning involves a type of machine learning algorithm that acquires insight by inferring a function for describing hidden structures from unlabeled data. This is a powerful approach for knowledge discovery from data without the benefit of clear hypotheses. Typical unsupervised methods include dimensionality reduction and clustering. Dimensionality reduction is the transformation of high-dimensional data into a meaningful representation of lower dimensionality. For example, principal component analysis (PCA), t-distributed stochastic neighbor embedding (t-SNE) [44] regarding shot type [45], non-negative matrix factorization (NMF) [46] or tensor decomposition [47] regarding the shot type, and topic modeling [48, 49] of the trajectory (i.e., one of the natural language processing algorithms), have been used to summarize diverse interactive sports behaviors into lower-dimensional representations. However, some of these methods have assumed independence of sampling. That is, the extracted information does not reflect dynamical properties. Therefore, an extraction method identifying the coordinative structures based on dynamical properties from data is needed.

A number of approaches are used to reduce the number of dimensions while considering the time-series structures. For example, image-based approaches transform trajectory data into images using neural networks (e.g., [50, 51]), including the self-organizing map (e.g., [52, 53]). Another approach for extracting physically-interpretable dynamical information is a method called

*dynamic mode decomposition* (DMD) [54, 55]. It can decompose data into a small number of time dynamics (i.e., frequency and growth rate) and their coefficients (i.e., extraction of dynamic properties). DMD is based on the spectral theory of the Koopman operator [56, 57]. Theoretically, to compute DMD, the data must be rich enough to approximate the eigenfunctions of the Koopman operator. However, in basic DMD algorithms that naively use the obtained data, the above assumption is not satisfied e.g., when the data dimension is too small to approximate the eigenfunctions. Thus, there are several algorithmic variants of DMDs to overcome this problem such as a formulation in reproducing kernel Hilbert spaces (RKHSs) [58], in a multitask framework [59], and using a neural network [60]. Researchers have applied the DMD in RKHSs to multi-agent relation sequences (see Section 2) in team sports [21, 9] and utilized the structure of an adjacency matrix series $R_K$ (see Section 2) via tensor-train decomposition [23]. This approach has the advantage of enabling (i) the extraction of the mathematical structure and (ii) visualization of the learned expressions for the above purposes of data-driven methods.

Clustering involves grouping a set of objects such that objects in the same group (called a cluster) are more similar to each other than to those in other groups (clusters). There are many clustering algorithms based on various cluster models, e.g., hierarchical clustering (based on the connectivity or similarity between two trajectories), centroid-based clustering (such as k-means), and distribution-based clustering (such as Gaussian mixture models). For team sports data, researchers have used hierarchical clustering [61, 62] based on similarity [63, 64, 65] and distribution-based clustering using a Gaussian mixture model [66]. However, again, problems can occur when using time-series data (for example, it is difficult to naively compute a similarity when the data do not have fixed time lengths). In that case, one approach is to specifically design the similarity of time series to enable the application of the conventional clustering method to static data.

Hierarchical clustering requires appropriate distance measures. Among the several distance measures available for trajectories, the Fréchet distance [67] and dynamic time warping (DTW) [68] have been frequently used (e.g., in basketball [63, 64] soccer [63] games). However, these simple approaches have high computational costs and are difficult to apply to large-scale sports data. Therefore, researchers have developed a scalable method for computing Fréchet distance by quickly performing a search on a tree data structure called *trie* [65]. Recently developed neural network approaches can also compute the similarities of a single-agent trajectory in scalable ways [69, 70], but these have not been applied to team sports multi-agent trajectory datasets.

Another problem is the computation of the distance or similarity between multi-agent trajectories. A simple method for comparing agent-to-agent trajectories encounters permutation problems among the players [64]. One rule-based approach permutes the players nearest to the

ball used such as in [9, 23]. A data-driven permutation method such as a linear assignment, known as the Hungarian algorithm [71], has also been used for role assignment problems in Basketball [72, 73, 64, 74] and soccer [75, 76] (e.g., guard, forward, and center in basketball).

Another approach to deal with the permutation problem is calculating the similarity of multivariate nonlinear dynamical systems using DMD [21, 9]. Since DMD is a dimensionality reduction method like PCA, the extracted dynamical property is permutation-invariant. Moreover, this approach uses a kernel that reflects the dynamics via the extraction of dynamical properties. A kernel called the Koopman spectral kernel can be regarded as a similarity between multivariate nonlinear dynamical systems, which permits the use of some clustering methods. However, in general, since unsupervised learning methods do not use objective variables (labeled data), it is sometimes difficult to validate them quantitatively. To evaluate them quantitatively, combining them with the following supervised learning methods may be effective.

## 3.3. Supervised learning

Supervised learning is a machine learning task of inferring a function from supervised or labeled training data. When labeled data has discrete values such as the type of play, it is called classification, and when it has (relatively) continuous values such as position and score, it is referred to as regression. Here, classification problems of team plays or regression problems for scoring probability are considered (other regression problems such as trajectory prediction are described later). A simple approach is to input static features into classification or regression models. For example, score prediction in basketball [77, 78, 79], team identification in soccer [80], screen-play classification, [81, 82, 83], and prediction of who will obtain a basketball in rebounding situations [84] using such as linear discriminant analysis (LDA), logistic regression, or support vector machine (SVM) with the hand-crafted static features described in Section 3.1. In this process, the static features obtained from unsupervised learning (e.g., [85]) can be input into classification or regression models.

However, it is often necessary to reflect the time-series structure also when supervised learning is applied to complex multi-agent behaviors. A simple approach is to use the dynamic features obtained from unsupervised learning, as described in Section 3. For example, by the use of the above DMD and computation of similarity, defensive tactics (defending the area or players) and offensive tactics (with or without cooperation) [23] can be classified. Another supervised learning method has also been used to classify and predict the scoring probability [21, 9]. The strength of supervised learning is that the results can be clearly evaluated.

More sophisticated approaches are end-to-end approaches, which use the same model to extract features and perform predictions (i.e., classification or regression). For example, a neural network approach can be used to classify offensive plays [50], team styles [86], and attack

outcomes based on evaluating micro-actions [87]. However, a neural network approach sometimes lacks interpretability. To obtain interpretable spatial representations, researchers have developed a number of approaches that provide both predictability and interpretability, such as using matrix [88] and tensor [89, 90] factor models, and Poisson point process [91]. Other researchers have applied a supervised pattern mining method to rugby event data [92], which can also be applied to trajectory data after transforming the data into symbol sequences.

The combinations of the predictability and interpretability are related to practical applications to actual sports games because coaches and players need information such as why the score was obtained and what characteristics are observed in the subsequent plays. To explain and understand multi-agent behaviors more quantitatively or practically, it is necessary not only to improve prediction performance, but also to clarify their underlying principles (e.g., identify the mathematical structure and provide visualized representations that are interpretable). Meanwhile, if the purpose of an analysis is close to its practical application, such as simulating and controlling behaviors as discussed in the next Section 4, there may be no problems in using even black-box learning-based models.

# 4. Simulating and controlling behaviors

This approach enables verification of researchers’ hypothesis by modeling for future prediction or in situations that cannot be actually measured. In this section, conventional rule-based (or physics-based) approaches are introduced, followed by pattern-based (or data-driven) and planning-based approaches, based on the categorization of a human trajectory prediction survey [93]. Pattern-based methods approximate an arbitrary dynamics function from training data to discover statistical behavioral patterns. Planning-based methods explicitly address long-term movement goals of an agent and compute policies or path hypotheses that enable the agent to reach those goals (often formulated as reinforcement learning).

## 4.1. Conventional rule-based approaches

Traditionally, rule-based (or physics-based) methods enable researchers to determine and model the parameters of models (e.g., player position, speed, and interaction with other players). For example, the movements of players in a 3-vs-1 soccer possession task was simply modeled using three virtual social forces: spatial, avoiding, cooperative forces [5]. In actual soccer games, pass probabilities [6] and the future trajectories of players in several seconds [7] have been modeled using more complex rule-based approaches. These approaches have the advantage of providing an understanding of simulated and controlled behaviors because the users set all of the parameters. However, the adaptation of this approach to different problems (e.g., from soccer to basketball) requires additional and costly human labor.

## 4.2. Pattern-based approaches

Pattern-based or data-driven approaches learn dynamics from data using less human knowledge to solve the above problem. In studies of team sports, there have been mainly two goals in applying these approaches: simulating multi-agent trajectories over several seconds and a more long-term team outcome. To predict long-term outcomes, if short-term behaviors are ignored, it is possible to simulate behaviors until the end of the possession (or attack). Although this methodology mainly involves supervised learning, which overlaps with the content in Section 3.3, these methods are used to simulate and evaluate player behaviors, rather than extracting features and rules. In particular, researchers can use the reinforcement learning framework to evaluate either a player’s action and state, or the team state to achieve the goals described in the following Section 4.3. In this subsection, modeling methods of multi-agent trajectories are then introduced.

### 4.2.1. Simulating multi-agent trajectories

The prediction of even just a few seconds of the multi-agent trajectories in team sports, e.g., basketball and soccer, is generally difficult. That is why it is one of the benchmark problems in the field of machine learning [94, 75, 95, 76, 96]. Most methods have leveraged recurrent neural networks (RNN) [94, 75, 97, 98] including variational RNNs [95, 76], although some have utilized generative adversarial networks (e.g., [99, 100]) and variational autoencoders [101] without RNNs. Most of these methods were simply formulated as a trajectory prediction problem, whereas a few studies formulated it as an imitation learning problem (e.g., [75, 19], which is one of reinforcement learning framework utilizing demonstration of experts (i.e., data).

Most of these methods assume full observation to achieve long-term prediction in a centralized manner (e.g., [95, 76]). In such a case, an important latent factor, e.g., whose information is utilized by each agent, is not interpretable. Methods for learning attention-based observation of agents have been proposed for multi-agent in virtual environments and in real-world systems [102, 103, 19]. Other approaches such as relational (e.g., [104, 105]) or a physically-interepretable approaches [22, 23] can learn interpretable representations of interactions. Rigorously, decentralized modeling [19] is needed to enable computation of each agent observation (or contribution). Meanwhile, recent graph neural network approaches can learn permutation-equivariant features [104, 76, 106, 105], which solve the permutation problem described in Section 3.2.

Another important approach is the tactical evaluation of a predicted trajectory. For example, trajectory prediction reflecting defensive evaluations in soccer [107] and trajectory computation optimizing defensive evaluations in basketball [14]. Qualitatively, the evaluation of counterfactual prediction (i.e., predicting "what if" situations) can be used to validate the models [76, 19] based on the user’s knowledge, whereas there is no ground truth in a

counterfactual situation.

Although it is generally difficult to extract mathematical structures with such an approach that prioritizes predictive performance and performs a nonlinear transformation, there are methods that make them compatible such as in [108] with applications other than sports. Such methods can be useful for explicit modeling (e.g., observation model) of the nonlinear model when the phenomenon can be sufficiently explained or used as a starting point for various theories [11] as mentioned in the Introduction. These approaches enable realistic and visually-understandable simulations (e.g., average athlete movements and the response to unobserved movements). Potential practical applications are presented in the following Section 5.

## 4.3. Planning-based approaches

Planning-based methods explicitly address the long-term movement goals of agents and compute policies or path hypotheses that enable the agent to reach those goals. According to [93], planning-based approaches are classified into two categories: inverse and forward planning methods. Inverse planning methods estimate the action model or reward function from observed data using statistical learning techniques. In other words, this approach utilizes a reinforcement learning framework in physical spaces (or in real-world data). Although it sometimes overlaps with supervised learning in Section 3.3 and imitation learning in Section 4.2, the methods introduced here are used to evaluate actions and states of a player or a team to achieve their goals, rather than to extract features and rules or predict trajectories. Forward planning methods make an explicit assumption regarding the optimal criteria of an agent’s movements, using a pre-defined reward function (e.g., a score in team sports). These two approaches are described in this subsection.

### 4.3.1. Inverse approach using real-world data

The inverse planning approach uses statistical learning techniques to estimate an action model or reward function from observed data. Here this idea is extended to consider and value players’ actions and the team’s states. For example, with respect to shooting, valuing player’s actions by estimating the scoring and conceding probability (VAEP) [20] and estimating a state-action value function (Q-function) using an RNN [109, 110], which made interpretable using a linear model tree [111]. To evaluate the shooting action of players, researchers have investigated allocative efficiency in shot [112], the expected possession value [77, 113, 114], and the value of the space [115, 116] by extending a Voronoi diagram [38]. Regarding passing actions, similarly, researchers have used modeling and valuing of a pass [117, 118, 119], pass-receiving [120], the defender’s pass-interception [121]. In team plays, deep reinforcement learning to estimate the quality of the defensive actions was used in ball-screen defense in basketball [122]. Another approach is the qualitative evaluation of counterfactual predictions as described

above. For example, researchers have modeled the transition probabilities and shot policy tensors and have simulated seasons under alternative shot policies of interest [123].

### 4.3.2. Forward approach in virtual spaces

The forward planning approach involves the development of algorithms for the purpose of winning a competition involving humans or machines in virtual space (e.g., video games). To develop methods both in physical and virtual spaces, RoboCup (the Robot World Cup Initiative) involves attempts by robot teams to actually play a soccer game [124]. Research has been conducted on virtual games such as puzzles and shooters, and recently an open-source simulator for soccer games was published [125]. Some researchers used a 3-vs-3 basketball simulator [126], which is not currently open-source. In these studies, using reinforcement learning, the performances are expected to defeat humans in some cases (such as mastering the game of Go [127]). It is also possible to learn similar behaviors from measurement data in sports games (e.g., using imitation learning frameworks as mentioned above). However, a few studies have combined inverse and forward planning-based frameworks. For example, reinforcement learning could generate the optimal defensive team trajectory with the reward of preventing opponent scores after the imitation learning [64]. An approach to bridge this gap is an important issue for future research.

## 5. Practical applications and future directions

There are a number of possible practical applications of extracted rules, features, and generated behaviors. First, if play classification and score prediction become possible based on the extracted features and rules of multi-agent behaviors, the most directly useful application is the decrease in the workload of those who manually classified and evaluated plays by watching videos. However, it would be sometimes difficult to define specific plays that the user wishes to classify, whereas other plays can be easily defined (e.g., offensive and defensive tactics [23] in basketball). In such a case, it may be possible to collect similar plays in the form of a recommendation system based on unsupervised learning (in Section 3.2), such as in an analogous way of a search on a web page.

Regarding the short-term future prediction discussed in Section 4, these can visually present e.g., how will a certain move work for a player in the same situation as a good player, and how will the team in the next game respond to our team. In long-term prediction, predicting the game situations and results of the opponent team in the next game would be useful for tactical planning purposes. Although there are gaps between the resolution of practical application and research on tactical planning in invasive sports (e.g., formations and styles in soccer [128] and specific cooperative plays and defense styles in basketball [23]), other team sports such as baseball have

fewer such gaps [129] because most of their actions can be evaluated discretely. Since individual results can be more easily predicted in invasive team sports (especially those near the ball), many studies in recent years have evaluated the behaviors of professional athletes (e.g., [20, 130]).

Three possible future issues can be considered. One is the high cost of using location information, which currently limits its usage to professional sports. This problem is being researched with respect to both software and hardware, and we expect that it may become easier to obtain and more accurate in the future, even for estimating joint positions [131]. With greater spatiotemporal resolution, skillful maneuver in terms of their cognition, force, and torque can be analyzed as described in Section 3.1. The second is that higher (almost perfect) performance is often required for practical use. However, it may be more constructive to consider whether the results obtained by these approaches are better (less expensive with fewer mistakes) than those obtained by humans.

## 6. Conclusions

This survey focused on data-driven analyses that can be used to obtain a quantitative understanding of invasion team sports behaviors. Two approaches for understanding these multi-agent behaviors were introduced: (1) the extraction of features or rules from data in interpretable ways and (2) the generation and control of behaviors in visually-understandable ways. Lastly, the potential practical applications of extracted rules, features, and generated behaviors were discussed. The development of these approaches would contribute to a better understanding of multi-agent behaviors in the real world.

### Acknowledgements

I would like to thank Atom Scott, Masaki Onishi, and Rory Bunker for their valuable comments on this work. This work was supported by JSPS KAKENHI (Grant Numbers 19H04941, 20H04075, 20H04087) and JST Presto (Grant Number JPMJPR20CA).

### References:

[1] Nur Azmina Rahmad, Muhammad Amir As’Ari, Nurul Fathiah Ghazali, Norazman Shahar, and Nur Anis Jasmin Sufri. A survey of video based action recognition in sports. Indonesian Journal of Electrical Engineering and Computer Science, 11(3):987–993, 2018.

[2] Paresh R Kamble, Avinash G Keskar, and Kishor M Bhurchandi. Ball tracking in sports: a survey. Artificial Intelligence Review, 52(3):1655–1705, 2019.

[3] Markel Rico-González, José Pino-Ortega, Fabio Y Nakamura, Felipe Arruda Moura, Daniel Rojas-Valverde, and Asier Los Arcos. Past, present, and future of the technological tracking methods to assess tactical variables in team sports: A systematic review. Proceedings of the Institution of Mechanical Engineers, Part P: Journal of Sports Engineering and Technology, pages 281–290, 2020.

[4] Dirk Helbing and Peter Molnar. Social force model for pedestrian dynamics. Physical Review E, 51(5):4282, 1995.

[5] Keiko Yokoyama, Hiroyuki Shima, Keisuke Fujii, Noriyuki Tabuchi, and Yuji Yamamoto. Social forces for team coordination in ball possession game. Physical Review E, 97(2):022410, 2018.

[6] William Spearman, Austin Basye, Greg Dick, Ryan Hotovy, and

Paul Pop. Physics-based modeling of pass probabilities in soccer. In *Proceeding of the MIT Sloan Sports Analytics Conference*, 2017.

[7] Francisco Peralta Alguacil, Pablo Piñones Arce, David Sumpter, and Javier Fernandez. Seeing in to the future: using self-propelled particle models to aid player decision-making in soccer. In *Proceedings of the MIT Sloan Sports Analytics Conference*, 2020.

[8] Keisuke Fujii, Keiko Yokoyama, Takeshi Koyama, Akira Rikukawa, Hiroshi Yamada, and Yuji Yamamoto. Resilient help to switch and overlap hierarchical subsystems in a small human group. *Scientific Reports*, 6, 2016.

[9] Keisuke Fujii, Takeshi Kawasaki, Yuki Inaba, and Yoshinobu Kawahara. Prediction and classification in equation-free collective motion dynamics. *PLoS Computational Biology*, 14(11):e1006545, 2018.

[10] Keisuke Fujii, Naoya Takeishi, Benio Kibushi, Motoki Kouzaki, and Yoshinobu Kawahara. Data-driven spectral analysis for coordinative structures in periodic human locomotion. *Scientific Reports*, 9(1):1–14, 2019.

[11] Radoslaw M Cichy and Daniel Kaiser. Deep neural networks as scientific models. *Trends in Cognitive Sciences*, 23(4):305–317, 2019.

[12] Joachim Gudmundsson and Michael Horton. Spatio-temporal analysis of team sports. *ACM Computing Surveys (CSUR)*, 50(2):1–34, 2017.

[13] Michael Horton. *Algorithms for the Analysis of Spatio-Temporal Data from Team Sports*. PhD thesis, University of Sydney, 2018.

[14] Long Sha. Representing and predicting multi-agent data in adversarial team sports. PhD thesis, Queensland University of Technology, 2018.

[15] Karl Tuyls, Shayegan Omidshafiei, Paul Muller, Zhe Wang, Jerome Connor, Daniel Hennes, Ian Graham, William Spearman, Tim Waskett, Dafydd Steele, et al. Game plan: What AI can do for football, and what football can do for AI. *arXiv preprint arXiv:2011.09192*, 2020.

[16] Ryan Beal, Timothy J Norman, and Sarvapali D Ramchurn. Artificial intelligence for team sports: a survey. *The Knowledge Engineering Review*, 34:e28, 2019.

[17] Rory P Bunker and Fadi Thabtah. A machine learning framework for sport result prediction. *Applied Computing and Informatics*, 15(1):27–33, 2019.

[18] Milad Keshtkar Langaroudi and Mohammadreza Yamaghani. Sports result prediction based on machine learning and computational intelligence approaches: A survey. *Journal of Advances in Computer Engineering and Technology*, 5(1):27–36, 2019.

[19] Keisuke Fujii, Naoya Takeishi, Yoshinobu Kawahara, and Kazuya Takeda. Policy learning with partial observation and mechanical constraints for multi-person modeling. *arXiv preprint arXiv:2007.03155*, 2020.

[20] Tom Decroos, Lotte Bransen, Jan Van Haaren, and Jesse Davis. Actions speak louder than goals: Valuing player actions in soccer. In *Proceedings of the 25th ACM SIGKDD International Conference on Knowledge Discovery & Data Mining*, pages 1851–1861, 2019.

[21] Keisuke Fujii, Yuki Inaba, and Yoshinobu Kawahara. Koopman spectral kernels for comparing complex dynamics: Application to multiagent sport plays. In *European Conference on Machine Learning and Knowledge Discovery in Databases (ECML-PKDD’17)*, pages 127–139. Springer, 2017.

[22] Keisuke Fujii and Yoshinobu Kawahara. Dynamic mode decomposition in vector-valued reproducing kernel hilbert spaces for extracting dynamical structure among observables. *Neural Networks*, 117:94–103, 2019.

[23] Keisuke Fujii, Naoya Takeishi, Motokazu Hojo, Yuki Inaba, and Yoshinobu Kawahara. Physically-interpretable classification of network dynamics for complex collective motions. *Scientific Reports*, 10(3005), 2020.

[24] Jérôme Bourbousson, Carole Sève, and Tim McGarry. Space–time coordination dynamics in basketball: Part 2. the interaction between the two teams. *Journal of Sports Sciences*, 28(3):349–358, 2010.

[25] B Travassos, D Araújo, R Duarte, and T McGarry. Spatiotemporal coordination behaviors in futsal (indoor football) are guided by informational game constraints. *Human Movement Science*, 31(4):932–945, 2012.

[26] Jaime Sampaio, Tim McGarry, Julio Calleja-González, Sergio Jiménez Sáiz, Xavi Schelling i del Alcázar, and Mindaugas Balciunas. Exploring game performance in the national basketball association using player tracking data. *PLoS One*, 10(7):e0132894, 2015.

[27] Kirk Goldsberry. Courtvision: New visual and spatial analytics for the nba. In *2012 MIT Sloan sports analytics conference*, volume 9, pages 12–15, 2012.

[28] Vanda Correia, Duarte Araujo, Cathy Craig, and Pedro Passos. Prospective information for pass decisional behavior in rugby union. *Human Movement Science*, 30(5):984–997, 2011.

[29] Luís Vilar, Duarte Araújo, Keith Davids, Vanda Correia, and Pedro Tiago Esteves. Spatial-temporal constraints on decision-making during shooting performance in the team sport of futsal. *Journal of Sports Sciences*, 31(8):840–846, 2013.

[30] Keisuke Fujii, Yujiro Yoshihara, Yukiko Matsumoto, Keima Tose, Hideaki Takeuchi, Masanori Isobe, Hiroto Mizuta, Daisuke Maniwa, Takehiko Okamura, Toshiya Murai, et al. Cognition and interpersonal coordination of patients with schizophrenia who have sports habits. *PLoS One*, 15(11):e0241863, 2020.

[31] Keisuke Fujii, Daichi Yamashita, Shinsuke Yoshioka, Tadao Isaka, and Motoki Kouzaki. Strategies for defending a dribbler: categorisation of three defensive patterns in 1-on-1 basketball. *Sports Biomechanics*, 13(3):204–214, 2014.

[32] Keisuke Fujii, Tadao Isaka, Motoki Kouzaki, and Yuji Yamamoto. Mutual and asynchronous anticipation and action in sports as globally competitive and locally coordinative dynamics. *Scientific Reports*, 5, 2015.

[33] Sebastien Brault, Benoit Bideau, Richard Kulpa, and Cathy M Craig. Detecting deception in movement: the case of the side-step in rugby. *PLoS One*, 7(6):e37494, 2012.

[34] Keisuke Fujii, Masahiro Shinya, Daichi Yamashita, Motoki Kouzaki, and Shingo Oda. Anticipation by basketball defenders: An explanation based on the three-dimensional inverted pendulum model. *European Journal of Sport Science*, 14(6):538–546, 2014.

[35] Keisuke Fujii, Shinsuke Yoshioka, Tadao Isaka, and Motoki Kouzaki. The preparatory state of ground reaction forces in defending against a dribbler in a basketball 1-on-1 dribble subphase. *Sports Biomechanics*, 14(1):28–44, 2015.

[36] Keisuke Fujii, Daichi Yamashita, Tetsuya Kimura, Tadao Isaka, and Motoki Kouzaki. Preparatory body state before reacting to an opponent: Short-term joint torque fluctuation in real-time competitive sports. *PLoS One*, 10(5):e0128571, 2015.

[37] Paul Power, Jennifer Hobbs, Hector Ruiz, Xinyu Wei, and Patrick Lucey. Mythbusting set-pieces in soccer. *Proceedings of the MIT Sloan Sports Analytics Conference*, 2018.

[38] Tsuyoshi Taki and Jun-Ichi Hasegawa. Visualization of dominant region in team games and its application to teamwork analysis. In *Proceedings Computer Graphics International*, pages 227–235, 2000.

[39] Yuji Yamamoto and Keiko Yokoyama. Common and unique network dynamics in football games. *PLoS One*, 6(12):e29638, 2011.

[40] Akifumi Kijima, Keiko Yokoyama, Hiroyuki Shima, and Yuji Yamamoto. Emergence of self-similarity in football dynamics. *The European Physical Journal B*, 87(2):41, 2014.

[41] Keiko Yokoyama and Yuji Yamamoto. Three people can synchronize as coupled oscillators during sports activities. *PLoS Comput Biol*, 7(10):e1002181, 2011.

[42] William Spearman. Beyond expected goals. In *Proceedings of the MIT Sloan Sports Analytics Conference*, pages 1–17, 2018.

[43] Christopher M Bishop. *Pattern recognition and machine learning*. springer, 2006.

[44] Laurens van der Maaten and Geoffrey Hinton. Visualizing data using t-SNE. *Journal of Machine Learning Research*, 9:2579–2605, 2008.

[45] Rachel Marty. High-resolution shot capture reveals systematic biases and an improved method for shooter evalutation. In *Proceedings of the 2018 MIT Sloan Sports Analytics Conference*, 2018.

[46] Andrew Miller, Luke Bornn, Ryan Adams, and Kirk Goldsberry. Factorized point process intensities: A spatial analysis of professional basketball. In *International Conference on Machine Learning*, pages 235–243, 2014.

[47] Evangelos Papalexakis and Konstantinos Pelechrinis. thoops: A multi-aspect analytical framework for spatio-temporal basketball data. In *Proceedings of the 27th ACM International Conference on Information and Knowledge Management*, pages 2223–2232, 2018.

[48] Qing Wang, Hengshu Zhu, Wei Hu, Zhiyong Shen, and Yuan Yao. Discerning tactical patterns for professional soccer teams: an enhanced topic model with applications. In *Proceedings of the 21th ACM SIGKDD International Conference on Knowledge Discovery and Data Mining*, pages 2197–2206, 2015.

[49] Andrew C Miller and Luke Bornn. Possession sketches: Mapping NBA strategies. In *Proceedings of the MIT Sloan Sports Analytics Conference*, 2017.

[50] Kuan-Chieh Wang and Richard Zemel. Classifying nba offensive plays using neural networks. In *Proceedings of the MIT Sloan Sports Analytics Conference*, 2016.

[51] Akhil Nistala. Using deep learning to understand patterns of player movement in basketball. PhD thesis, Massachusetts Institute of Technology, 2018.

[52] Andreas Grunz, Daniel Memmert, and Jürgen Perl. Tactical pattern recognition in soccer games by means of special self-organizing maps. *Human Movement Science*, 31(2):334–343, 2012.

[53] Matthias Kempe, Andreas Grunz, and Daniel Memmert. Detecting tactical patterns in basketball: comparison of merge self-organising maps and dynamic controlled neural networks. *European Journal of Sport Science*, 15(4):249–255, 2015.

[54] Clarence W Rowley, Igor Mezić, Shervin Bagheri, Philipp Schlatter, and Dan S Henningson. Spectral analysis of nonlinear flows. *Journal of Fluid Mechanics*, 641:115–127, 2009.

[55] Peter J Schmid. Dynamic mode decomposition of numerical and experimental data. *Journal of Fluid Mechanics*, 656:5–28, 2010.

[56] Bernard O Koopman. Hamiltonian systems and transformation in hilbert space. *Proceedings of the National Academy of Sciences*, 17(5):315–318, 1931.

[57] Igor Mezić. Spectral properties of dynamical systems, model reduction and decompositions. *Nonlinear Dynamics*, 41(1):309–325, 2005.

[58] Yoshinobu Kawahara. Dynamic mode decomposition with reproducing kernels for koopman spectral analysis. In *Advances in Neural Information Processing Systems 29*, pages 911–919, 2016.

[59] Keisuke Fujii and Yoshinobu Kawahara. Supervised dynamic mode decomposition via multitask learning. *Pattern Recognition Letters*, 122(1):7–13, 2019.

[60] Naoya Takeishi, Yoshinobu Kawahara, and Takehisa Yairi. Learning koopman invariant subspaces for dynamic mode decomposition. In *Advances in Neural Information Processing Systems 30*, pages 1130–1140, 2017.

[61] Jennifer Hobbs, Paul Power, and Long Sha. Quantifying the value of transitions in soccer via spatiotemporal trajectory clustering. In *Proceedings of the MIT Sloan Sports Analytics Conference*, 2018.

[62] Jennifer Hobbs, Matthew Holbrook, Nathan Frank, Long Sha, and Patrick Lucey. Improved structural discovery and representation learning of multi-agent data. arXiv preprint arXiv:1912.13107, 2019.

[63] Tom Decroos, Jan Van Haaren, and Jesse Davis. Automatic discovery of tactics in spatio-temporal soccer match data. In *ACM SIGKDD International Conference on Knowledge Discovery & Data Mining*, pages 223–232, 2018.

[64] Long Sha, Patrick Lucey, Yisong Yue, Peter Carr, Charlie Rohlf, and Iain Matthews. Chalkboarding: A new spatiotemporal query paradigm for sports play retrieval. In *International Conference on Intelligent User Interfaces*, pages 336–347, 2016.

[65] Shunsuke Kanda, Koh Takeuchi, Keisuke Fujii, and Yasuo Tabei. Succinct trit-array trie for scalable trajectory similarity search. arXiv preprint arXiv:2005.10917, 2020.

[66] Matej Perše, Matej Kristan, Stanislav Kovačič, Goran Vučkovič, and Janez Perš. A trajectory-based analysis of coordinated team activity in a basketball game. *Computer Vision and Image Understanding*, 113(5):612–621, 2009.

[67] Helmut Alt and Michael Godau. Computing the Fréchet distance between two polygonal curves. *International Journal of Computational Geometry & Applications*, 5(01n02):75–91, 1995.

[68] Donald J. Berndt and James Clifford. Using Dynamic Time Warping to Find Patterns in Time Series. In *Proceedings of AAAI workshop*, 1994.

[69] Di Yao, Chao Zhang, Zhihua Zhu, Jianhui Huang, and Jingping Bi. Trajectory clustering via deep representation learning. In *2017 International Joint Conference on Neural Networks (IJCNN)*, pages 3880–3887. IEEE, 2017.

[70] Xiucheng Li, Kaiqi Zhao, Gao Cong, Christian S Jensen, and Wei Wei. Deep representation learning for trajectory similarity computation. In *2018 IEEE 34th International Conference on Data Engineering (ICDE)*, pages 617–628. IEEE, 2018.

[71] Christos H Papadimitriou and Kenneth Steiglitz. *Combinatorial optimization*, volume 24. Prentice Hall Englewood Cliffs, 1982.

[72] Patrick Lucey, Alina Bialkowski, Peter Carr, Stuart Morgan, Iain Matthews, and Yaser Sheikh. Representing and discovering adversarial team behaviors using player roles. In *Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition*, pages 2706–2713, 2013.

[73] Patrick Lucey, Alina Bialkowski, Peter Carr, Yisong Yue, and Iain Matthews. How to get an open shot: Analyzing team movement in basketball using tracking data. In *Proceedings of the MIT Sloan Sports Analytics Conference*, 2014.

[74] Long Sha, Patrick Lucey, Stephan Zheng, Taehwan Kim, Yisong Yue, and Sridha Sridharan. Fine-grained retrieval of sports plays using tree-based alignment of trajectories. arXiv preprint arXiv:1710.02255, 2017.

[75] Hoang M Le, Yisong Yue, Peter Carr, and Patrick Lucey. Coordinated multi-agent imitation learning. In *Proceedings of the 34th International Conference on Machine Learning 70*, pages 1995–2003, 2017.

[76] Raymond A Yeh, Alexander G Schwing, Jonathan Huang, and Kevin Murphy. Diverse generation for multi-agent sports games. In *Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition*, pages 4610–4619, 2019.

[77] Dan Cervone, Alexander D’Amour, Luke Bornn, and Kirk Goldsberry. Pointwise: Predicting points and valuing decisions in real time with nba optical tracking data. In *Proceedings of the MIT Sloan Sports Analytics Conference*, 2014.

[78] Yu-Han Chang, R. Maheswaran, Sheldon J. J. Kwok, Tal Levy, A. Wexler, and K. Squire. Quantifying shot quality in the nba. In *Proceedings of the MIT Sloan Sports Analytics Conference*, 2014.

[79] Patrick Lucey, Alina Bialkowski, Mathew Monfort, Peter Carr, and Iain Matthews. quality vs quantity: Improved shot prediction in soccer using strategic features from spatiotemporal data. In *Proceedings of the MIT Sloan Sports Analytics Conference*, 2014.

[80] Patrick Lucey, Dean Oliver, Peter Carr, Joe Roth, and Iain Matthews. Assessing team strategy using spatiotemporal data. In *Proceedings of the 19th ACM SIGKDD International Conference on Knowledge Discovery and Data Mining*, pages 1366–1374, 2013.

[81] Armand McQueen, Jenna Wiens, and John Guttag. Automatically recognizing on-ball screens. In *Proceedings of the MIT Sloan Sports Analytics Conference*, 2014.

[82] Avery McIntyre, Joel Brooks, John Guttag, and Jenna Wiens. Recognizing and analyzing ball screen defense in the NBA. In *Proceedings of the MIT Sloan Sports Analytics Conference*, pages 11–12, 2016.

[83] Motokazu Hojo, Keisuke Fujii, Yuki Inaba, Yoichi Motoyasu, and Yoshinobu Kawahara. Automatically recognizing strategic cooperative behaviors in various situations of a team sport. *PLoS One*, 13(12):e0209247, 2018.

[84] Motokazu Hojo, Keisuke Fujii, and Yoshinobu Kawahara. Analysis of factors predicting who obtains a ball in basketball rebounding situations. *International Journal of Performance Analysis in Sport*, pages 1–14, 2019.

[85] Alina Bialkowski, Patrick Lucey, Peter Carr, Yisong Yue, Sridha Sridharan, and Iain Matthews. Identifying team style in soccer using formations learned from spatiotemporal tracking data. In *2014 IEEE International Conference on Data Mining Workshop*, pages 9–14. IEEE, 2014.

[86] Nazanin Mehrasa, Yatao Zhong, Frederick Tung, Luke Bornn, and Greg Mori. Deep learning of player trajectory representations for team activity analysis. In *Proceedings of the MIT Sloan Sports Analytics Conference*, 2018.

[87] Anthony Sicilia, Konstantinos Pelechrinis, and Kirk Goldsberry. Deephoops: Evaluating micro-actions in basketball using deep feature representations of spatio-temporal data. In *Proceedings of the 25th ACM SIGKDD International Conference on Knowledge Discovery & Data Mining*, pages 2096–2104, 2019.

[88] Yisong Yue, Patrick Lucey, Peter Carr, Alina Bialkowski, and Iain Matthews. Learning fine-grained spatial models for dynamic sports play prediction. In *2014 IEEE International conference on Data Mining*, pages 670–679. IEEE, 2014.

[89] Stephan Zheng, Rose Yu, and Yisong Yue. Multi-resolution tensor learning for large-scale spatial data. arXiv preprint arXiv:1802.06825, 2018.

[90] Jung Yeon Park, Kenneth Theo Carr, Stephan Zhang, Yisong Yue, and Rose Yu. Multiresolution tensor learning for efficient and interpretable spatial analysis. arXiv preprint arXiv:2002.05578, 2020.

[91] Jacob Mortensen and Luke Bornn. From markov models to poisson point processes: Modeling movement in the nba. In *Proceedings of the 2019 MIT Sloan Sports Analytics Conference*, 2019.

[92] Rory Bunker, Keisuke Fujii, Hiroyuki Hanada, and Ichiro Takeuchi. Supervised sequential pattern mining of event sequences in sport to identify important patterns of play: an application to rugby union. arXiv preprint arXiv:2010.15377, 2020.

[93] Andrey Rudenko, Luigi Palmieri, Michael Herman, Kris M Kitani, Dariu M Gavrila, and Kai O Arras. Human motion trajectory prediction: A survey. *The International Journal of Robotics Research*, 39(8):895–935, 2020.

[94] Stephan Zheng, Yisong Yue, and Jennifer Hobbs. Generating long-term trajectories using deep hierarchical networks. In *Advances in Neural Information Processing Systems 29*, pages 1543–1551, 2016.

[95] Eric Zhan, Stephan Zheng, Yisong Yue, Long Sha, and Patrick Lucey. Generating multi-agent trajectories using programmatic weak supervision. In *International Conference on Learning Representations*, 2019.

[96] Yukai Liu, Rose Yu, Stephan Zheng, Eric Zhan, and Yisong Yue. Naomi: Non-autoregressive multiresolution sequence imputation. In *Advances in Neural Information Processing Systems 32*, pages 11236–11246, 2019.

[97] Thomas Seidl, Aditya Cherukumudi, Andrew Hartnett, Peter Carr, and Patrick Lucey. Bhostgusters: Realtime interactive play sketching with synthesized nba defenses. In *Proceedings of the MIT Sloan Sports Analytics Conference*, 2018.

[98] Boris Ivanovic, Edward Schmerling, Karen Leung, and Marco Pavone. Generative modeling of multimodal multi-human behavior. In *2018 IEEE/RSJ International Conference on Intelligent Robots and Systems*, pages 3088–3095. IEEE, 2018.

[99] Chieh-Yu Chen, Wenze Lai, Hsin-Ying Hsieh, Wen-Hao Zheng, Yu-Shuen Wang, and Jung-Hong Chuang. Generating defensive plays in basketball games. In *Proceedings of the 26th ACM International Conference on Multimedia*, pages 1580–1588, 2018.

[100] Hsin-Ying Hsieh, Chieh-Yu Chen, Yu-Shuen Wang, and Jung-Hong Chuang. Basketballgan: Generating basketball play simulation through sketching. In *Proceedings of the 27th ACM International Conference on Multimedia*, pages 720–728, 2019.

[101] Panna Felsen, Patrick Lucey, and Sujoy Ganguly. Where will they go? predicting fine-grained adversarial multi-agent motion using conditional variational autoencoders. In *Proceedings of the European Conference on Computer Vision (ECCV)*, pages 732–747, 2018.

[102] Yedid Hoshen. Vain: Attentional multi-agent predictive modeling. In *Advances in Neural Information Processing Systems 30*, pages 2701–2711, 2017.

[103] Li Guangyu, Jiang Bo, Zhu Hao, Che Zhengping, and Liu Yan. Generative attention networks for multi-agent behavioral modeling. In *Thirty-Fourth AAAI Conference on Artificial Intelligence*, 2020.

[104] Thomas Kipf, Ethan Fetaya, Kuan-Chieh Wang, Max Welling, and Richard Zemel. Neural relational inference for interacting systems. In *International Conference on Machine Learning*, pages 2688–2697, 2018.

[105] Colin Graber and Alexander G. Schwing. Dynamic neural relational inference. In *Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)*, June 2020.

[106] Chen Sun, Per Karlsson, Jiajun Wu, Joshua B Tenenbaum, and Kevin Murphy. Predicting the present and future states of multi-agent systems from partially-observed visual data. In *International Conference on Learning Representations*, 2019.

[107] Masakiyo Teranishi, Keisuke Fujii, and Kazuya Takeda. Trajectory prediction with imitation learning reflecting defensive evaluation in team sports. In *IEEE 9th Global Conference on Consumer Electronics (GCCE 2020)*, 2020.

[108] Marco Fraccaro, Simon Kamronn, Ulrich Paquet, and Ole Winther. A disentangled recognition and nonlinear dynamics model for unsupervised learning. In *Advances in Neural Information Processing Systems 30*, pages 3601–3610, 2017.

[109] Guiliang Liu and Oliver Schulte. Deep reinforcement learning in ice hockey for context-aware player evaluation. In *Proceedings of the 27th International Joint Conference on Artificial Intelligence*, pages 3442–3448, 2018.

[110] Guiliang Liu, Yudong Luo, Oliver Schulte, and Tarak Kharrat. Deep soccer analytics: learning an action-value function for evaluating soccer players. *Data Mining and Knowledge Discovery*, 34(5):1531–1559, 2020.

[111] Xiangyu Sun, Jack Davis, Oliver Schulte, and Guiliang Liu. Cracking the black box: Distilling deep sports analytics. In *Proceedings of the 26th ACM SIGKDD International Conference on Knowledge Discovery & Data Mining*, 2020.

[112] Nathan Sandholtz, Jacob Mortensen, and Luke Bornn. Chuckers: Measuring lineup shot distribution optimality using spatial allocative efficiency models. In *Proceedings of the MIT Sloan Sports Analytics Conference*, 2019.

[113] Daniel Cervone, Alex D’Amour, Luke Bornn, and Kirk Goldsberry. A multiresolution stochastic process model for predicting basketball possession outcomes. *Journal of the American Statistical Association*, 111(514):585–599, 2016.

[114] Javier Fernández, Luke Bornn, and Dan Cervone. Decomposing the immeasurable sport: A deep learning expected possession value framework for soccer. In *Proceedings of the MIT Sloan Sports Analytics Conference*, 2019.

[115] Dan Cervone, Luke Bornn, and Kirk Goldsberry. NBA court realty. In *Proceedings of the MIT Sloan Sports Analytics Conference*, 2016.

[116] Javier Fernandez and Luke Bornn. Wide open spaces: A statistical technique for measuring space creation in professional soccer. In *Proceedings of the MIT Sloan Sports Analytics Conference*, 2018.

[117] Paul Power, Hector Ruiz, Xinyu Wei, and Patrick Lucey. Not all passes are created equal: Objectively measuring the risk and reward of passes in soccer from tracking data. In *Proceedings of the 23rd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining*, pages 1605–1613, 2017.

[118] Floris R Goes, Matthias Kempe, Laurentius A Meerhoff, and Koen APM Lemmink. Not every pass can be an assist: a data-driven model to measure pass effectiveness in professional soccer matches. *Big Data*, 7(1):57–70, 2019.

[119] Lotte Bransen, Jan Van Haaren, and Michel van de Velden. Measuring soccer players’ contributions to chance creation by valuing their passes. *Journal of Quantitative Analysis in Sports*, 15(2):97–116, 2019.

[120] Sergio Llana, Pau Madrero, Javier Fernández, and FC Barcelona. The right place at the right time: Advanced off-ball metrics for exploiting an opponent’s spatial weaknesses in soccer. In *Proceedings of the MIT Sloan Sports Analytics Conference*, 2020.

[121] Pieter Robberechts. Valuing the art of pressing. In *StatsBomb Innovation in Football Conference*, 2019.

[122] Jiaxuan Wang, Ian Fox, Jonathan Skaza, Nick Linck, Satinder Singh, and Jenna Wiens. The advantage of doubling: a deep reinforcement learning approach to studying the double team in the nba. *arXiv preprint arXiv:1803.02940*, 2018.

[123] Nathan Sandholtz and Luke Bornn. Replaying the NBA. In *Proceedings of the MIT Sloan Sports Analytics Conference*, 2018.

[124] Hiroaki Kitano, Minoru Asada, Yasuo Kuniyoshi, Itsuki Noda, and Eiichi Osawa. Robocup: The robot world cup initiative. In *Proceedings of the First International Conference on Autonomous Agents*, pages 340–347, 1997.

[125] Karol Kurach, Anton Raichuk, Piotr Stańczyk, Michał Zając, Olivier Bachem, Lasse Espeholt, Carlos Riquelme, Damien Vincent, Marcin Michalski, Olivier Bousquet, et al. Google research football: A novel reinforcement learning environment. In *Proceedings of the AAAI Conference on Artificial Intelligence*, volume 34, pages 4501–4510, 2020.

[126] Hongyao Tang, Jianye Hao, Tangjie Lv, Yingfeng Chen, Zongzhang Zhang, Hangtian Jia, Chunxu Ren, Yan Zheng, Zhaopeng Meng, Changjie Fan, et al. Hierarchical deep multiagent reinforcement learning with temporal abstraction. *arXiv preprint arXiv:1809.09332*, 2018.

[127] David Silver, Aja Huang, Chris J Maddison, Arthur Guez, Laurent Sifre, George Van Den Driessche, Julian Schrittwieser, Ioannis Antonoglou, Veda Panneershelvam, Marc Lanctot, et al. Mastering the game of go with deep neural networks and tree search. *Nature*, 529(7587):484–489, 2016.

[128] Ryan Beal, Georgios Chalkiadakis, Timothy J Norman, and Sarvapali D Ramchurn. Optimising game tactics for football. *arXiv preprint arXiv:2003.10294*, 2020.

[129] Matthew Stephan and Kaan Koseler. A survey of baseball machine learning: A technical report. Technical report, Miami University, 2018.

[130] Luca Pappalardo, Paolo Cintia, Paolo Ferragina, Emanuele Massucco, Dino Pedreschi, and Fosca Giannotti. Playerank: data-driven performance evaluation and player ranking in soccer via a machine learning approach. *ACM Transactions on Intelligent Systems and Technology (TIST)*, 10(5):1–27, 2019.

[131] Panna Felsen and Patrick Lucey. Body shots: Analyzing shooting styles in the nba using body pose. In *Proceedings of the MIT Sloan Sports Analytics Conference*, pages 3–4, 2017.