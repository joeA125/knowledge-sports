# Year-to-Year Metric Stability in Elite Football: A Positional Analysis Across the Big Five European Leagues

Mohammad Arshan Shaikh

Khoury College of Computer Science, Northeastern University, Miami, FL, USA

shaikh.mohammada@northeastern.edu ORCID: 0009-0004-6962-3633

*This manuscript has been submitted to the Journal of Sports Analytics (IOS Press).*

This preprint is made available under a CC-BY 4.0 licence.

## Abstract

Understanding which player performance metrics remain consistent from one season to the next is fundamental to football analytics, yet position-specific evidence across multiple leagues and a long time horizon remains scarce. We construct a panel of 8,207 player-season pairs from all five major European leagues across seven consecutive season pairs (2017/18–2024/25), covering 2,866 unique players. For each of three position groups (forwards, midfielders, defenders), we compute Pearson correlations and intraclass correlation coefficients (ICC) across 24 position-metric combinations, and extend the analysis to age-group comparisons via Fisher $z$-tests, league-level variation, two-year stability decay, and sensitivity analyses. Pass completion percentage (the proportion of attempted passes that reach a teammate) emerges as the most stable metric overall ($r = 0.824$), while ball-progression statistics form a reliable second tier for forwards. Defensive volume counts display low stability ($r < 0.50$). Veterans are systematically more stable than under-23 players in 18 of 24 position-metric combinations ($p < 0.05$). All 24 combinations exhibit stability decay at the two-year horizon. Results are robust to the playing-time threshold ($\Delta r = 0.006$) and to the exclusion of position-changers (23/24 combinations).

**Keywords:** football analytics; metric stability; year-to-year consistency; pass completion; player performance; Big Five leagues; intraclass correlation coefficient; Pearson correlation

# 1 Introduction

A recurring assumption in football analytics is that player performance metrics measured in one season can inform predictions about the next. Transfer-fee models, scouting algorithms, and performance-projection frameworks all implicitly or explicitly rely on some degree of metric stability over time. Yet the extent to which individual metrics actually persist from one season to the next, and how that persistence varies by playing position, player age, and league, has received surprisingly little systematic attention in the published literature.

This matters for at least two reasons. First, recruitment decisions are made on the basis of historical statistics; if a metric fluctuates substantially year to year, its predictive value for subsequent seasons is limited regardless of its face validity as a performance indicator. Second, analysts and practitioners need to know not just what a player has done, but how reliably that history predicts future performance, a question that depends directly on metric stability.

Metric stability is most naturally operationalised as the year-to-year correlation of a statistic across players. A high correlation implies that players who score highly on a metric in season $t$ tend to do so again in season $t + 1$, indicating that the metric reflects a stable underlying trait. A low correlation suggests that season-to-season variation is large relative to between-player differences, making the metric unreliable as a predictive signal.

Existing work has addressed related questions. Pappalardo et al. (2019) examined individual performance consistency using composite performance indices across European leagues, establishing that multi-metric representations provide more stable player profiles than single-metric approaches. Franks et al. (2016) investigated the repeatability of various metrics in ice hockey, introducing the concept of meta-analytics to evaluate the statistical properties of sports metrics; their decomposition of variance into signal and noise components provides the conceptual foundation adopted here. The year-to-year reliability analysis tradition in baseball (Albert and Bennett, 2003) further informed the methodological choices used in this study. More recently, Decroos et al. (2019) introduced VAEP (Valuing Actions by Estimating Probabilities), emphasising that player contribution is tightly linked to tactical role and game state, supporting the position-specific modelling approach adopted here.

In football specifically, cross-context performance adjustment has been studied through league translation frameworks (Glazer, 2026), which estimate how player statistics change when moving between competitive environments. Muller et al. (2017) demonstrated that market value estimation benefits from stable, context-adjusted metrics, motivating an explicit understanding of which metrics are most reliable across seasons. Van Roy et al.

(2020) applied deep learning to event-level data for player rating, illustrating the breadth of methodology now applied to cross-context performance evaluation. Spearman (2018) demonstrated that structural league differences in tactics, possession style, and defensive pressure create systematic differences in how process metrics accumulate, motivating position-specific and context-specific modelling of metric stability. Brooks et al. (2016) examined the reliability of baseball pitch-type metrics, providing a methodological parallel for the stability analysis conducted here.

However, while prior work has examined the reliability of individual sports metrics and tracking-derived measures, a comprehensive position-specific stability ranking across multiple seasons and leagues has not previously been reported for elite football. No study has provided a full positional breakdown across all Big Five European leagues over an eight-season window, with formal age-group comparisons and multi-year decay estimation.

The present paper addresses this gap by providing a practical framework for identifying which football metrics yield reliable signals for player evaluation. Our specific contributions are:

1. A panel of 8,207 player-season pairs across five leagues and seven consecutive season pairs (2017/18–2024/25), enabling position-specific stability estimates with sufficient statistical power.

2. Stability estimates for 24 position-metric combinations (10 forward, 8 midfielder, 6 defender metrics), with Pearson *r* and ICC reported alongside 95% confidence intervals, forming a reference guide for practitioners selecting metrics for scouting or projection models.

3. Fisher *z*-test comparisons between age groups, showing that veterans are systematically more stable across the majority of metrics, with direct implications for how uncertainty should be scaled when projecting younger players.

4. A two-year stability decay analysis, demonstrating that $t \rightarrow t + 2$ correlations are uniformly lower than $t \rightarrow t + 1$ correlations, informing how analysts should weight multi-season histories.

5. Robustness checks covering playing-time threshold sensitivity and position-change contamination, validating that findings are not artefacts of sample construction choices.

Collectively, these results provide actionable guidance for performance analysts, recruitment departments, and modellers deciding which metrics to prioritise and how much confidence to place in historical statistics of different types.

# 2 Methods

## 2.1 Data Source and Season Coverage

Data were obtained from two publicly available FBref-derived Kaggle datasets covering the English Premier League, Spanish La Liga, German Bundesliga, Italian Serie A, and French Ligue 1. A historical dataset covering 2017/18–2023/24 (Krithick, 2024) was combined with a separate 2024/25 dataset (Sidorowicz, 2025) that uses different column naming conventions; a standardisation layer reconciled column names and units across both formats. Full pipeline details are available at the code repository listed in the Data Availability statement. Eight seasons in total were used, generating seven consecutive season pairs.

Player names were Unicode-normalised and stripped of diacritics before lower-casing and concatenating tokens to form a unique `Player_Key`, enabling reliable within-player matching across seasons and data formats.

## 2.2 Position Groups and Metric Selection

Each player was assigned to one of three position buckets (FWD: forward; MID: midfielder; DEF: defender) based on the primary component of their reported position code. Players whose primary position code contained ‘D’ were classified as defenders; those with ‘M’ as midfielders; those with ‘F’ as forwards. Players coded as ‘OTHER’ were excluded.

All metrics are expressed as season totals unless otherwise stated, reflecting total production over the season. The per-90 rate for any counting metric (e.g., goals per 90 minutes) is simply the season total divided by the player’s total minutes played divided by 90; we work with season totals to assess total production predictability, which is the quantity relevant for recruitment. The metrics analysed and their definitions are as follows:

* **Key Passes (KP):** passes that directly create a shot attempt for a teammate, counting the final pass before the shot.

* **Progressive Passes (PrgP):** completed passes that move the ball at least 10 yards closer to the opponent’s goal, excluding passes in the defensive third of the pitch.

* **Progressive Carries (PrgC):** dribbles or ball carries that move the ball at least 5 yards closer to the opponent’s goal, excluding carries that begin in the defensive third.

* **Shot-Creating Actions (SCA):** the two offensive actions (passes, dribbles, or drawn fouls) immediately preceding a shot attempt by any teammate. A player receives credit for each of the two actions that directly created the shot.

*   **Goals**: the total number of goals scored from open play, set pieces, and penalties.

*   **Assists**: passes or actions that directly lead to a teammate scoring a goal.

*   **Expected Goals (xG)**: a probabilistic estimate of the number of goals a player would be expected to score given the quality of shot attempts, based on historical shot conversion rates from comparable positions and situations (Rathke, 2017).

*   **Non-Penalty Expected Goals (npxG)**: expected goals with penalty kick attempts excluded, isolating open-play and set-piece shooting quality.

*   **Goal-Creating Actions (GCA)**: the two offensive actions immediately preceding a goal scored by any teammate; analogous to SCA but requiring the shot to result in a goal.

*   **Passes into the Penalty Area (PPA)**: completed passes that enter the opponent’s 18-yard box, capturing the frequency with which a player delivers the ball into dangerous areas.

*   **Pass Completion Percentage (PassPct)**: the proportion of attempted passes that are completed (i.e., reach a teammate), expressed as a percentage. This rate metric captures passing style, technical execution, and tactical role simultaneously.

*   **Progressive Passing Distance (PrgDist)**: the total distance in yards covered by completed progressive passes in a season, combining the frequency and average length of progressive passing actions.

*   **Tackles Won (TklW)**: the number of tackle attempts in which the player wins the ball outright from an opponent, requiring physical contact and possession gain.

*   **Interceptions (Int)**: the number of times a player intercepts a pass intended for an opponent, cutting out the ball before it reaches its target.

*   **Clearances (Clr)**: the number of times a player clears the ball away from their own penalty area or defensive zone when under immediate threat.

*   **Blocks**: the number of times a player blocks a shot or pass by getting their body in the trajectory of the ball.

Metrics were assigned to position groups as follows:

*   **FWD (10 metrics)**: KP, PrgP, PrgC, SCA, Goals, Assists, xG, npxG, GCA, PPA.

*   **MID (8 metrics)**: KP, PrgP, PrgC, SCA, GCA, PPA, PassPct, PrgDist.

*   **DEF (6 metrics)**: TklW, Int, Clr, Blocks, PrgP, PassPct.

Several metrics appear in more than one position group (e.g., PrgP spans all three groups; KP, PPA, SCA, and GCA appear in both FWD and MID); where this occurs, stability is estimated separately for each positional context, since the distributions and sample compositions differ substantially.

Volume metrics (Goals, Assists, TklW, etc.) and passing-volume metrics (KP, PrgP, PrgC, etc.) were measured as raw season totals. PassPct, stored as a per-90 rate in the historical dataset, was converted to a seasonal total by multiplying by 90-minute equivalents (90s) prior to analysis. SCA and GCA were similarly converted. All season totals thus represent the player’s aggregate production across their playing time in that season.

## 2.3 Sample Construction

For each consecutive season pair $(t, t + 1)$, we merged the two season files on `Player_Key` and retained only players appearing in the same league in both seasons (within-league stability). Where a player appeared for multiple clubs in the same season, we retained the row with the highest number of 90-minute equivalents (90s), reflecting their primary stint.

A minimum playing-time threshold of MIN_90S = 5.0 (that is, at least $5 \times 90 = 450$ minutes played in each of the two seasons) was applied in both seasons of each pair, removing part-season participants whose statistics are subject to excessive sampling noise. The sensitivity of results to this threshold is examined explicitly in Section 3.6. Table 1 summarises the resulting panel.

Table 1: Sample summary. Seven consecutive season pairs drawn from the Big Five European leagues, 2017/18–2024/25, after applying the MIN_90S = 5.0 filter.

<table>
  <thead>
    <tr>
        <th>Dimension</th>
        <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>Season pairs</td>
        <td>7</td>
    </tr>
    <tr>
        <td>Leagues</td>
        <td>5 (PL, La Liga, Bundesliga, Serie A, Ligue 1)</td>
    </tr>
    <tr>
        <td>Player-season pairs (filtered)</td>
        <td>8,207</td>
    </tr>
    <tr>
        <td>Unique players</td>
        <td>2,866</td>
    </tr>
    <tr>
        <td colspan="2"><em>Position breakdown</em></td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>1,925 (23.5%)</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>2,815 (34.3%)</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>3,467 (42.2%)</td>
    </tr>
    <tr>
        <td colspan="2"><em>Age group breakdown</em></td>
    </tr>
    <tr>
        <td>Young (U23)</td>
        <td>2,272 (25.3%)</td>
    </tr>
    <tr>
        <td>Prime (23–29)</td>
        <td>5,210 (57.9%)</td>
    </tr>
    <tr>
        <td>Veteran (30+)</td>
        <td>1,511 (16.8%)</td>
    </tr>
  </tbody>
</table>

# 2.4 Stability Estimation

## 2.4.1 Primary Statistic: Pearson $r$

For each position-metric combination, we computed the Pearson correlation between each player’s metric value in season $t$ and season $t + 1$, pooling all seven season pairs. Pearson $r$ is computed on raw season totals, capturing both the rate and the playing-time components of a metric; this is appropriate for recruitment-facing analyses where the question is whether a player’s total production is predictable, not just their rate per 90 minutes.

Ninety-five percent confidence intervals were constructed via Fisher $z$-transformation:

$$ z = \tanh^{-1}(r), \quad \text{SE}(z) = \frac{1}{\sqrt{n - 3}}, \quad \text{CI} = [\tanh(z \pm z_{0.975} \cdot \text{SE}(z))]. \eqno(1) $$

## 2.4.2 Secondary Statistic: ICC

To provide a complementary estimate that is not inflated by between-player playing-time variance, we also computed the two-way random-effects intraclass correlation coefficient (ICC) under an absolute-agreement model (Shrout and Fleiss, 1979), following Case 2 of Shrout and Fleiss. The ICC quantifies the proportion of total variance that is attributable to stable between-player differences, rather than within-player year-to-year fluctuation or measurement noise:

$$ \text{ICC}(2, 1) = \frac{MSR - MSE}{MSR + (k - 1)MSE + \frac{k}{n}(MSC - MSE)}, \eqno(2) $$

where $k = 2$ (seasons $t$ and $t + 1$), and $MSR$, $MSC$, $MSE$ are the row, column, and error mean squares from a two-way ANOVA respectively. ICC was computed on per-90 rates rather than raw totals, explicitly removing the playing-time scale that inflates raw-count ICCs. PassPct is already a rate and was used directly.

Stability labels were assigned on the basis of Pearson $r$ (the primary metric): Very High ($r \geq 0.80$), High ($r \geq 0.70$), Moderate ($0.50 \leq r < 0.70$), and Low ($r < 0.50$), following conventions commonly applied in sports science reliability research (Koo and Li, 2016).

# 2.5 Extension Analyses

## 2.5.1 Age Group Comparisons (Fisher $z$-Tests)

Players were stratified into three age groups based on age at the start of season $t$: Young (U23: $< 23$ years), Prime (23–29), and Veteran (30+). Pearson $r$ was computed separately for each age group within each position-metric combination. Pairwise differences between

age groups were formally tested using the two-sample Fisher $z$-test:

$$ Z_{\text{test}} = \frac{\tanh^{-1}(r_1) - \tanh^{-1}(r_2)}{\sqrt{\frac{1}{n_1-3} + \frac{1}{n_2-3}}}, \tag{3} $$

with significance assessed at the two-tailed $\alpha = 0.05$ level.

## 2.5.2 League-Level Stability

Pearson $r$ was computed separately for each of the five leagues within each position-metric combination, yielding a metrics $\times$ leagues stability matrix per position group. No formal cross-league significance tests were conducted given the multiple-comparison burden; league stability matrices are presented descriptively.

## 2.5.3 Two-Year Stability Decay ($t \rightarrow t + 2$)

To assess whether stability degrades over a two-season horizon, $t \rightarrow t + 2$ pairs were constructed by chaining consecutive season pairs: for each triplet ($s_t, s_{\text{mid}}, s_{t+2}$), a player must appear in the same league in all three seasons. This yielded 4,844 $t \rightarrow t + 2$ observations (1,886 unique players across six triplets). Pearson $r(t \rightarrow t + 2)$ was compared to $r(t \rightarrow t + 1)$ for each position-metric combination; a decay criterion of $\Delta r < -0.03$ was used.

## 2.5.4 Sensitivity to Playing-Time Threshold

MIN_90S was swept from 3.0 to 7.0 in increments of 0.5. Mean Pearson $r$ across all 24 position-metric combinations was recomputed at each threshold, and the range $\Delta r_{\text{sweep}}$ quantified robustness to the threshold choice.

## 2.5.5 Role-Change Sensitivity

Players who changed position bucket between season $t$ and season $t + 1$ (682/8,207; 8.3%) were flagged. Stability was recomputed on the same-role-only subset and compared to full-sample estimates; $|\Delta r| < 0.02$ was the criterion for robustness.

# 3 Results

## 3.1 Overall Stability: Primary Findings

Table 2 presents Pearson $r$ and ICC for all 24 position-metric combinations, ranked by stability within each position group. Figure 1 displays the same data as a colour-coded heatmap.

Table 2: Year-to-year metric stability for all position-metric combinations ($n$ = player-season pairs; Pearson $r$ on raw counts; ICC on per-90 rates; 95% CI via Fisher $z$-transformation; MIN_90S = 5.0; seven season pairs, 2017/18–2024/25). Stability labels: Very High $r \geq 0.80$; High $r \geq 0.70$; Moderate $0.50 \leq r < 0.70$; Low $r < 0.50$.

<table>
  <thead>
    <tr>
        <th>Position</th>
        <th>Metric</th>
        <th>n</th>
        <th>Pearson r</th>
        <th>95% CI</th>
        <th>ICC</th>
        <th>Stability</th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td colspan="7"><em>Forwards</em></td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>PrgC</td>
        <td>1,925</td>
        <td>0.733</td>
        <td>[0.712, 0.753]</td>
        <td>0.731</td>
        <td>High</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>PrgP</td>
        <td>1,925</td>
        <td>0.709</td>
        <td>[0.686, 0.731]</td>
        <td>0.709</td>
        <td>High</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>PPA</td>
        <td>1,925</td>
        <td>0.707</td>
        <td>[0.684, 0.729]</td>
        <td>0.706</td>
        <td>High</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>xG</td>
        <td>1,925</td>
        <td>0.664</td>
        <td>[0.638, 0.688]</td>
        <td>0.663</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>npxG</td>
        <td>1,925</td>
        <td>0.635</td>
        <td>[0.608, 0.661]</td>
        <td>0.633</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>SCA</td>
        <td>1,925</td>
        <td>0.626</td>
        <td>[0.598, 0.652]</td>
        <td>0.624</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>KP</td>
        <td>1,925</td>
        <td>0.620</td>
        <td>[0.591, 0.647]</td>
        <td>0.619</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>Goals</td>
        <td>1,925</td>
        <td>0.593</td>
        <td>[0.563, 0.621]</td>
        <td>0.591</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>GCA</td>
        <td>1,925</td>
        <td>0.528</td>
        <td>[0.495, 0.559]</td>
        <td>0.526</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>Assists</td>
        <td>1,925</td>
        <td>0.428</td>
        <td>[0.391, 0.464]</td>
        <td>0.428</td>
        <td>Low</td>
    </tr>
    <tr>
        <td colspan="7"><em>Midfielders</em></td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PassPct</td>
        <td>2,815</td>
        <td>0.824</td>
        <td>[0.812, 0.836]</td>
        <td>0.823</td>
        <td>Very High</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PrgDist</td>
        <td>2,815</td>
        <td>0.676</td>
        <td>[0.656, 0.696]</td>
        <td>0.676</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PPA</td>
        <td>2,815</td>
        <td>0.675</td>
        <td>[0.655, 0.695]</td>
        <td>0.675</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PrgC</td>
        <td>2,815</td>
        <td>0.672</td>
        <td>[0.651, 0.691]</td>
        <td>0.668</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>KP</td>
        <td>2,815</td>
        <td>0.655</td>
        <td>[0.634, 0.676]</td>
        <td>0.655</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PrgP</td>
        <td>2,815</td>
        <td>0.646</td>
        <td>[0.624, 0.667]</td>
        <td>0.644</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>SCA</td>
        <td>2,815</td>
        <td>0.635</td>
        <td>[0.613, 0.657]</td>
        <td>0.634</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>GCA</td>
        <td>2,815</td>
        <td>0.524</td>
        <td>[0.496, 0.550]</td>
        <td>0.523</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td colspan="7"><em>Defenders</em></td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>PassPct</td>
        <td>3,467</td>
        <td>0.824</td>
        <td>[0.813, 0.834]</td>
        <td>0.822</td>
        <td>Very High</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>Clr</td>
        <td>3,467</td>
        <td>0.588</td>
        <td>[0.566, 0.609]</td>
        <td>0.585</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>PrgP</td>
        <td>3,467</td>
        <td>0.529</td>
        <td>[0.504, 0.552]</td>
        <td>0.528</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>Int</td>
        <td>3,467</td>
        <td>0.476</td>
        <td>[0.450, 0.502]</td>
        <td>0.470</td>
        <td>Low</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>TklW</td>
        <td>3,467</td>
        <td>0.414</td>
        <td>[0.386, 0.441]</td>
        <td>0.409</td>
        <td>Low</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>Blocks</td>
        <td>3,467</td>
        <td>0.399</td>
        <td>[0.371, 0.427]</td>
        <td>0.398</td>
        <td>Low</td>
    </tr>
  </tbody>
</table>

The dominant finding is the uniformly high stability of pass completion percentage (PassPct), which achieves $r = 0.824$ for both midfielders and defenders, with ICC values of 0.823 and 0.822 respectively. The near-perfect agreement between the Pearson and ICC estimates confirms that this result does not depend on playing-time variance. PassPct reflects a stylistic trait that is largely determined by a player’s positional role, passing range preferences, and technical profile, all of which change slowly over a career.

Ball-progression statistics form a reliable second tier among forwards. Progressive carries (PrgC: $r = 0.733$), progressive passes (PrgP: $r = 0.709$), and passes into the penalty area (PPA: $r = 0.707$) all cross the High stability threshold. These statistics capture habitual movement and passing behaviour that reflects positional roles rather than purely luck-driven outcomes. The same metrics show only Moderate stability for midfielders (PrgC: $r = 0.672$; PrgP: $r = 0.646$; PPA: $r = 0.675$), suggesting that the forward role concentrates these behaviours in a more player-specific way than the midfielder role, which spans a wider range of tactical functions.

Expected goals (xG: $r = 0.664$) and non-penalty xG (npxG: $r = 0.635$) exhibit Moderate stability for forwards, higher than raw goal counts (Goals: $r = 0.593$) and substantially higher than assists ($r = 0.428$), the only Low-stability metric among forwards. This ordering is consistent with the view that xG strips some of the finishing-luck component from goal counts (Rathke, 2017; Caley, 2015), making it a more reliable forecasting input. Assists are more dependent on teammate quality, tactical scheme, and the decisions of other players at the moment of delivery, factors that are not attributable to the assisting player alone.

Defensive volume statistics exhibit the lowest stability in the dataset. TklW ($r = 0.414$), Blocks ($r = 0.399$), and Int ($r = 0.476$) all fall in the Low category, while Clr ($r = 0.588$) and PrgP ($r = 0.529$) reach Moderate status. This pattern likely reflects the dual sensitivity of defensive volume counts to both individual positioning and team-level defensive structure: a defender who changes clubs or whose team alters its pressing shape will exhibit discontinuous changes in raw defensive action counts even if their individual effort and positional diligence remain constant (Spearman, 2018).

## 3.2 Overlapping Metrics Across Position Groups

Seven metrics appear in more than one position group: GCA, KP, PPA, PassPct, PrgC, PrgP, and SCA. Figure 3 compares their stability across positions. PassPct is uniformly the most stable metric in every context it appears. PrgP is the only metric appearing across all three groups; it is most stable among forwards (High) and comparably Moderate for midfielders and defenders. KP, SCA, GCA, and PPA show consistently higher stability for forwards than midfielders, consistent with the positional-concentration argument:

Figure 1: Year-to-Year Metric Stability by Position Group
(Pearson r, season t vs t+1)

<table>
  <thead>
    <tr>
        <th colspan="3">FWD (n=1925)</th>
        <th colspan="3">MID (n=2815)</th>
        <th colspan="3">DEF (n=3467)</th>
    </tr>
    <tr>
        <th>Metric</th>
        <th>Pearson r</th>
        <th>Stability</th>
        <th>Metric</th>
        <th>Pearson r</th>
        <th>Stability</th>
        <th>Metric</th>
        <th>Pearson r</th>
        <th>Stability</th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>PrgC</td>
        <td>0.733</td>
        <td>High</td>
        <td>PassPct</td>
        <td>0.824</td>
        <td>High</td>
        <td>PassPct</td>
        <td>0.824</td>
        <td>High</td>
    </tr>
    <tr>
        <td>PrgP</td>
        <td>0.709</td>
        <td>High</td>
        <td>PrgDist</td>
        <td>0.676</td>
        <td>Moderate</td>
        <td>Clr</td>
        <td>0.588</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>PPA</td>
        <td>0.707</td>
        <td>High</td>
        <td>PPA</td>
        <td>0.675</td>
        <td>Moderate</td>
        <td>PrgP</td>
        <td>0.529</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>xG</td>
        <td>0.664</td>
        <td>Moderate</td>
        <td>PrgC</td>
        <td>0.672</td>
        <td>Moderate</td>
        <td>Int</td>
        <td>0.476</td>
        <td>Low</td>
    </tr>
    <tr>
        <td>npxG</td>
        <td>0.635</td>
        <td>Moderate</td>
        <td>KP</td>
        <td>0.655</td>
        <td>Moderate</td>
        <td>TklW</td>
        <td>0.414</td>
        <td>Low</td>
    </tr>
    <tr>
        <td>SCA</td>
        <td>0.626</td>
        <td>Moderate</td>
        <td>PrgP</td>
        <td>0.646</td>
        <td>Moderate</td>
        <td>Blocks</td>
        <td>0.399</td>
        <td>Low</td>
    </tr>
    <tr>
        <td>KP</td>
        <td>0.620</td>
        <td>Moderate</td>
        <td>SCA</td>
        <td>0.635</td>
        <td>Moderate</td>
        <td> </td>
        <td> </td>
        <td> </td>
    </tr>
    <tr>
        <td>Goals</td>
        <td>0.593</td>
        <td>Moderate</td>
        <td>GCA</td>
        <td>0.524</td>
        <td>Moderate</td>
        <td> </td>
        <td> </td>
        <td> </td>
    </tr>
    <tr>
        <td>GCA</td>
        <td>0.528</td>
        <td>Moderate</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
    </tr>
    <tr>
        <td>Assists</td>
        <td>0.428</td>
        <td>Low</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
    </tr>
  </tbody>
</table>

Figure 1: Year-to-year metric stability heatmap by position group. Colour scale: red = low stability ($r \approx 0$), green = high stability ($r \approx 1$). Stability labels (Very High/High/Moderate/Low) annotated to the right of each row. Sample sizes: FWD $n = 1,925$; MID $n = 2,815$; DEF $n = 3,467$.

the forward role imposes tighter behavioural constraints than the midfielder role, which encompasses a wider range of tactical functions and positional sub-types.

## 3.3 Stability by Age Group

Table 3 presents Pearson $r$ by age group for each position-metric combination. Figure 4 traces the age trajectory for each metric within each position group.

The dominant pattern is monotonic: stability is lowest for Young (U23) players and highest for Veterans (30+) in the large majority of cases. The formal Fisher $z$-tests confirm this: 18 of 24 Young vs Veteran comparisons are statistically significant at $p < 0.05$, with 15 of those reaching $p < 0.001$ (Table 4).

The age effect is strongest for attacking and midfield metrics. For forwards, nine of ten Young vs Veteran comparisons are significant; PrgP ($z = -6.19$, $p < 0.001$), PPA ($z = -5.91$, $p < 0.001$), and SCA ($z = -4.48$, $p < 0.001$) show the largest differentials. For midfielders, all eight comparisons are significant, with PassPct ($z = -6.35$, $p < 0.001$) and KP ($z = -5.78$, $p < 0.001$) leading.

Defenders are the notable exception: only PassPct ($z = -2.74$, $p < 0.01$) reaches significance among the six defensive metrics. The uniformly low stability of defensive volume statistics (TklW, Blocks, Int) appears to overwhelm any age-related signal, with Clr and PrgP also showing non-significant trends. This suggests that for defenders, team-context and tactical factors that drive defensive action counts dominate individual-level continuity regardless of career stage.

# Figure 2A: FWD — Season t vs t+1 (ordered by stability)

<table>
  <thead>
    <tr>
        <th>Metric</th>
        <th>r-value</th>
        <th>Stability Label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>PrgC</td>
        <td>0.733</td>
        <td>High</td>
    </tr>
    <tr>
        <td>PrgP</td>
        <td>0.709</td>
        <td>High</td>
    </tr>
    <tr>
        <td>PPA</td>
        <td>0.707</td>
        <td>High</td>
    </tr>
    <tr>
        <td>xG</td>
        <td>0.664</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>npxG</td>
        <td>0.635</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>SCA</td>
        <td>0.626</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>KP</td>
        <td>0.620</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>Goals</td>
        <td>0.593</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>GCA</td>
        <td>0.528</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>Assists</td>
        <td>0.428</td>
        <td>Low</td>
    </tr>
  </tbody>
</table>

(a) Forwards

# Figure 2B: MID — Season t vs t+1 (ordered by stability)

<table>
  <thead>
    <tr>
        <th>Metric</th>
        <th>r-value</th>
        <th>Stability Label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>PassPct</td>
        <td>0.824</td>
        <td>High</td>
    </tr>
    <tr>
        <td>PrgDist</td>
        <td>0.676</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>PPA</td>
        <td>0.675</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>PrgC</td>
        <td>0.672</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>KP</td>
        <td>0.655</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>PrgP</td>
        <td>0.646</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>SCA</td>
        <td>0.635</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>GCA</td>
        <td>0.524</td>
        <td>Moderate</td>
    </tr>
  </tbody>
</table>

(b) Midfielders

# Figure 2C: DEF - Season t vs t+1 (ordered by stability)

<table>
  <thead>
    <tr>
        <th>Metric</th>
        <th>r-value</th>
        <th>Stability Label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>PassPct</td>
        <td>0.824</td>
        <td>High</td>
    </tr>
    <tr>
        <td>Clr</td>
        <td>0.588</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>PrgP</td>
        <td>0.529</td>
        <td>Moderate</td>
    </tr>
    <tr>
        <td>Int</td>
        <td>0.476</td>
        <td>Low</td>
    </tr>
    <tr>
        <td>TklW</td>
        <td>0.414</td>
        <td>Low</td>
    </tr>
    <tr>
        <td>Blocks</td>
        <td>0.399</td>
        <td>Low</td>
    </tr>
  </tbody>
</table>

(c) Defenders

Figure 2: Scatter plots of season t vs season t + 1 metric values, ordered by stability rank within each position group. Each point represents one player-season pair. Regression line colour encodes stability label (blue = High, orange = Moderate, red = Low).

Figure 3: Stability of Overlapping Metrics Across Position Groups
(Pearson r with 95% CI)

<table>
  <thead>
    <tr>
        <th>Metric</th>
        <th>Position</th>
        <th>Pearson r</th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>GCA</td>
        <td>FWD</td>
        <td>0.528</td>
    </tr>
    <tr>
        <td>GCA</td>
        <td>MID</td>
        <td>0.524</td>
    </tr>
    <tr>
        <td>KP</td>
        <td>FWD</td>
        <td>0.620</td>
    </tr>
    <tr>
        <td>KP</td>
        <td>MID</td>
        <td>0.655</td>
    </tr>
    <tr>
        <td>PPA</td>
        <td>FWD</td>
        <td>0.707</td>
    </tr>
    <tr>
        <td>PPA</td>
        <td>MID</td>
        <td>0.675</td>
    </tr>
    <tr>
        <td>PassPct</td>
        <td>MID</td>
        <td>0.824</td>
    </tr>
    <tr>
        <td>PassPct</td>
        <td>DEF</td>
        <td>0.824</td>
    </tr>
    <tr>
        <td>PrgC</td>
        <td>FWD</td>
        <td>0.733</td>
    </tr>
    <tr>
        <td>PrgC</td>
        <td>MID</td>
        <td>0.672</td>
    </tr>
    <tr>
        <td>PrgP</td>
        <td>FWD</td>
        <td>0.709</td>
    </tr>
    <tr>
        <td>PrgP</td>
        <td>MID</td>
        <td>0.646</td>
    </tr>
    <tr>
        <td>PrgP</td>
        <td>DEF</td>
        <td>0.529</td>
    </tr>
    <tr>
        <td>SCA</td>
        <td>FWD</td>
        <td>0.626</td>
    </tr>
    <tr>
        <td>SCA</td>
        <td>MID</td>
        <td>0.635</td>
    </tr>
  </tbody>
</table>

Figure 3: Stability comparison for overlapping metrics (appearing in more than one position group). Bars show Pearson r with 95% CI error bars. Horizontal dashed lines indicate High (r = 0.70) and Moderate (r = 0.50) thresholds.

Table 3: Year-to-year stability (Pearson r) by age group. Columns: Young (U23, <23 years), Prime (23–29), Veteran (30+).

<table>
  <thead>
    <tr>
        <th>Position</th>
        <th>Metric</th>
        <th>Young (U23)</th>
        <th>Prime (23–29)</th>
        <th>Veteran (30+)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td colspan="5"><em>Forwards</em></td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>KP</td>
        <td>0.592</td>
        <td>0.602</td>
        <td>0.725</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>PrgP</td>
        <td>0.621</td>
        <td>0.688</td>
        <td>0.825</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>PrgC</td>
        <td>0.678</td>
        <td>0.738</td>
        <td>0.828</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>SCA</td>
        <td>0.590</td>
        <td>0.603</td>
        <td>0.761</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>Goals</td>
        <td>0.595</td>
        <td>0.517</td>
        <td>0.749</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>Assists</td>
        <td>0.459</td>
        <td>0.363</td>
        <td>0.550</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>xG</td>
        <td>0.643</td>
        <td>0.609</td>
        <td>0.809</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>npxG</td>
        <td>0.621</td>
        <td>0.590</td>
        <td>0.777</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>GCA</td>
        <td>0.522</td>
        <td>0.467</td>
        <td>0.692</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>PPA</td>
        <td>0.619</td>
        <td>0.698</td>
        <td>0.817</td>
    </tr>
    <tr>
        <td colspan="5"><em>Midfielders</em></td>
    </tr>
    <tr>
        <td>MID</td>
        <td>KP</td>
        <td>0.521</td>
        <td>0.676</td>
        <td>0.733</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PrgP</td>
        <td>0.581</td>
        <td>0.661</td>
        <td>0.690</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PrgC</td>
        <td>0.607</td>
        <td>0.693</td>
        <td>0.713</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>SCA</td>
        <td>0.537</td>
        <td>0.654</td>
        <td>0.717</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>GCA</td>
        <td>0.473</td>
        <td>0.529</td>
        <td>0.587</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PPA</td>
        <td>0.586</td>
        <td>0.695</td>
        <td>0.724</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PassPct</td>
        <td>0.763</td>
        <td>0.834</td>
        <td>0.884</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PrgDist</td>
        <td>0.600</td>
        <td>0.685</td>
        <td>0.720</td>
    </tr>
    <tr>
        <td colspan="5"><em>Defenders</em></td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>TklW</td>
        <td>0.399</td>
        <td>0.418</td>
        <td>0.432</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>Int</td>
        <td>0.458</td>
        <td>0.485</td>
        <td>0.490</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>Clr</td>
        <td>0.553</td>
        <td>0.610</td>
        <td>0.550</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>Blocks</td>
        <td>0.406</td>
        <td>0.407</td>
        <td>0.392</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>PrgP</td>
        <td>0.541</td>
        <td>0.528</td>
        <td>0.546</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>PassPct</td>
        <td>0.791</td>
        <td>0.831</td>
        <td>0.840</td>
    </tr>
  </tbody>
</table>

Table 4: Fisher $z$-test results for Young (U23) vs Veteran (30+) stability differences.
Significance: \*$p < 0.05$, \*\*$p < 0.01$, \*\*\*$p < 0.001$, ns = not significant.

<table>
  <thead>
    <tr>
        <th>Position</th>
        <th>Metric</th>
        <th>r (Young)</th>
        <th>r (Veteran)</th>
        <th>z-stat</th>
        <th>Sig.</th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>FWD</td>
        <td>KP</td>
        <td>0.592</td>
        <td>0.725</td>
        <td>−3.31</td>
        <td>***</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>PrgP</td>
        <td>0.621</td>
        <td>0.825</td>
        <td>−6.19</td>
        <td>***</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>PrgC</td>
        <td>0.678</td>
        <td>0.828</td>
        <td>−4.95</td>
        <td>***</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>SCA</td>
        <td>0.590</td>
        <td>0.761</td>
        <td>−4.48</td>
        <td>***</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>Goals</td>
        <td>0.595</td>
        <td>0.749</td>
        <td>−3.97</td>
        <td>***</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>Assists</td>
        <td>0.459</td>
        <td>0.550</td>
        <td>−1.70</td>
        <td>ns</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>xG</td>
        <td>0.643</td>
        <td>0.809</td>
        <td>−5.02</td>
        <td>***</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>npxG</td>
        <td>0.621</td>
        <td>0.777</td>
        <td>−4.33</td>
        <td>***</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>GCA</td>
        <td>0.522</td>
        <td>0.692</td>
        <td>−3.78</td>
        <td>***</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>PPA</td>
        <td>0.619</td>
        <td>0.817</td>
        <td>−5.91</td>
        <td>***</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>KP</td>
        <td>0.521</td>
        <td>0.733</td>
        <td>−5.78</td>
        <td>***</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PrgP</td>
        <td>0.581</td>
        <td>0.690</td>
        <td>−2.97</td>
        <td>**</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PrgC</td>
        <td>0.607</td>
        <td>0.713</td>
        <td>−3.07</td>
        <td>**</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>SCA</td>
        <td>0.537</td>
        <td>0.717</td>
        <td>−4.87</td>
        <td>***</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>GCA</td>
        <td>0.473</td>
        <td>0.587</td>
        <td>−2.57</td>
        <td>*</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PPA</td>
        <td>0.586</td>
        <td>0.724</td>
        <td>−3.95</td>
        <td>***</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PassPct</td>
        <td>0.763</td>
        <td>0.884</td>
        <td>−6.35</td>
        <td>***</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PrgDist</td>
        <td>0.600</td>
        <td>0.720</td>
        <td>−3.49</td>
        <td>***</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>TklW</td>
        <td>0.399</td>
        <td>0.432</td>
        <td>−0.75</td>
        <td>ns</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>Int</td>
        <td>0.458</td>
        <td>0.490</td>
        <td>−0.77</td>
        <td>ns</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>Clr</td>
        <td>0.553</td>
        <td>0.550</td>
        <td>+0.06</td>
        <td>ns</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>Blocks</td>
        <td>0.406</td>
        <td>0.392</td>
        <td>+0.29</td>
        <td>ns</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>PrgP</td>
        <td>0.541</td>
        <td>0.546</td>
        <td>−0.14</td>
        <td>ns</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>PassPct</td>
        <td>0.791</td>
        <td>0.840</td>
        <td>−2.74</td>
        <td>**</td>
    </tr>
  </tbody>
</table>

18/24 combinations significant at $p < 0.05$; 15/24 at $p < 0.001$.

Figure 4: Year-to-Year Stability by Age Group
(Pearson r per metric, within position group)

FWD
<table>
    <tr>
        <td>Age Group</td>
        <td>PrgC</td>
        <td>PrgP</td>
        <td>PPA</td>
        <td>xG</td>
        <td>npxG</td>
        <td>SCA</td>
        <td>Goals</td>
        <td>KP</td>
        <td>GCA</td>
        <td>Assists</td>
    </tr>
    <tr>
        <td>U23</td>
        <td>0.68</td>
        <td>0.62</td>
        <td>0.62</td>
        <td>0.64</td>
        <td>0.62</td>
        <td>0.59</td>
        <td>0.60</td>
        <td>0.59</td>
        <td>0.52</td>
        <td>0.46</td>
    </tr>
    <tr>
        <td>23-29</td>
        <td>0.66</td>
        <td>0.63</td>
        <td>0.63</td>
        <td>0.64</td>
        <td>0.62</td>
        <td>0.58</td>
        <td>0.58</td>
        <td>0.58</td>
        <td>0.51</td>
        <td>0.41</td>
    </tr>
    <tr>
        <td>30+</td>
        <td>0.83</td>
        <td>0.83</td>
        <td>0.82</td>
        <td>0.81</td>
        <td>0.78</td>
        <td>0.76</td>
        <td>0.75</td>
        <td>0.73</td>
        <td>0.69</td>
        <td>0.55</td>
    </tr>
</table>

MID
<table>
    <tr>
        <td>Age Group</td>
        <td>PassPct</td>
        <td>KP</td>
        <td>PPA</td>
        <td>PrgDist</td>
        <td>SCA</td>
        <td>PrgC</td>
        <td>PrgP</td>
        <td>GCA</td>
    </tr>
    <tr>
        <td>U23</td>
        <td>0.76</td>
        <td>0.52</td>
        <td>0.59</td>
        <td>0.60</td>
        <td>0.54</td>
        <td>0.61</td>
        <td>0.58</td>
        <td>0.47</td>
    </tr>
    <tr>
        <td>23-29</td>
        <td>0.83</td>
        <td>0.64</td>
        <td>0.65</td>
        <td>0.65</td>
        <td>0.64</td>
        <td>0.65</td>
        <td>0.64</td>
        <td>0.54</td>
    </tr>
    <tr>
        <td>30+</td>
        <td>0.88</td>
        <td>0.73</td>
        <td>0.72</td>
        <td>0.72</td>
        <td>0.72</td>
        <td>0.71</td>
        <td>0.69</td>
        <td>0.59</td>
    </tr>
</table>

DEF
<table>
    <tr>
        <td>Age Group</td>
        <td>PassPct</td>
        <td>Clr</td>
        <td>PrgP</td>
        <td>Int</td>
        <td>TklW</td>
        <td>Blocks</td>
    </tr>
    <tr>
        <td>U23</td>
        <td>0.79</td>
        <td>0.55</td>
        <td>0.54</td>
        <td>0.46</td>
        <td>0.40</td>
        <td>0.41</td>
    </tr>
    <tr>
        <td>23-29</td>
        <td>0.83</td>
        <td>0.63</td>
        <td>0.54</td>
        <td>0.53</td>
        <td>0.44</td>
        <td>0.40</td>
    </tr>
    <tr>
        <td>30+</td>
        <td>0.84</td>
        <td>0.55</td>
        <td>0.55</td>
        <td>0.49</td>
        <td>0.43</td>
        <td>0.39</td>
    </tr>
</table>

--- High threshold (0.7) -- Moderate threshold (0.5)

Figure 4: Year-to-year metric stability by age group (U23, 23–29, 30+). Each line represents one metric within the position group. Horizontal dashed lines mark the High ($r = 0.70$) and Moderate ($r = 0.50$) thresholds. Monotonic increases from young to veteran are generally visible for attacking and midfield metrics; defensive volume metrics show flat or non-monotonic patterns.

## 3.4 Stability by League

Figure 5 presents the league-level stability matrices for each position group. Table 5 provides the full numerical values. The main finding is league-level consistency: stability rankings across metrics are broadly preserved across all five leagues, and the absolute magnitude of $r$ does not vary dramatically within any metric. For example, PassPct among midfielders ranges from $r = 0.772$ (Serie A) to $r = 0.861$ (La Liga), a range of 0.089, compared to a pooled estimate of 0.824. TklW for defenders ranges from $r = 0.348$ (Ligue 1) to $r = 0.462$ (Bundesliga).

Ligue 1 tends to show somewhat lower stability values on attacking metrics. The Bundesliga shows lower forward stability for Assists ($r = 0.340$) and GCA ($r = 0.433$), potentially reflecting the higher turnover rate of offensive talent in that league. La Liga tends to show higher forward stability for ball-progression metrics (PrgC: $r = 0.810$; PPA: $r = 0.782$), consistent with the possession-dominated playing style of its leading clubs, which imposes consistent spatial roles on attackers from season to season (Spearman, 2018).

Figure 5: Year-to-Year Stability by League
(Pearson r per metric, within position group)

<table>
  <thead>
    <tr>
        <th colspan="6">FWD</th>
        <th colspan="6">MID</th>
        <th colspan="6">DEF</th>
    </tr>
    <tr>
        <th>Metric</th>
        <th>PL</th>
        <th>La Liga</th>
        <th>Bund.</th>
        <th>Serie A</th>
        <th>L1</th>
        <th>Metric</th>
        <th>PL</th>
        <th>La Liga</th>
        <th>Bund.</th>
        <th>Serie A</th>
        <th>L1</th>
        <th>Metric</th>
        <th>PL</th>
        <th>La Liga</th>
        <th>Bund.</th>
        <th>Serie A</th>
        <th>L1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>PrgC</td>
        <td>0.71</td>
        <td>0.81</td>
        <td>0.70</td>
        <td>0.69</td>
        <td>0.70</td>
        <td>PassPct</td>
        <td>0.84</td>
        <td>0.86</td>
        <td>0.78</td>
        <td>0.77</td>
        <td>0.81</td>
        <td>PassPct</td>
        <td>0.85</td>
        <td>0.83</td>
        <td>0.82</td>
        <td>0.78</td>
        <td>0.82</td>
    </tr>
    <tr>
        <td>PrgP</td>
        <td>0.71</td>
        <td>0.78</td>
        <td>0.63</td>
        <td>0.70</td>
        <td>0.67</td>
        <td>PrgDist</td>
        <td>0.67</td>
        <td>0.75</td>
        <td>0.66</td>
        <td>0.70</td>
        <td>0.56</td>
        <td>Clr</td>
        <td>0.64</td>
        <td>0.55</td>
        <td>0.57</td>
        <td>0.56</td>
        <td>0.58</td>
    </tr>
    <tr>
        <td>PPA</td>
        <td>0.68</td>
        <td>0.78</td>
        <td>0.60</td>
        <td>0.71</td>
        <td>0.69</td>
        <td>PPA</td>
        <td>0.68</td>
        <td>0.64</td>
        <td>0.68</td>
        <td>0.72</td>
        <td>0.61</td>
        <td>PrgP</td>
        <td>0.55</td>
        <td>0.53</td>
        <td>0.56</td>
        <td>0.53</td>
        <td>0.46</td>
    </tr>
    <tr>
        <td>xG</td>
        <td>0.65</td>
        <td>0.67</td>
        <td>0.66</td>
        <td>0.70</td>
        <td>0.62</td>
        <td>PrgC</td>
        <td>0.68</td>
        <td>0.66</td>
        <td>0.63</td>
        <td>0.68</td>
        <td>0.67</td>
        <td>Int</td>
        <td>0.49</td>
        <td>0.48</td>
        <td>0.51</td>
        <td>0.51</td>
        <td>0.37</td>
    </tr>
    <tr>
        <td>npxG</td>
        <td>0.62</td>
        <td>0.63</td>
        <td>0.62</td>
        <td>0.67</td>
        <td>0.62</td>
        <td>KP</td>
        <td>0.68</td>
        <td>0.64</td>
        <td>0.71</td>
        <td>0.65</td>
        <td>0.56</td>
        <td>TklW</td>
        <td>0.45</td>
        <td>0.41</td>
        <td>0.46</td>
        <td>0.38</td>
        <td>0.35</td>
    </tr>
    <tr>
        <td>SCA</td>
        <td>0.61</td>
        <td>0.71</td>
        <td>0.54</td>
        <td>0.61</td>
        <td>0.60</td>
        <td>PrgP</td>
        <td>0.65</td>
        <td>0.71</td>
        <td>0.64</td>
        <td>0.66</td>
        <td>0.51</td>
        <td>Blocks</td>
        <td>0.50</td>
        <td>0.26</td>
        <td>0.41</td>
        <td>0.34</td>
        <td>0.37</td>
    </tr>
    <tr>
        <td>KP</td>
        <td>0.59</td>
        <td>0.69</td>
        <td>0.54</td>
        <td>0.61</td>
        <td>0.61</td>
        <td>SCA</td>
        <td>0.63</td>
        <td>0.63</td>
        <td>0.68</td>
        <td>0.66</td>
        <td>0.54</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
    </tr>
    <tr>
        <td>Goals</td>
        <td>0.59</td>
        <td>0.63</td>
        <td>0.56</td>
        <td>0.58</td>
        <td>0.59</td>
        <td>GCA</td>
        <td>0.46</td>
        <td>0.54</td>
        <td>0.58</td>
        <td>0.53</td>
        <td>0.49</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
    </tr>
    <tr>
        <td>GCA</td>
        <td>0.48</td>
        <td>0.63</td>
        <td>0.43</td>
        <td>0.51</td>
        <td>0.54</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
    </tr>
    <tr>
        <td>Assists</td>
        <td>0.39</td>
        <td>0.47</td>
        <td>0.34</td>
        <td>0.41</td>
        <td>0.50</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
    </tr>
  </tbody>
</table>

Figure 5: Year-to-year metric stability heatmap by league (columns: PL = Premier League, La Liga, Bund. = Bundesliga, Serie A, L1 = Ligue 1) for each position group. Colour scale: red = low, green = high. Stability rankings are broadly preserved across leagues.

## 3.5 Two-Year Stability Decay

Every one of the 24 position-metric combinations shows a decline in Pearson r from the one-year ($t \rightarrow t + 1$) to the two-year ($t \rightarrow t + 2$) horizon ($\Delta r < -0.03$ in all 24 cases; Figure 6). The median decline is $-0.097$ (IQR: $[-0.118, -0.083]$). The most stable metric at one year (PassPct: $r = 0.824$ for MID and DEF) falls to $r = 0.768$ and $r = 0.754$ respectively at the two-year horizon, indicating still High stability but meaningfully lower values. The most susceptible metrics are the defensive volume statistics: DEF PrgP

Table 5: Year-to-year metric stability (Pearson $r$) by league. Columns: PL = Premier League, La Liga, Bund. = Bundesliga, SA = Serie A, L1 = Ligue 1. Metrics ranked by overall pooled stability within each position group (as in Table 2). Seven season pairs, 2017/18–2024/25; MIN_90S = 5.0.

<table>
  <thead>
    <tr>
        <th>Pos</th>
        <th>Metric</th>
        <th>PL</th>
        <th>La Liga</th>
        <th>Bund.</th>
        <th>SA</th>
        <th>L1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td colspan="7"><em>Forwards (ranked by pooled r)</em></td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>PrgC</td>
        <td>0.712</td>
        <td>0.810</td>
        <td>0.705</td>
        <td>0.693</td>
        <td>0.703</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>PrgP</td>
        <td>0.709</td>
        <td>0.782</td>
        <td>0.629</td>
        <td>0.704</td>
        <td>0.669</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>PPA</td>
        <td>0.680</td>
        <td>0.782</td>
        <td>0.604</td>
        <td>0.707</td>
        <td>0.694</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>xG</td>
        <td>0.650</td>
        <td>0.673</td>
        <td>0.659</td>
        <td>0.702</td>
        <td>0.624</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>npxG</td>
        <td>0.617</td>
        <td>0.631</td>
        <td>0.621</td>
        <td>0.674</td>
        <td>0.616</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>SCA</td>
        <td>0.611</td>
        <td>0.711</td>
        <td>0.537</td>
        <td>0.609</td>
        <td>0.596</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>KP</td>
        <td>0.595</td>
        <td>0.694</td>
        <td>0.539</td>
        <td>0.610</td>
        <td>0.614</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>Goals</td>
        <td>0.594</td>
        <td>0.633</td>
        <td>0.560</td>
        <td>0.576</td>
        <td>0.592</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>GCA</td>
        <td>0.483</td>
        <td>0.631</td>
        <td>0.433</td>
        <td>0.509</td>
        <td>0.539</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>Assists</td>
        <td>0.387</td>
        <td>0.468</td>
        <td>0.340</td>
        <td>0.410</td>
        <td>0.502</td>
    </tr>
    <tr>
        <td colspan="7"><em>Midfielders (ranked by pooled r)</em></td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PassPct</td>
        <td>0.842</td>
        <td>0.861</td>
        <td>0.782</td>
        <td>0.772</td>
        <td>0.806</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PrgDist</td>
        <td>0.668</td>
        <td>0.749</td>
        <td>0.660</td>
        <td>0.695</td>
        <td>0.556</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PPA</td>
        <td>0.675</td>
        <td>0.640</td>
        <td>0.682</td>
        <td>0.716</td>
        <td>0.612</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PrgC</td>
        <td>0.675</td>
        <td>0.663</td>
        <td>0.628</td>
        <td>0.681</td>
        <td>0.672</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>KP</td>
        <td>0.680</td>
        <td>0.640</td>
        <td>0.712</td>
        <td>0.645</td>
        <td>0.563</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PrgP</td>
        <td>0.646</td>
        <td>0.708</td>
        <td>0.645</td>
        <td>0.662</td>
        <td>0.506</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>SCA</td>
        <td>0.635</td>
        <td>0.627</td>
        <td>0.678</td>
        <td>0.658</td>
        <td>0.544</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>GCA</td>
        <td>0.465</td>
        <td>0.541</td>
        <td>0.582</td>
        <td>0.527</td>
        <td>0.489</td>
    </tr>
    <tr>
        <td colspan="7"><em>Defenders (ranked by pooled r)</em></td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>PassPct</td>
        <td>0.846</td>
        <td>0.830</td>
        <td>0.825</td>
        <td>0.778</td>
        <td>0.824</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>Clr</td>
        <td>0.640</td>
        <td>0.549</td>
        <td>0.570</td>
        <td>0.560</td>
        <td>0.575</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>PrgP</td>
        <td>0.546</td>
        <td>0.528</td>
        <td>0.555</td>
        <td>0.527</td>
        <td>0.465</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>Int</td>
        <td>0.495</td>
        <td>0.477</td>
        <td>0.505</td>
        <td>0.508</td>
        <td>0.371</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>TklW</td>
        <td>0.447</td>
        <td>0.413</td>
        <td>0.462</td>
        <td>0.375</td>
        <td>0.348</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>Blocks</td>
        <td>0.499</td>
        <td>0.264</td>
        <td>0.406</td>
        <td>0.344</td>
        <td>0.371</td>
    </tr>
  </tbody>
</table>

declines from 0.529 to 0.405 ($\Delta r = -0.124$), TklW from 0.414 to 0.293 ($\Delta r = -0.120$), and Int from 0.476 to 0.357 ($\Delta r = -0.119$).

This universal decay has a direct implication for practice: analysts using metric histories stretching two or more seasons back should weight recent seasons more heavily than older ones, and should not assume that long-run stability estimates are interchangeable with one-year estimates.

Figure 6: Stability Decay — One-Year vs Two-Year Horizon
(Pearson r at t→t+1 and t→t+2, by position group)

<table>
  <thead>
    <tr>
        <th>Position</th>
        <th>Metric</th>
        <th>t→t+1</th>
        <th>t→t+2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>FWD</td>
        <td>PrgC</td>
        <td>0.73</td>
        <td>0.61</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>PrgP</td>
        <td>0.71</td>
        <td>0.60</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>PPA</td>
        <td>0.70</td>
        <td>0.58</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>xG</td>
        <td>0.67</td>
        <td>0.56</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>npxG</td>
        <td>0.65</td>
        <td>0.54</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>SCA</td>
        <td>0.64</td>
        <td>0.53</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>KP</td>
        <td>0.61</td>
        <td>0.51</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>Goals</td>
        <td>0.58</td>
        <td>0.48</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>GCA</td>
        <td>0.53</td>
        <td>0.44</td>
    </tr>
    <tr>
        <td>FWD</td>
        <td>Assists</td>
        <td>0.45</td>
        <td>0.37</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PassPct</td>
        <td>0.83</td>
        <td>0.72</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PrgDist</td>
        <td>0.68</td>
        <td>0.55</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PPA</td>
        <td>0.68</td>
        <td>0.56</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PrgC</td>
        <td>0.68</td>
        <td>0.56</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>KP</td>
        <td>0.67</td>
        <td>0.55</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>PrgP</td>
        <td>0.66</td>
        <td>0.54</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>SCA</td>
        <td>0.66</td>
        <td>0.53</td>
    </tr>
    <tr>
        <td>MID</td>
        <td>GCA</td>
        <td>0.53</td>
        <td>0.42</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>PassPct</td>
        <td>0.83</td>
        <td>0.71</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>Clr</td>
        <td>0.57</td>
        <td>0.43</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>PrgP</td>
        <td>0.53</td>
        <td>0.41</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>Int</td>
        <td>0.48</td>
        <td>0.36</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>TklW</td>
        <td>0.41</td>
        <td>0.29</td>
    </tr>
    <tr>
        <td>DEF</td>
        <td>Blocks</td>
        <td>0.40</td>
        <td>0.28</td>
    </tr>
  </tbody>
</table>

Figure 6: Two-year stability decay by position group. Darker bars show $r(t \rightarrow t + 1)$; lighter bars show $r(t \rightarrow t + 2)$. Red arrows indicate metric-position combinations where $\Delta r < -0.03$. All 24 combinations decay.

## 3.6 Sensitivity Analyses

### 3.6.1 Playing-Time Threshold

The MIN_90S sweep (Figure 7) shows that mean Pearson r across all 24 position-metric combinations varies by at most $\Delta = 0.006$ across thresholds ranging from 3.0 to 7.0 ninety-minute equivalents. Stability estimates at the chosen threshold of MIN_90S = 5.0 ($\bar{r} = 0.616$) are representative of the full sweep range (0.614–0.620). The positional ranking (MID > FWD > DEF) is preserved at all thresholds.

### 3.6.2 Role-Change Exclusion

Of the 8,207 player-season pairs, 682 (8.3%) involved a player changing position bucket between season $t$ and $t + 1$. Excluding these changes the role-specific stability estimates by less than $|\Delta r| = 0.02$ in 23 of 24 position-metric combinations (the single exception is FWD PrgP, where same-role-only $r = 0.733$ vs full-sample $r = 0.709$, $\Delta r = +0.024$). Results are therefore robust to this source of potential contamination.

## 3.7 Stability Class Summary

Table 6 consolidates the full set of 24 position-metric combinations into four stability classes, providing a single-glance reference for practitioners. Metrics appearing in multiple

Figure 7: Sensitivity Analysis — MIN_90S Threshold Sweep
(Mean Pearson r and sample size across threshold values)

<table>
  <thead>
    <tr>
        <th>Metric</th>
        <th>3.0</th>
        <th>3.5</th>
        <th>4.0</th>
        <th>4.5</th>
        <th>5.0</th>
        <th>5.5</th>
        <th>6.0</th>
        <th>6.5</th>
        <th>7.0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>FWD (Mean r)</td>
        <td>0.628</td>
        <td>0.627</td>
        <td>0.626</td>
        <td>0.625</td>
        <td>0.625</td>
        <td>0.626</td>
        <td>0.627</td>
        <td>0.627</td>
        <td>0.628</td>
    </tr>
    <tr>
        <td>MID (Mean r)</td>
        <td>0.658</td>
        <td>0.659</td>
        <td>0.660</td>
        <td>0.661</td>
        <td>0.663</td>
        <td>0.662</td>
        <td>0.662</td>
        <td>0.663</td>
        <td>0.664</td>
    </tr>
    <tr>
        <td>DEF (Mean r)</td>
        <td>0.541</td>
        <td>0.539</td>
        <td>0.537</td>
        <td>0.536</td>
        <td>0.538</td>
        <td>0.538</td>
        <td>0.540</td>
        <td>0.542</td>
        <td>0.546</td>
    </tr>
    <tr>
        <td>N pairs</td>
        <td>9000</td>
        <td>8800</td>
        <td>8610</td>
        <td>8410</td>
        <td>8210</td>
        <td>8030</td>
        <td>7830</td>
        <td>7630</td>
        <td>7450</td>
    </tr>
  </tbody>
</table>

Figure 7: Sensitivity of stability estimates to the playing-time threshold (MIN_90S). Left panel: mean Pearson r by position group across thresholds 3.0–7.0; dashed vertical line marks the chosen threshold of 5.0. Right panel: corresponding sample sizes. Results are stable across the full sweep ($\Delta = 0.006$ in mean r).

position groups are assigned the class corresponding to their pooled estimate; where the class differs by position (e.g., PPA is High for FWD but Moderate for MID), both are listed.

# 4 Discussion

Before interpreting the individual findings, Table 7 summarises the practical recommendations that follow from the stability results, intended as a reference guide for performance analysts and recruitment practitioners.

## 4.1 Interpreting the Stability Hierarchy

The principal finding is that pass completion percentage is the most stable metric across all positional contexts ($r = 0.824$ for both midfielders and defenders), with a clear practical implication: recruitment departments should place greater weight on PassPct when evaluating future performance, since it reflects a player’s passing choices (short versus long, safe versus risky) that are determined by tactical role, individual playing style, and technical aptitude, all of which change slowly over a career. This aligns with the well-established finding in football analytics that possession and ball-retention metrics exhibit the highest signal-to-noise ratios among available event-level statistics (Pappalardo et al., 2019).

The ordering within the forward group is informative for scouting practice. Ball-progression metrics (PrgC, PrgP, PPA) rank higher than outcome metrics (Goals, xG, Assists). Progressive carrying and passing should therefore be treated as more

Table 6: Metric stability classes across all position groups (pooled seven-season estimates, MIN_90S = 5.0). Thresholds: Very High $r \geq 0.80$; High $0.70 \leq r < 0.80$; Moderate $0.50 \leq r < 0.70$; Low $r < 0.50$. Metrics appearing in multiple position groups are annotated where their class differs by position.

<table>
  <thead>
    <tr>
        <th>Stability Class</th>
        <th>Metric(s)</th>
        <th>Notes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>Very High ($r \geq 0.80$)</td>
        <td>PassPct</td>
        <td>Consistent across MID and DEF; most stable metric in dataset</td>
    </tr>
    <tr>
        <td>High ($0.70 \leq r &#x3C; 0.80$)</td>
        <td>PrgC (FWD), PrgP (FWD), PPA (FWD)</td>
        <td>Ball-progression metrics; reliably stable for forwards. PPA is Moderate for MID</td>
    </tr>
    <tr>
        <td rowspan="3">Moderate ($0.50 \leq r &#x3C; 0.70$)</td>
        <td>xG, npxG, SCA, KP, Goals, GCA</td>
        <td>Attacking metrics for FWD and MID; useful but with meaningful year-to-year variance</td>
    </tr>
    <tr>
        <td>PrgDist, PrgC (MID), PrgP (MID), PPA (MID)</td>
        <td>Progression metrics for midfielders</td>
    </tr>
    <tr>
        <td>Clr, PrgP (DEF)</td>
        <td>Most stable metrics among defenders</td>
    </tr>
    <tr>
        <td rowspan="2">Low ($r &#x3C; 0.50$)</td>
        <td>Assists</td>
        <td>Highly dependent on teammates and tactics</td>
    </tr>
    <tr>
        <td>Int, TklW, Blocks</td>
        <td>Defensive volume counts; strongly team-context dependent</td>
    </tr>
  </tbody>
</table>

Table 7: Practitioner takeaways: metric stability categories and recommendations for player evaluation and projection. Stability categories from Table 2; recommendations derived from the stability estimates and age-group results reported in Section 3.

<table>
  <thead>
    <tr>
        <th>Metric</th>
        <th>Stability</th>
        <th>Practitioner Recommendation</th>
    </tr>
    <tr>
        <th colspan="3"><em>All positions</em></th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>PassPct</td>
        <td>Very High</td>
        <td>Strong projection variable; prioritise in player profiling and benchmarking</td>
    </tr>
    <tr>
        <th colspan="3"><em>Forwards and Midfielders</em></th>
    </tr>
    <tr>
        <td>PrgC</td>
        <td>High</td>
        <td>Reliable scouting signal; use confidently in projection models</td>
    </tr>
    <tr>
        <td>PrgP</td>
        <td>High</td>
        <td>Reliable scouting signal; use confidently in projection models</td>
    </tr>
    <tr>
        <td>PPA</td>
        <td>High (FWD) / Moderate (MID)</td>
        <td>Strong scouting signal for forwards; useful but less reliable for midfielders</td>
    </tr>
    <tr>
        <td>xG</td>
        <td>Moderate</td>
        <td>Useful forecasting variable; preferable to raw goals</td>
    </tr>
    <tr>
        <td>npxG</td>
        <td>Moderate</td>
        <td>Useful forecasting variable for non-penalty output</td>
    </tr>
    <tr>
        <td>SCA</td>
        <td>Moderate</td>
        <td>Informative but apply appropriate uncertainty</td>
    </tr>
    <tr>
        <td>KP</td>
        <td>Moderate</td>
        <td>Informative but apply appropriate uncertainty</td>
    </tr>
    <tr>
        <td>Goals</td>
        <td>Moderate</td>
        <td>Use with context; outperformed by xG as projection input</td>
    </tr>
    <tr>
        <td>GCA</td>
        <td>Moderate</td>
        <td>Use with context; year-to-year variance is meaningful</td>
    </tr>
    <tr>
        <td>Assists</td>
        <td>Low</td>
        <td>Avoid over-weighting; highly dependent on teammates and tactics</td>
    </tr>
    <tr>
        <th colspan="3"><em>Defenders</em></th>
    </tr>
    <tr>
        <td>Clr</td>
        <td>Moderate</td>
        <td>More stable than other defensive volume counts; use with context</td>
    </tr>
    <tr>
        <td>PrgP (DEF)</td>
        <td>Moderate</td>
        <td>Ball-playing contribution; more stable than defensive action counts</td>
    </tr>
    <tr>
        <td>Int</td>
        <td>Low</td>
        <td>Context-dependent; do not use as standalone quality indicator</td>
    </tr>
    <tr>
        <td>TklW</td>
        <td>Low</td>
        <td>Context-dependent; strongly influenced by team defensive structure</td>
    </tr>
    <tr>
        <td>Blocks</td>
        <td>Low</td>
        <td>Least stable defensive metric; treat with caution in individual assessment</td>
    </tr>
    <tr>
        <th colspan="3"><em>Cross-cutting recommendations</em></th>
    </tr>
    <tr>
        <td>Age</td>
        <td>N/A</td>
        <td>Apply greater uncertainty when projecting U23 players; veteran histories are more reliable signals</td>
    </tr>
    <tr>
        <td>Temporal</td>
        <td>N/A</td>
        <td>Discount statistics older than one season; two-year-old data provides meaningfully weaker signal</td>
    </tr>
  </tbody>
</table>

reliable indicators of a forward’s future contribution than goal or assist tallies. This is consistent with the view that progression behaviour reflects habitual spatial tendencies and role-specific movement patterns (Decroos et al., 2019), whereas outcome realisation introduces additional randomness at the point of shooting or assist delivery. The finding connects to broader arguments that process metrics provide more stable assessments of player quality than outcome metrics (Muller et al., 2017).

The relative stability advantage of xG over raw Goals ($r = 0.664$ vs $r = 0.593$) further supports the view that expected metrics strip some of the finishing-luck component from goal counts (Rathke, 2017), making them more reliable forecasting inputs. Caley (2015) noted that shot quality models reduce the noise in goal-based performance estimation; the present results corroborate this at the season-to-season stability level.

The low stability of assists ($r = 0.428$ for forwards) deserves particular emphasis. A high-assist season is only weakly predictive of a high-assist following season, which is substantially weaker than an equivalent xG season. Recruitment analysts who weight assists heavily when evaluating attacking players risk over-pricing a metric that is disproportionately driven by teammate quality and tactical scheme rather than individual contribution. This is consistent with the finding of Brooks et al. (2016) in baseball that outcome metrics involving other players’ contributions display lower reliability than metrics reflecting only the focal player’s actions.

## 4.2 The Unreliability of Defensive Volume Metrics

The defensive volume statistics (TklW: $r = 0.414$; Blocks: $r = 0.399$; Int: $r = 0.476$) represent the lowest-stability metrics in the entire dataset, and this finding warrants explicit discussion given how prominently these metrics feature in defensive player assessments.

These statistics are widely used in scouting contexts as proxies for defensive activity and work rate. The present results suggest that such use should be treated with considerable caution. A player who records high tackle and interception counts in one season is not reliably predicted to do so the following season, even within the same league and club. This is most plausibly explained by the strong sensitivity of these counts to team context: a defender’s tackle count depends not just on their individual positioning and effort, but on how often their team defends, how high up the pitch the defensive line sits, whether teammates win the ball before it reaches them, and how opponents are set up to attack. A change in manager, a shift in formation, or a squad strengthening elsewhere can substantially alter a defender’s defensive action count without any change in their individual quality (Spearman, 2018).

This finding has important practical consequences. Defensive volume metrics should not

be used as standalone indicators of individual defensive quality in scouting or recruitment contexts. Where possible, analysts should contextualise them relative to team defensive metrics, or prefer metrics less sensitive to team structure. The comparatively higher stability of clearances (Clr: $r = 0.588$) and progressive passes (PrgP: $r = 0.529$) among defenders suggests that output-based and ball-playing contributions may provide a more stable signal of individual defensive quality than pure volume counts.

## 4.3 Age Effects and Implications for Recruitment

The systematic stability advantage of veterans over young players across attacking and midfield metrics likely reflects career-stage phenomena: experienced players have converged to stable positional and tactical roles, whereas younger players are still developing, experimenting with their game, or moving between tactical systems. The non-significant age effect among defensive volume metrics is consistent with the team-context argument: if team structure dominates individual signal for those metrics, age-related convergence has little room to manifest.

The practical implication is direct: recruitment analysts should apply greater uncertainty when projecting younger players, even when their recent statistics are comparable to veterans (Muller et al., 2017). A 22-year-old forward’s xG history is a noisier signal of future xG than a 32-year-old’s comparable history, because more of the younger player’s production reflects a developmental trajectory that has not yet stabilised. Projection models and transfer-fee models that treat all age groups symmetrically will systematically underestimate uncertainty for younger players and overestimate it for veterans.

## 4.4 Two-Year Decay and Temporal Weighting

The universal two-year decay, with a median $\Delta r = -0.097$, implies that two-season-old statistics carry substantially less predictive signal than one-season-old statistics. The magnitude is large enough to be practically significant: a metric with $r = 0.65$ at one year drops to approximately $r = 0.55$ at two years, representing the difference between Moderate and nearing the boundary of Low stability. Analysts assembling multi-season statistics panels should apply temporal discounting, upweighting recent seasons, rather than treating all historical seasons equally. For the least stable metrics (defensive volume statistics, where the two-year $r$ drops below 0.30), statistics older than one season arguably carry negligible individual-level signal and should be used with great caution in any projection context.

## 4.5 Stability, Skill, and Contextual Confounding

An important conceptual clarification: a high stability coefficient does not mean a metric is a pure measure of individual skill. PassPct is highly stable, but that stability may reflect not only technical aptitude but also tactical role (a deep-lying playmaker in a possession team will tend to have a high PassPct by design) and team style (squads with high average possession will systematically elevate individual completion rates). Two players with the same PassPct may represent very different levels of underlying skill if they operate in structurally different environments (Franks et al., 2016).

This distinction matters for how stability estimates should be used in practice. A high $r$ tells us that a player’s observed metric value is a reliable predictor of their future observed value, which is exactly what analysts need when selecting features for projection models or setting performance benchmarks. It does not, by itself, tell us that the metric isolates individual quality from context. Analysts seeking to separate player skill from team context should combine stability information with contextual adjustment methods such as league translation frameworks (Glazer, 2026); stability estimates of the kind reported here inform which metrics are worth that additional effort.

## 4.6 Limitations

Several limitations merit acknowledgement. First, stability estimates are pooled across all seven season pairs; we do not model temporal trends in stability, which may exist if tactical styles across the Big Five have changed systematically over the 2017/18–2024/25 period. Second, the within-league design (requiring the same league in both seasons) excludes transfer-year observations, meaning our estimates describe within-context stability and may not directly translate to cross-context predictions; that question is addressed by league translation research (Glazer, 2026). Third, position buckets are coarse; stability differences within each bucket (e.g., between central midfielders and wingers both classified as MID) may be informative but are beyond the scope of this analysis.

Fourth, the analysis does not account for several unmeasured confounders that may reduce within-player year-to-year consistency independently of any metric-level signal: significant injuries mid-season can truncate a player’s statistics even above the MIN_90S threshold; major tactical changes (new manager, formation shift) mid-season can alter role-dependent counts substantially; and squad role transitions (e.g., a player moving from squad rotation to first-choice starter, or vice versa) change both playing time and the competitive context in which statistics are accumulated. These factors may particularly affect the lower-stability metrics, where individual signal is already weak relative to contextual noise.

## 4.7 Future Directions

A natural extension of this work is to evaluate how the stability categories identified here translate into actual forecasting accuracy. Specifically, it would be informative to benchmark out-of-sample prediction error across metrics stratified by their stability class, quantifying the extent to which higher stability translates into lower projection error in practice. A second extension would apply the same framework to within-season split-half reliability, which would disentangle year-to-year contextual change from the fundamental reliability of each metric as a measurement instrument. Finally, finer-grained positional analysis, distinguishing central defenders from fullbacks or central midfielders from wide midfielders, could reveal within-bucket heterogeneity that the current three-group taxonomy obscures.

## 4.8 Implications for Scouting, Recruitment, and Forecasting

The results reported here have broad implications for any model or workflow that uses historical player statistics as inputs. Scouting systems that rank players on single-season metrics should weight stable metrics (PassPct, PrgC, PrgP) more heavily than volatile ones (Assists, TklW, Blocks), since a high score on a Low-stability metric is more likely to reflect transient circumstances than a genuine individual trait. Recruitment models that aggregate multi-season histories should apply temporal discounting rather than treating all seasons equally. Forecasting pipelines should incorporate age-group-specific uncertainty: the same historical statistic carries a meaningfully different predictive confidence for a 21-year-old versus a 31-year-old. The stability classes in Table 6 provide a ready reference for practitioners making these decisions.

# 5 Conclusions

This paper has developed a practical framework for identifying which football performance metrics provide reliable signals for player evaluation across the Big Five European leagues. Using 8,207 player-season pairs across seven consecutive season pairs and all five major European leagues, we show that:

1. Pass completion percentage is the single most stable metric ($r = 0.824$ for midfielders and defenders), and should be prioritised in player profiling and projection models across all positional contexts.

2. Ball-progression statistics are the most reliable scouting signals for forwards (PrgC: $r = 0.733$; PrgP: $r = 0.709$; PPA: $r = 0.707$) and should be weighted above outcome metrics such as goals and assists.

3. Defensive volume statistics (TklW, Blocks, Int) are the least stable metrics in the dataset ($r < 0.50$) and should not be used as standalone indicators of individual defensive quality; their sensitivity to team context limits their value in player assessment.

4. Veterans ($\ge$30 years) are systematically more stable than young players for attacking and midfield metrics (18/24 Fisher $z$-tests significant); recruitment analysts should apply greater uncertainty when projecting U23 players.

5. Stability decays universally at the two-year horizon (all 24 combinations; median $\Delta r = -0.097$); statistics older than one season should be temporally discounted in projection pipelines.

6. Results are robust to the playing-time threshold ($\Delta = 0.006$ in mean $r$ across MIN_90S $\in [3.0, 7.0]$) and to the exclusion of position-changers (23/24 combinations $|\Delta r| < 0.02$).

These findings provide actionable guidance for performance analysts, recruitment departments, and modellers. The practitioner takeaways in Table 7 summarise the metric-level recommendations. More broadly, the results demonstrate that metric stability varies substantially across positions and metric types, and that treating all statistics as equally reliable signals of future performance is an assumption that the data do not support.

# References

Albert, J., & Bennett, J. (2003). *Curve Ball: Baseball, Statistics, and the Role of Chance in the Game*. Copernicus/Springer-Verlag, New York.

Brooks, J., Kerr, M., & Guttag, J. (2016). Developing a data-driven player ranking in soccer using predictive model weights. In *Proceedings of the 22nd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining* (pp. 49–55). ACM. [https://doi.org/10.1145/2939672.2939695](https://doi.org/10.1145/2939672.2939695)

Caley, M. (2015). Premier League projections and new expected goals. *Cartilage Free Captain* [Online article].

Decroos, T., Bransen, L., Van Haaren, J., & Davis, J. (2019). Actions speak louder than goals: Valuing player actions in soccer. In *Proceedings of the 25th ACM SIGKDD International Conference on Knowledge Discovery and Data Mining* (pp. 1851–1861). ACM. [https://doi.org/10.1145/3292500.3330758](https://doi.org/10.1145/3292500.3330758)

Franks, A., D’Amour, A., Cervone, D., & Bornn, L. (2016). Meta-analytics: Tools for un-

derstanding the statistical properties of sports metrics. *Journal of Quantitative Analysis in Sports*, 12(4), 151–165.

Glazer, A. K. (2026). Nuthin’ but a G League: Estimating league translation factors. *Journal of Sports Analytics*. [https://doi.org/10.1177/22150218261428808](https://doi.org/10.1177/22150218261428808)

Krithick, A. (2024). *FBRef 2017–2024 for Europe’s Top 5 Leagues* [Kaggle dataset]. [https://www.kaggle.com/datasets/akshankrithick/fbref-2017-2024-for-europes-top-5-leagues](https://www.kaggle.com/datasets/akshankrithick/fbref-2017-2024-for-europes-top-5-leagues)

Koo, T. K., & Li, M. Y. (2016). A guideline of selecting and reporting intraclass correlation coefficients for reliability research. *Journal of Chiropractic Medicine*, 15(2), 155–163.

Müller, O., Simons, A., & Weinmann, M. (2017). Beyond crowd judgments: Data-driven estimation of market value in association football. *European Journal of Operational Research*, 263(2), 611–624. [https://doi.org/10.1016/j.ejor.2017.05.005](https://doi.org/10.1016/j.ejor.2017.05.005)

Pappalardo, L., Cintia, P., Rossi, A., Massucco, E., Ferragina, P., Pedreschi, D., & Giannotti, F. (2019). PlayeRank: Data-driven performance evaluation of soccer players. *Scientific Data*, 6, 236. [https://doi.org/10.1038/s41597-019-0247-7](https://doi.org/10.1038/s41597-019-0247-7)

Rathke, A. (2017). An examination of expected goals and shot efficiency in soccer. *Journal of Human Sport and Exercise*, 12(2proc), S514–S529. [https://doi.org/10.14198/jhse.2017.12.Proc2.05](https://doi.org/10.14198/jhse.2017.12.Proc2.05)

Sidorowicz, H. (2025). *Football Players Stats 2024–2025* [Kaggle dataset]. [https://www.kaggle.com/datasets/hubertsidorowicz/football-players-stats-2024-2025](https://www.kaggle.com/datasets/hubertsidorowicz/football-players-stats-2024-2025)

Shrout, P. E., & Fleiss, J. L. (1979). Intraclass correlations: Uses in assessing rater reliability. *Psychological Bulletin*, 86(2), 420–428.

Spearman, W. (2018). Beyond expected goals: Correcting for selection bias in soccer shooting data. In *Proceedings of the 12th MIT Sloan Sports Analytics Conference*. MIT, Cambridge, MA.

Van Roy, M., Robberechts, P., Yang, W., De Raedt, L., & Davis, J. (2020). Valuing the art of pressing in soccer. In *Proceedings of the AAAI Workshop on AI in Team Sports*. AAAI, New York.

# Statements and Declarations

## Ethical Considerations

Not applicable. This study analyses publicly available aggregate player performance data from sports statistics databases and does not involve human subjects research, clinical

trials, or personal data collection.

## Consent to Participate

Not applicable.

## Consent for Publication

Not applicable.

## Declaration of Conflicting Interest

The author declared no potential conflicts of interest with respect to the research, authorship, and/or publication of this article.

## Funding

This research received no specific grant from any public, commercial, or not-for-profit funding agency.

## Data Availability

All data used in this study are publicly available via the FBref-derived Kaggle datasets cited in Section 2. The processed analysis panel and fully reproducible analysis code are available at: [https://github.com/mohammadarshan/football-metric-stability](https://github.com/mohammadarshan/football-metric-stability).