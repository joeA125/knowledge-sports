A catalog of all wiki pages, organised by type.

**Scope: football and sports analytics.** General ML, LLM and mathematical-foundations material migrated to the sibling general vault on 2026-08-14. Concepts retained here are those a football page genuinely depends on.

## Navigation

- [[overview]] — narrative map of what this vault holds and how it hangs together
- Dashboards: [[health|Health]] · [[reinforcement|Reinforcement]] · [[sources|Source Tracking]]

## Entities

### The Fujii group — nine held sources, the largest cluster here
- [[keisuke-fujii]] — author on all nine; senior author of VDEP, GVDEP, DRSO, C-OBSO, NMSTPP and SPC, first author of the adaptive action supervision paper
- [[calvin-yeung]] — NMSTPP and the game-theoretic SPC framework
- [[kosuke-toda]] — VDEP · [[rikuhei-umemoto]] — GVDEP and DRSO · [[masakiyo-teranishi]] — C-OBSO
- [[hiroshi-nakahara]] — multi-agent deep RL valuation; the inverse half of a forward/inverse pair
- [[kazushi-tsutsui]] — three held sources; crosses every division in the group
- [[kazuya-takeda]] · [[keisuke-kushiro]] · [[tony-sit]]
- [[atom-scott]] — lead author of the GFootball/real-football comparison
- [[masaki-onishi]] — AIST senior author on the agent-based simulation paper
- [[naoya-takeishi]] · [[yoshinobu-kawahara]] — the group's methodological line

### Pitch control, off-ball value and possession
- [[william-spearman]] — pass probability model, PPCF and OBSO; the substrate for much of this vault
- [[javier-fernandez]] · [[luke-bornn]] — Wide Open Spaces, SoccerMap, the soccer EPV framework
- [[daniel-cervone]] · [[alex-damour]] · [[kirk-goldsberry]] — the NBA martingale EPV paper

### Action valuation and event modelling
- [[tom-decroos]] · [[jesse-davis]] — SPADL and VAEP
- [[maaike-van-roy]] · [[pieter-robberechts]] — the xT vs VAEP critical comparison
- [[karun-singh]] — expected threat
- [[andrei-shelopugin]] — EPV of control and duel actions, PCR
- [[ian-simpson]] — Seq2Event and poss-util
- [[david-hirnschall]] · [[robert-bajons]] — the path-signature possession paper
- [[koffi-amezouwui]] · [[brigitte-gelein]] · [[matthieu-marbac]] · [[anthony-sorel]] — possession mixture-model clustering
- [[sandeep-narayanan]] — Bayesian point-process model of football events
- [[miru-hong]] · [[minho-lee]] · [[sang-ki-ko]] · [[geonhee-jo]] · [[jae-hee-so]] — EventGPT and ScoutGPT
- [[tiago-mendes-neves]] · [[luis-meireles]] · [[joao-mendes-moreira]] — Valuing Players Over Time

### Network analysis of team structure
- [[javier-lopez-pena]] · [[hugo-touchette]] — the 2010 World Cup pass-network paper; the vault's oldest football source, and its only primary source on this tradition

### Ratings and duels
- [[garry-gelade]] — Bradley-Terry model of 1v1 duel ability
- [[mark-glickman]] — Glicko and Glicko-2 · [[alexander-sirotkin]] — Glicko-2 duel and league ratings

### Computer vision for sport
- [[floriane-magera]] · [[marc-van-droogenbroeck]] — ProCC benchmarking; SoccerNet-v2
- [[jianhui-chen]] · [[james-little]] — Sports Camera Calibration via Synthetic Data
- [[pascal-bauer]] — Chair for Sports Analytics, Saarland

### Institutions and environments
- [[nagoya-university]] — nine held sources; the largest single-institution cluster
- [[aist]] · [[kyoto-university]] · [[university-of-tokyo]] · [[osaka-university]]
- [[universidade-do-porto]] · [[inesc-tec]] · [[fc-porto]] · [[fc-barcelona]]
- [[data-stadium]] — J-League data behind five held sources, each subsetting it differently
- [[stats-perform]] — STATS LLC / Opta
- [[google-research-football]] — the shared 11v11 simulator, used by one held source and rejected by another
- [[nfootball]] — the bespoke replacement, and what building your own environment costs
- [[google-brain]] · [[google-research]] · [[openai]] · [[microsoft-research]] — retained for what bears on football

## Concepts

### Action Valuation
- [[action-valuation]] — valuing individual actions via change in game-state quality
- [[expected-possession-value]] — umbrella: a possession's worth, and the four things the term means
- [[expected-goals]] · [[expected-threat]] · [[vaep]] · [[martingale-epv]] · [[pass-carry-reward]] · [[on-ball-value]]
- [[xsot]] — expected shot on target, and its off-ball counterpart
- [[intent-vs-outcome-valuation]] · [[possession-risk]] · [[effective-playing-time]] · [[temporal-discounting]]
- [[counterfactual-baseline]] — the reference against which deviation is credited
- [[structured-model-decomposition]] · [[symmetrical-duel-valuation]] · [[duel-skill-rating]]

### Off-Ball and Spatial
- [[off-ball-value]] — six mechanisms, and the disagreement between the two that have been compared
- [[obso]] · [[c-obso]] · [[space-creation]] · [[space-occupation-gain]]
- [[pitch-control]] — two traditions, framed against [[voronoi-tessellation]]
- [[pitch-value-model]] — learning what a location is worth from where defenders stand
- [[probability-surface]] · [[soccermap]] · [[single-pixel-supervision]] · [[pass-probability-model]]
- [[receiving-efficiency]] · [[dynamic-pressure-lines]] · [[tactical-analysis]]

### Defensive Valuation
- [[defensive-valuation]] · [[vdep]] · [[gvdep]] · [[drso]]

### Reinforcement Learning and Simulation
- [[reinforcement-learning]] — what sports valuation borrows, and the three frameworks here that genuinely do it
- [[multi-agent-reinforcement-learning]] · [[temporal-difference-learning]] · [[deep-q-network]] · [[proximal-policy-optimization]]
- [[action-supervision]] · [[action-space-design]] · [[imitation-reward-tradeoff]]
- [[domain-adaptation]] — Sim-to-Real and its harder inversion
- [[agent-based-simulation]] — why simulate at all
- [[policy-modelling]] · [[imitation-learning]] · [[counterfactual-simulation]] · [[rlhf]]
- [[markov-game]] · [[value-iteration]] · [[game-theory]]

### Sequence and Event Modelling
- [[event-prediction]] · [[nmstpp]] · [[seq2event]] · [[sig-model]] · [[hpus]] · [[lpv]]
- [[neural-temporal-point-process]] · [[point-process]] · [[large-event-model]] · [[eventgpt]] · [[scoutgpt]]
- [[trajectory-prediction]] · [[tokenization]] · [[constrained-decoding]] · [[player-embedding]]
- [[transformer]] · [[attention-mechanism]] · [[encoder-decoder-bottleneck]] · [[lstm]] · [[gated-recurrent-unit]]
- [[autoregressive-model]] · [[generative-model]] · [[variational-autoencoder]] · [[graph-neural-network]] · [[message-passing]]
- [[path-signature]] · [[representation-learning]] · [[feature-engineering]] · [[multi-task-learning]]

### Statistics and Inference
- [[stochastic-process]] · [[martingale]] · [[survival-analysis]] · [[competing-risks]] · [[poisson-binomial]]
- [[bayesian-inference]] · [[bayes-theorem]] · [[expectation-propagation]] · [[inla]] · [[car-prior]] · [[gaussian-process]]
- [[mixture-model]] · [[expectation-maximization]] · [[identifiability]] · [[clustering]] · [[absorbing-markov-chain]]
- [[multiresolution-modelling]] · [[kl-divergence]] · [[smoothing]] · [[player-rating-time-series]]
- [[dynamic-time-warping]] — aligning sequences that run at different rates

### Evaluation and Practice
- [[predictive-validity]] · [[split-half-reliability]] · [[construct-validity]] · [[capability-profiling]]
- [[probability-calibration]] · [[probabilistic-classification]] · [[uncertainty-quantification]] · [[class-imbalance-evaluation]]
- [[rare-event-proxy-targets]] · [[selection-bias]] · [[positive-unlabeled-learning]] · [[sample-weighting]]
- [[model-selection]] · [[interpretability]] · [[shap]] · [[gradient-boosting]] · [[random-forest]] · [[regularization]]
- [[theory-based-modelling]] · [[adjusted-rand-index]] · [[jaccard-index]]

### Ratings, Recruitment and Careers
- [[elo-rating-system]] · [[glicko-rating-system]] · [[bradley-terry-model]] · [[trueskill]] · [[league-strength-rating]]
- [[recruitment]] · [[transfer-performance-prediction]] · [[player-development-curve]]
- [[player-rating-time-series]] · [[performance-volatility]]

### Data and Representation
- [[spadl]] · [[event-stream-data]] · [[optical-tracking-data]]
- [[permutation-problem]] — 22 players have no canonical order; four routes past it, and what each costs
- [[koopman-mode-decomposition]] — comparing plays by their generating dynamics rather than their trajectories
- [[social-network-analysis]] — pass networks as weighted directed graphs; player-level centrality
- [[network-cohesion]] — the team-level half: clustering, edge connectivity, cliques, and a reported community-detection failure

### Computer Vision
- [[game-state-reconstruction]] · [[multi-object-tracking]] · [[object-detection]] · [[camera-calibration]]
- [[homography]] · [[radial-distortion]] · [[image-alignment]] · [[enhanced-correlation-coefficient]] · [[jac-metric]]
- [[semantic-segmentation]] · [[fully-convolutional-network]] · [[feature-pyramid-network]] · [[optical-flow]] · [[siamese-network]]

## Syntheses

- [[action-valuation-frameworks-compared]] — **Football Modelling Tasks Compared**: the seven distinct tasks, how each is validated, why possession metrics outpredict goals, and the two findings that cut against the field — where metrics have been compared they disagree, and where the forward approach has been attempted it failed

## Questions

Eight open investigations, grouped by who could answer them.

### Component-level benchmarking gaps
- [[pitch-control-traditions-compared]] — **Do the two pitch-control traditions agree?** Neither cites the other.
- [[shot-value-formulations-compared]] — **Are the four shot-value formulations interchangeable?**
- [[tracking-error-propagation]] — **Does tracking error propagate into value estimates?**

### Untested assumptions in held work
- [[free-parameters-load-bearing]] — **Are the free parameters load-bearing?** Sixteen asserted, across six kinds.
- [[vaep-conceding-classifier]] — **Is VAEP's conceding classifier broken, or just unthresholdable?**

### Claims this vault generated
- [[within-season-variation-noise-or-signal]] — **Is within-season variation noise or signal?**
- [[observed-versus-optimal-decisions]] — **Do players decide suboptimally, or do the models only think so?**
- [[handcrafted-features-rule]] — **Is the handcrafted-features rule right?** Three candidate tests, all uninformative.

## Source Summaries

### Action valuation and possession
- [[evaluating-football-player-actions]] — "Actions Speak Louder than Goals" (Decroos et al., 2019) — VAEP
- [[on-ball-actions-football-xt-vs-vaep]] — "Valuing On-the-Ball Actions in Soccer: xT vs VAEP" (Van Roy et al., 2020)
- [[expected-value-possession-framework]] — Fernández, Bornn & Cervone (2020) — the EPV framework and SoccerMap
- [[epv-control-duel-skills-football]] — "EPV of Control and Duel Actions" (Shelopugin) — PCR
- [[multiresolution-stochastic-process-nba-possessions]] — Cervone et al. (2016) — NBA martingale EPV
- [[football-performance-time-series]] — "Valuing Players Over Time" (Mendes-Neves et al.)

### Pitch control and off-ball
- [[physics-based-pass-probabilities]] — Spearman et al. (MIT Sloan 2017) — the intercept/control model and PPCF
- [[beyond-expected-goals]] — Spearman (MIT Sloan 2018) — OBSO
- [[wide-open-spaces-space-creation]] — Fernández & Bornn (MIT Sloan 2018) — SOG and SGG
- [[creating-scoring-opportunities-trajectory-prediction]] — Teranishi et al. (MLSA 2022/23) — C-OBSO

### Defensive valuation
- [[football-defence-evaluation-vdep]] — Toda et al. (PLOS ONE 2022) — VDEP
- [[generalized-vdep-euro-location-analysis]] — Umemoto, Tsutsui & Fujii (2022) — GVDEP
- [[team-defense-positioning-counterfactuals]] — Umemoto & Fujii (StatsBomb 2023) — EF-OBSO and DRSO

### Reinforcement learning and simulation
- [[ai-football-reinforcement-learning]] — "How does AI play football?" (Scott, Fujii & Onishi, ICAART 2022) — the vault's only positive simulator-transfer result
- [[action-valuation-multi-agent-reinforcement-learning]] — Nakahara et al. (2023) — per-player SARSA agents and the first off-ball metric comparison
- [[adaptive-action-supervision-multi-agent-rl]] — Fujii et al. (2023) — Real-to-Sim, DTW supervision, NFootball
- [[optimal-decisions-shot-taking-situations]] — Yeung & Fujii (2024) — game theory, xSOT

### Event modelling and generation
- [[transformer-point-process-football-event-modelling]] — Yeung et al. (2023) — NMSTPP
- [[understanding-football-possessions-path-signatures]] — Hirnschall & Bajons (2025) — Sig-Model
- [[football-event-sequences-point-process-mixture]] — Amezouwui et al. (2025)
- [[eventgpt-player-impact-team-action-sequences]] — Lee, Hong et al. (2025)
- [[scoutgpt-counterfactual-player-valuation]] — Hong et al. (2026)

### Field surveys
- [[data-driven-team-sports-behaviors]] — Fujii (2021), sole-authored survey — the group's own map of the field, written before eight of the nine Fujii-group sources held here

### Tactical description and pass networks
- [[network-theory-football-strategies]] — "A network theory analysis of football strategies" (López Peña & Touchette, 2012) — the tactical task's only primary source

### Computer vision for sport
- [[soccernet-game-state-reconstruction]] — Somers et al. (2024)
- [[soccernet-game-state-reconstruction-improvement]] — Golovkin et al. (2024)
- [[soccernet-v2-action-spotting]] — Cioppa et al. (2021)
- [[camera-calibration-benchmarking]] — ProCC (Magera et al., 2025)
- [[tvcalib-camera-calibration-football]] — Theiner & Ewerth (2023)
- [[sports-camera-calibration-synthetic-data]] — Chen & Little (2019)
- [[detection-tracking-football-broadcast-footage]] — Tshiani (2025)
- [[computer-vision-football-review]] — Zheng et al. (2025)
- [[amateur-football-analytics-computer-vision]] — Mavrogiannis (2021)

### Retained general sources — held because a football page depends on them
- [[attention-is-all-you-need]] — Vaswani et al. (2017); NMSTPP's encoder
- [[neural-machine-translation]] — Bahdanau et al. (2015); the GRU and attention
- [[variational-lossy-autoencoders]] — Chen et al. (2017); the VAE behind C-OBSO's GVRNN
- [[context-aggregation-dilated-convolutions]] — Yu & Koltun (2016); the road SoccerMap did not take
- [[bayesian-true-skill-rating]] — Herbrich et al. (2006); ranking RL agents
- [[training-lm-follow-instructions-with-human-feedback]] — Ouyang et al. (2022); PPO, and the coefficient the football papers do not report
- [[rnn-regularisation]] — Zaremba et al. (2015); small-data overfitting
- [[pointer-networks]] — Vinyals et al. (2015); pass selection as a pointer problem
- [[sequence-to-sequence-sets]] — Vinyals et al. (2016); 22 players have no canonical order
