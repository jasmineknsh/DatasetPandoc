![](media/image1.jpeg){width="0.8272965879265092in"
height="0.9066655730533684in"}![](media/image2.jpeg)[European Journal of
Operational Research 320 (2025)
271--289](https://doi.org/10.1016/j.ejor.2024.03.020)

> Invited Review
>
> ![](media/image3.png){width="0.38965660542432196in"
> height="0.3896609798775153in"}A survey of contextual optimization
> methods for decision-making under uncertainty
>
> Utsav Sadana [a](#_bookmark0), Abhilash Chenreddy [b](#_bookmark1),
> Erick Delage [b](#_bookmark1),[∗](#_bookmark4), Alexandre Forel
> [c](#_bookmark2), Emma Frejinger [d](#_bookmark3),
>
> Thibaut Vidal [c](#_bookmark2)
>
> []{#_bookmark0 .anchor}a []{#_bookmark1 .anchor}*Department of
> Computer Science and Operations Research, Université de Montréal,
> Québec, Canada*
>
> b []{#_bookmark2 .anchor}*GERAD & Department of Decision Sciences, HEC
> Montréal, Québec, Canada*
>
> c []{#_bookmark3 .anchor}*CIRRELT & SCALE-AI Chair in Data-Driven
> Supply Chains, Department of Mathematical and Industrial Engineering,
> Polytechnique Montréal, Québec, Canada*
>
> d *CIRRELT & Department of Computer Science and Operations Research,
> Université de Montréal, Québec, Canada*
>
> A R T I C L E I N F O
>
> *Keywords:*
>
> Contextual optimization Conditional stochastic programming Task-based
> learning
>
> Data-driven optimization Policy optimization
>
> A B S T R A C T
>
> Recently there has been a surge of interest in operations research
> (OR) and the machine learning (ML) community in combining prediction
> algorithms and optimization techniques to solve decision-making
> problems in the face of uncertainty. This gave rise to the field of
> contextual optimization, under which data-driven procedures are
> developed to prescribe actions to the decision-maker that make the
> best use of the most recently updated information. A large variety of
> models and methods have been presented in both OR and ML literature
> under a variety of names, including data-driven optimization,
> prescriptive optimization, predictive stochastic programming, policy
> optimization, (smart) predict/estimate-then-optimize, decision-focused
> learning, (task- based) end-to-end learning/forecasting/optimization,
> etc. This survey article unifies these models under the lens of
> contextual stochastic optimization, thus providing a general
> presentation of a large variety of problems. We identify three main
> frameworks for learning policies from data and present the existing
> models and methods under a uniform notation and terminology. Our
> objective with this survey is to both strengthen the general
> understanding of this active field of research and stimulate further
> theoretical and algorithmic advancements in integrating ML and
> stochastic programming.

Introduction
------------

This article surveys the literature on single and two-stage contex- tual
optimization. In contextual optimization, a decision-maker faces a
decision-making problem with uncertainty where the distribution of
uncertain parameters that affect the objective and the constraints is
unknown, although correlated side information (covariates or fea- tures)
can be exploited. The usefulness of side information in inferring
relevant statistics of uncertain parameters and, thereby, in decision-
making is evident in many different fields. For example, weather and
time of day can help resolve uncertainty about congestion on a road
network and aid in finding the shortest path traversing a city. In
portfolio optimization, stock returns may depend on historical prices
and sentiments posted on Twitter ([Xu & Cohen](#_bookmark221),
[2018](#_bookmark221)). Harnessing this information can allow
decision-makers to build a less risky portfolio. Similarly, a retailer
facing uncertain demand for summer products can infer whether the demand
will be low or high depending on the forecasted weather conditions
([Martínez-de Albeniz & Belkaid](#_bookmark155), [2021](#_bookmark155)).

In these applications, the decision-maker has access to historical data,
that is, past values of the covariates (e.g., weather) and the
corresponding uncertain parameter (e.g., congestion). Data-driven con-
textual optimization methods use this data to estimate the conditional
distribution of the uncertain parameter (or a sufficient statistic)
based on the covariate. Conversely, traditional stochastic optimization
models ignore contextual information and use unconditional distributions
of the uncertain parameters to make a decision ([Birge &
Louveaux](#_bookmark64), [2011](#_bookmark64)). Such a decision may be
suboptimal ([Ban & Rudin](#_bookmark51), [2019](#_bookmark51)) and, in
[some](#_bookmark180) cases, even at the risk of being infeasible
([Rahimian & Pagnon-](#_bookmark180) [celli](#_bookmark180),
[2022](#_bookmark180)). The availability of data and huge computational
power combined with advancements in machine learning (ML) and optimiza-
tion techniques have resulted in a shift of paradigm to contextual
optimization ([Mišić & Perakis](#_bookmark159), [2020](#_bookmark159)).

Making prescriptions using the side information requires a decision rule
that maps the observed covariate to an action. We identify three
different paradigms for learning this mapping.

> ∗ []{#_bookmark4 .anchor}Corresponding author.
>
> *E-mail addresses:* <utsav.sadana@umontreal.ca> (U. Sadana),
> <abhilash.chenreddy@hec.ca> (A. Chenreddy), <erick.delage@hec.ca> (E.
> Delage), <alexandre.forel@polymtl.ca> (A. Forel),
> <emma.frejinger@umontreal.ca> (E. Frejinger),
> <thibaut.vidal@polymtl.ca> (T. Vidal).
>
> <https://doi.org/10.1016/j.ejor.2024.03.020> Received 10 July 2023;
> Accepted 14 March 2024
>
> Available online 15 March 2024
>
> 0377-2217/© 2024 The Authors. Published by Elsevier B.V. This is an
> open access article under the CC BY license
> (<http://creativecommons.org/licenses/by/4.0/>).

-   **Decision rule optimization**: This approach was introduced to the
    > operation research community in [Liyanage and
    > Shanthikumar](#_bookmark148) ([2005](#_bookmark148)) for
    > data-driven optimization and popularized in [Ban
    > and](#_bookmark51) [Rudin](#_bookmark51) ([2019](#_bookmark51))
    > for big data environments, although a similar idea was already
    > common practice in reinforcement learning under the name of policy
    > gradient methods (see [Sutton et al.](#_bookmark204),
    > [1999](#_bookmark204), and literature that followed). It consists
    > in employing a parameterized mapping as the decision rule and in
    > identifying the parameter that achieves the best empirical
    > performance based on the available data. The decision rule can be
    > formed as a linear combination of functions of the covariates or
    > even using a deep neural net- work (DNN). When the data available
    > is limited, some form of regularization might also be needed.

-   **Sequential learning and optimization (SLO)**: [Bertsimas
    > and](#_bookmark58) [Kallus](#_bookmark58) ([2020](#_bookmark58))
    > appears to be the first to have formalized this two-stage
    > procedure (also referred to as predict/estimate-then- optimize or
    > prescriptive optimization/stochastic programming) that first uses
    > a trained model to predict a conditional distribution for the
    > uncertain parameters given the covariates, and then solves an
    > associated contextual stochastic optimization (CSO) problem to
    > obtain the optimal action. This procedure can be robustified to
    > reduce post-decision disappointment ([Smith &
    > Winkler](#_bookmark196), [2006](#_bookmark196)) caused by model
    > overfitting or misspecification by using proper regularization at
    > training time or by adapting the CSO problem formulation.

-   **Integrated learning and optimization (ILO)**: In the spirit of
    > identifying the best decision rule, one might question in SLO the
    > need for high precision predictors when one is instead mostly
    > interested in the quality of the resulting prescribed action. This
    > idea motivates an integrated version of learning and optimiza-
    > tion that searches for the predictive model that guides the CSO
    > problem toward the best-performing actions. The ILO paradigm
    > appears as early as in [Bengio](#_bookmark54)
    > ([1997](#_bookmark54)) and has seen a resurgence recently in
    > active streams of literature under various names such as smart
    > predict-then-optimize, decision-focused learning, and (task-based)
    > end-to-end learning/forecasting/optimization.

The outline of the survey goes as follows. Section [2](#_bookmark6)
rigorously defines the three frameworks for identifying the best mapping
from covariate to action based on data: decision rule optimization, SLO,
and ILO. Section [3](#_bookmark20) reviews the literature on decision
rule optimization with linear and non-linear decision rules. Section
[4](#_bookmark24) focuses on SLO, including the models that lead to
robust decisions, and Section [5](#_bookmark28) describes the models
based on the ILO framework and the algorithms used to train them.
Because ILO is the more recent and less explored framework of the three
identified, we provide a separate subsection of applications of ILO to
diverse problems such as logistics and energy management. Section
[6](#_bookmark40) provides an overview of active research directions
being pursued both from a theoretical and applications perspective.
Section [7](#_bookmark41) concludes our survey with a summary of our
contributions.

We note that there are other surveys and tutorials in the literature
that are complementary to ours. [Mišić and Perakis](#_bookmark159)
([2020](#_bookmark159)) survey the applications of the SLO framework to
problems in supply chain man- agement, revenue management, and
healthcare operations. [Qi and Shen](#_bookmark178)
([2022](#_bookmark178)) is a tutorial that mainly focuses on the
application of ILO to expected value-based models with limited
discussions on more general approaches. It summarizes the most popular
methods and some of their theoretical guarantees. [Kotary et
al.](#_bookmark136) ([2021](#_bookmark136)) provide a comprehensive

[et al.](#_bookmark153) ([2023](#_bookmark153))[1](#_bookmark5) include
more recent expected value-based models and provide a comprehensive
evaluation of their methods, complementing the toolbox of [Tang and
Khalil](#_bookmark205) ([2022](#_bookmark205)) that provided an
interface for solving expected value-based models.

Our survey of ILO literature goes beyond the expected value-based models
and reflects better the more modern literature by casting the contextual
decision problem as a CSO problem and presenting a com- prehensive
overview of the current state of this rapidly progressing field of
research. We establish links between approaches that minimize regret
([Elmachtoub & Grigas](#_bookmark100), [2022](#_bookmark100)),
(task-based) end-to-end learn- ing ([Donti et al.](#_bookmark94),
[2017](#_bookmark94)) and imitation-based models ([Kong et
al.](#_bookmark135), [2022](#_bookmark135)). Further, we create a
taxonomy based on the training procedure for a general ILO framework
encompassing recent theoretical and algorith- mic progresses in
designing differentiable surrogates and optimizers and improving
training procedures based on unrolling and implicit differentiation.

Contextual optimization: An overview
------------------------------------

[]{#_bookmark6 .anchor}The contextual optimization paradigm seeks a
decision (i.e., an action) ***𝒛*** in a feasible set  ⊆ R𝑑***𝒛*** that
minimizes a cost function 𝑐(***𝒛***, ***𝒚***) with uncertain parameters
***𝒚*** ∈  ⊆ R^𝑑^***𝒚*** . The uncertain parameters are unknown when
making the decision. However, a vector of relevant covariates ***𝒙*** ∈
 ⊆ R𝑑***𝒙*** , which is correlated with the uncertain param- eters
***𝒚***, is revealed before having to choose ***𝒛***. The joint
distribution of the covariates in  and uncertain parameters in  is
denoted by P.

1.  *Contextual problem and policy*

In general, uncertainty can appear in the objectives and constraints of
the problem. In the main sections of this paper, we focus on problems
with uncertain objectives and consider that the decision-maker is risk-
neutral. We broaden the discussion to risk-averse settings and uncertain
constraints in Section [6](#_bookmark40).

*The CSO problem.* Given a covariate described by a vector of covari-
ates ***𝒙*** and the joint distribution P of the covariates ***𝒙*** and
uncertain parameter ***𝒚***, a risk-neutral decision-maker is interested
in finding an optimal action 𝑧∗(***𝒙***) ∈  that minimizes the expected
costs conditioned on the covariate ***𝒙***. Formally, the optimal action
is a solution to the CSO problem given by:

(CSO) 𝑧^∗^(***𝒙***) ∈ argmin E~P(***𝒚***~ ~***𝒙***)~ \[𝑐 (***𝒛***,
***𝒚***)\] , []{#_bookmark7 .anchor}(1)

> ***𝒛***∈

where P(***𝒚 𝒙***) denotes the conditional distribution of ***𝒚*** given
the co- variate ***𝒙*** and it is assumed that a minimizer exists. For
instance, a minimizer exists when  is compact, P(***𝒚 𝒙***) has bounded
support and

𝑐(***𝒛***, ***𝒚***) is continuous in ***𝒛*** almost surely (see [Van
Parys et al.](#_bookmark209), [2021](#_bookmark209), for more details).

Problem ([1](#_bookmark7)) can equivalently be written in a compact form
using the expected cost operator ℎ(⋅, ⋅) that receives an action as a
first argument and a distribution as a second argument:

𝑧^∗^(***𝒙***) ∈ argmin ℎ(***𝒛***, P(***𝒚 𝒙***)) ∶= E~P(***𝒚***~
~***𝒙***)~ \[𝑐 (***𝒛***, ***𝒚***)\] . []{#_bookmark8 .anchor}(2)

> ***𝒛***∈

*Optimal policy.* In general, the decision-maker repeatedly solves CSO
problems in many different contexts. Hence, the decision-maker is
interested in finding the policy that provides the lowest long-term
expected cost, that is:

> 𝜋^∗^ ∈ argmin E~P~\[𝑐(𝜋(***𝒙***), ***𝒚***)\] = argmin
> E~P~\[ℎ(𝜋(***𝒙***), P(***𝒚***\|***𝒙***))\], []{#_bookmark9 .anchor}(3)

tion of constrained optimization models (see also [Bengio et
al.](#_bookmark55), [2021](#_bookmark55)). It also reviews some of the
earlier literature on ILO applied to what

where 𝛱 ∶= {𝜋 ∶  → } denotes the class of all feasible policies.

we define as ''expected value-based models'', a subset of CSO problems [
]{.underline}

(see [Definition](#_bookmark15) [1](#_bookmark15)) where uncertainty can
be completely described by [a](#_bookmark153) sufficient statistic
before the optimization problem is solved. [Mandi](#_bookmark153)

> 1 [Mandi et al.](#_bookmark153) ([2023](#_bookmark153)) appeared
> online soon after the submission of our paper.
>
> ![](media/image4.png){width="0.5191229221347332in"
> height="8.666666666666667e-2in"}

![](media/image5.png){width="2.012396106736658in"
height="0.40625in"}![](media/image6.png)

> []{#_bookmark10 .anchor}**Fig. 1.** Decision and training pipelines
> based on the decision rule paradigm: (left) the decision pipeline and
> (right) the training pipeline for a given training example
> (***𝒙***~𝑖~, ***𝒚***~𝑖~).

Note that the optimal policy does not need to be obtained explic- itly
in a closed form. Indeed, based on the interchangeability prop- erty
(see Theorem 14.60 of [Rockafellar & Wets](#_bookmark183),
[2009](#_bookmark183)), solving the CSO problem ([1](#_bookmark7)) for
any covariate ***𝒙*** naturally identifies an optimal policy:

> 𝜋̄(***𝒙***) ∈ argmin ℎ(***𝒛***, P(***𝒚 𝒙***)) a.s.
>
> ***𝒛***∈
>
> ⇔ E ( ( ) P( )) = min E ( ( ) P( ))
>
> 𝜋∈𝛱

assuming that a minimizer of ℎ(***𝒛***, P(***𝒚 𝒙***)) exists almost
surely. Thus, the two optimal policies 𝜋∗ and 𝑧∗(⋅) coincide.

2.  *Mapping covariate to actions in a data-driven environment*

Unfortunately, the joint distribution P is generally unknown. In- stead,
the decision-maker possesses historical data  𝑁 that is assumed to be
made of independent and identically distribut^𝑖^e^=^d^1^ realizations of
(***𝒙***, ***𝒚***) ∈  × . Using this data, the decision-maker aims

to find a policy that approximates well the optimal policy given by
([3](#_bookmark9)). Many approaches have been proposed to find effective
approx- imate policies. Most of them can be classified into the three
main frameworks that we introduce below: (i) decision rule optimization,

\(ii\) sequential learning and optimization, and (iii) integrated
learning and optimization.

1.  *Decision rule optimization*

In this framework, the policy is assumed to belong to a hypothesis class
𝛱***𝜽*** ∶= {𝜋***~𝜽~***}~***𝜽***∈𝛩~ ⊆ 𝛱 that contains a family of
parametric policies

𝜋***~𝜽~*** ∶  →  (e.g., linear functions or decision trees). The
parameterized

policy 𝜋***~𝜽~*** maps directly any covariate ***𝒙*** to an action
𝜋***~𝜽~***(***𝒙***) and will be

referred to as a decision rule.

> Denote by P̂ 𝑁 the empirical distribution of (***𝒙***, ***𝒚***) given
> historical

2.  []{#bookmark11 .anchor}*Learning and optimization*

The second and third frameworks combine a predictive component and an
optimization component. The predictive component is a general model
𝑓***~𝜽~***, parameterized by ***𝜽***, whose role is to provide the input
of the optimization component. For any covariate ***𝒙***, the
intermediate input

𝑓***~𝜽~***(***𝒙***) can be interpreted as a predicted distribution that
approximates the true conditional distribution P(***𝒚 𝒙***) (or a
sufficient statistic in the case of expected value-based models). The
predictive component is typically learned from historical data.

At decision time, a learning and optimization decision pipeline (see
[Fig. 2](#_bookmark13)) solves the CSO problem under
𝑓***~𝜽~***(***𝒙***), namely:

> 𝑧^∗^(***𝒙***, 𝑓***~𝜽~***) ∈ argmin ℎ(***𝒛***, 𝑓***~𝜽~***(***𝒙***)).
> []{#_bookmark12 .anchor}(5)
>
> ***𝒛***∈

The solution of Problem ([5](#_bookmark12)) minimizes the expected cost
with respect to the predicted distribution 𝑓***~𝜽~***(***𝒙***). Notice
that the only approximation between Problem ([5](#_bookmark12)) and the
true CSO problem in ([2](#_bookmark8)) lies in 𝑓***~𝜽~*** being an
approximation of P(***𝒚 𝒙***). Since the predicted distribution changes
with the covariate ***𝒙***, this pipeline also provides a policy. In
fact, if the predictive component were able to perfectly predict the
true conditional distribution P(***𝒚 𝒙***) for any ***𝒙***, the pipeline
would recover the optimal policy 𝜋∗ given in ([3](#_bookmark9)).

![](media/image8.png){width="3.4068219597550304in" height="0.40625in"}

> []{#_bookmark13 .anchor}**Fig. 2.** Decision pipeline for learning and
> optimization.

We now detail the second and third frameworks to address con- textual
optimization: SLO and ILO. They differ in the way the predic- tor
𝑓***~𝜽~***(***𝒙***) is trained using the historical data.

> *Sequential learning and optimization.* In this framework, the
> contextual

data  . One can identify the ''best'' parameterization of the policy

in 𝛱***𝜽*** by solving the following empirical risk minimization (ERM)
problem:

predictor is obtained by minimizing an estimation error, 𝜌, between the
conditional distribution given by 𝑓***~𝜽~***(***𝒙***) and the true
conditional []{#_bookmark14 .anchor}distribution of ***𝒚*** given
***𝒙***. Training the contextual parametric predictor

> (ERM) ***𝜽***∗ ∈ argmin 𝐻(𝜋***~𝜽~***, P̂ 𝑁 ) ∶= EP̂
>
> ***𝜽***

\[𝑐(𝜋***~𝜽~***(***𝒙***), ***𝒚***)\]. (4)

> usually implies solving the following estimation problem:

In simple terms, Problem ([4](#_bookmark14)) finds the policy
𝜋***~𝜽~***∗ ∈ 𝛱***𝜽*** that minimizes the expected costs over the
training data. This decision pipeline is shown in [Fig.](#_bookmark10)
[1](#_bookmark10). Notice that there are two approximations of Problem
([3](#_bookmark9)) made by Problem ([4](#_bookmark14)). First, the
policy is restricted to a hypothesis class that may not contain the true
optimal policy. Second, the long-term expected costs are calculated over
the empirical distribu-

tion P̂ 𝑁 rather than the true distribution P. Furthermore, Problem
([4](#_bookmark14))

focuses its policy optimization efforts on the overall performance (av-

eraged over different covariates) and disregards the question of making
the policy achieve a good performance uniformly from one covariate to
another.

> min 𝜌(𝑓***~𝜽~***, P̂ 𝑁 ) + 𝛺(***𝜽***) with 𝜌(𝑓***~𝜽~***, P̂ 𝑁 ) ∶= EP̂ 𝑁
> \[D(𝑓***~𝜽~***(***𝒙***), ***𝒚***)\], (6)

where D is a divergence function, e.g., negative log-likelihood and the
regularization term 𝛺(***𝜽***) controls the complexity of 𝑓***~𝜽~***.
The conditional distribution can also take a non-parametric form, e.g.
𝑘-nearest neigh- bor, where ***𝜽*** then captures hyper-parameters of
the non-parametric model (such as the number of neighbors) selected
through a form of []{#_bookmark15 .anchor}cross-validation scheme. The
SLO training pipeline is shown in [Fig. 3](#_bookmark17).

> **Definition 1** (*Expected Value-Based Models*)**.** When the cost
> function

𝑐(***𝒙***, ***𝒚***) of the decision model is linear in ***𝒚***, the
problem of estimating a conditional distribution reduces to finding the
expected value of

> ![](media/image9.png)![](media/image12.png){width="0.5183748906386701in"
> height="8.654090113735784e-2in"}Finding the best parameterization of a
> contextual predictor that minimizes the downstream expected costs of
> the CSO solution can be formulated as the following problem:
>
> min 𝐻(𝑧^∗^(⋅, 𝑓***~𝜽~***), P̂ 𝑁 ) = min EP̂ \[𝑐(𝑧^∗^(***𝒙***,
> 𝑓***~𝜽~***), ***𝒚***)\]. []{#_bookmark16 .anchor}(9)
>
> []{#_bookmark17 .anchor}**Fig. 3.** SLO training pipeline for a given
> training example.

the uncertain parameter given the covariates since ℎ(***𝒛***, P(***𝒚
𝒙***)) = E ~(~ ~)~\[𝑐(***𝒛***, ***𝒚***)\] = 𝑐(***𝒛***, E ~(~
~)~\[***𝒚***\]). Training the contextual predictor, therefore, reduces
to a mean regression problem over a parameterized function
𝑔***~𝜽~***(***𝒙***). Specifically,

> min 𝜌(𝑔 , P̂ ) + 𝛺(***𝜽***) 𝜌(𝑔 , P̂ ) ∶= E 𝑑(𝑔 (***𝒙***), ***𝒚***) ,
> []{#_bookmark18 .anchor}(7)
>
> ***𝜽***

for some distance metric 𝑑, usually the mean squared errors. While the
mean squared error is known to asymptotically retrieve 𝑔***𝜽***̂
(***𝒙***) = E ~(~ ~)~\[***𝒚***\] as 𝑁 → ∞ under standard conditions,
other distance metric or more general loss functions can also be used
([Hastie et al.](#_bookmark119), [2009](#_bookmark119)). For any new
covariate ***𝒙***, an action is obtained using:

> 𝑧^∗^(***𝒙***, 𝑔***~𝜽~***) ∈ argmin ℎ(***𝒛***, 𝛿~𝑔~ ~(***𝒙***)~) =
> argmin 𝑐(***𝒛***, 𝑔***~𝜽~***(***𝒙***)), []{#_bookmark19 .anchor}(8)

The objective function in ([9](#_bookmark16)) minimizes the expected
cost of the policy over the empirical distribution. The policy induced
by this training problem is thus optimal with respect to the predicted
distribution and minimizes the average historical costs over the whole
training data. [Fig.](#_bookmark22) [5](#_bookmark22) describes how the
downstream cost is propagated by the predictive model during training.
This training procedure necessarily comes at the price of heavier
computations because an optimization model needs to be solved for each
data point, and differentiation needs to be applied through an argmin
operation.

3.  *Summary*

This section presented the main pipelines proposed in recent years to
address contextual optimization. Although these pipelines all in- clude
a learning component, they differ significantly in their specific
structures and training procedures. Overall, there are several design
choices that the decision-maker should make when tackling contextual
optimization: (i) the type of loss function used during training, which
defines whether an approach belongs to the decision rule (using ERM),
sequential (minimizing the estimation error), or integrated paradigm
(minimizing the downstream cost of the policy), (ii) the class of the

***𝒛***∈

> ***𝜽 𝒛***∈
>
> predictive model (e.g., linear, neural network, or random forest) and

where, with a slight abuse of notation, 𝑧∗ now takes an estimator of the
mean of the conditional distribution as the second argument, and with
𝛿***~𝒚~*** being the Dirac distribution putting all its mass at ***𝒚***.
In the remainder of this survey, we refer to these approaches as
**expected value-based models**, while the more general models that
prescribe using a conditional distribution estimator (i.e. 𝑧∗(***𝒙***,
𝑓***~𝜽~***)) will be referred as a **conditional distribution-based
models** when it is not clear from the context.

> *Integrated learning and optimization.* Sequential approaches ignore
> the mismatch between the prediction divergence D and the cost function

𝑐(***𝒙***, ***𝒚***). Depending on the covariate, a small prediction
error about

P(***𝒚 𝒙***) may have a large impact on the prescription. In integrated
learning, the goal is to maximize the prescriptive performance of the
induced policy. That is, we want to train the predictive component to
minimize the task loss (i.e., the downstream costs incurred by the
decision) as stated in ([3](#_bookmark9)). The prescriptive performance
may guide the estimation procedure toward a solution with higher MSE (or
any distance metric) that nevertheless produces a nearly-optimal
decision. This is illustrated in [Fig. 4](#_bookmark21).

its hyperparameters. Each design choice has its own inductive bias and
may imply specific methodological challenges, especially for ILO. In
general, it is a priori unclear what combination of choices will lead to
the best performance with limited data; therefore, pipelines have to be
evaluated experimentally.

In the following sections, we survey the recent literature corre-
sponding to the three main frameworks for contextual optimization using
the notation introduced so far, which is summarized in [Table
1](#_bookmark23).

Decision rule optimization {#decision-rule-optimization-1}
--------------------------

[]{#_bookmark20 .anchor}Decision rules obtained by solving the ERM in
Problem ([4](#_bookmark14)) minimize the cost of a policy on the task,
that is, the downstream optimization problem. Policy-based approaches
are especially efficient computation- ally at decision time since it
suffices to evaluate the estimated policy. No optimization problem needs
to be solved once the policy is trained. We defined the decision rule
approach as employing a parameterized mapping 𝜋***~𝜽~***(***𝒙***), e.g.,
linear policies ([Ban & Rudin](#_bookmark51), [2019](#_bookmark51)) or a
neural network ([Oroojlooyjadid et al.](#_bookmark172),
[2020](#_bookmark172)). Since policies obtained using neural networks
lack interpretability, linear decision rules are widely used.

1.  *Linear decision rules*

[Ban and Rudin](#_bookmark51) ([2019](#_bookmark51)) show that an
approach based on the sample- average approximation (SAA) that
disregards side information can lead to inconsistent decisions (i.e.,
asymptotically suboptimal) for a newsvendor problem. Using linear
decision rules (LDRs), they study two variants of the newsvendor problem
with and without regulariza- tion:

> ![](media/image13.png){width="2.8443952318460193in"
> height="1.846451224846894in"}min 𝐻(𝜋, P̂ )+𝛺(𝜋) = min [ 1]{.underline}
> ∑ ( − )+ + ( − )+ + ‖ ‖2

𝑁

𝑢 𝑦 ***𝜽***^⊤^***𝒙*** 𝑜 ***𝜽***^⊤^***𝒙*** 𝑦 𝜆

> where 𝑢 and 𝑜 denote the per unit backordering (underage) and hold-
> ing (overage) costs. [Ban and Rudin](#_bookmark51)
> ([2019](#_bookmark51)) show that for a linear
>
> []{#_bookmark21 .anchor}~∗~ ~∗~ demand model, the generalization error
> for the ERM model scales as
>
> **Fig. 4.** Predicting 𝑔***~𝜽~*** (***𝒙***) results in the optimal
> action 𝑧 (***𝒙***, 𝑔***~𝜽~*** ) = 𝑧 (***𝒙***) whereas a small

O(𝑑 ∕√𝑁) when there is no regularization and as O(𝑑 ∕(√𝑁𝜆)) with

> 𝑐(***𝒙***, ***𝒚***) ∶= −***𝒚***⊤ ***𝒙***, i.e., ℎ(***𝒛***,
> P(***𝒚***\|***𝒙***)) = −E\[***𝒚***\|***𝒙***\]⊤ ***𝒛*** (adapted from
> [Elmachtoub and Grigas](#_bookmark100)

regularization. However, one needs to balance the trade-off between

> ![](media/image14.png){width="0.5191229221347332in"
> height="8.666666666666667e-2in"}

![](media/image15.png)

[]{#_bookmark22 .anchor}**Fig. 5.** ILO training pipeline for a given
training example.

> []{#_bookmark23 .anchor}**Table 1**

[Notation: distributions, variables, and operators.]{.underline}

+-------------------------+---------------+-------------------------+
|                         | Domain        | Description             |
+=========================+===============+=========================+
| > P                     | ( × )      | True (unknown) joint    |
|                         |               | distribution of         |
|                         |               | (***𝒙***, ***𝒚***)      |
+-------------------------+---------------+-------------------------+
| > P̂ 𝑁                   | ( × )      | Joint empirical         |
|                         |               | distribution of         |
|                         |               | (***𝒙***, ***𝒚***)      |
+-------------------------+---------------+-------------------------+
| > 𝛿~𝑦~                  | ()          | Dirac distribution that |
|                         |               | puts all of its weight  |
|                         |               | on ***𝒚***              |
+-------------------------+---------------+-------------------------+
| > ***𝒙***               |  ⊆ R𝑑***𝒙*** | Contextual information  |
+-------------------------+---------------+-------------------------+
| > ***𝒚***               | 𝑌 ⊆ R𝑑***𝒚*** | Uncertain parameters    |
+-------------------------+---------------+-------------------------+
| > ***𝒛***               |  ⊆ R𝑑***𝒛*** | A feasible action       |
+-------------------------+---------------+-------------------------+
| > ***𝜽***               | 𝛩             | Parameters of a         |
|                         |               | prediction model        |
+-------------------------+---------------+-------------------------+
| > ***𝜽***̂               | 𝛩             | Optimal parameter value |
|                         |               | that minimizes the      |
|                         |               | estimation error        |
+-------------------------+---------------+-------------------------+
| > 𝑐(***𝒛***, ***𝒚***)   | R             | Cost of an action       |
|                         |               | ***𝒛*** under ***𝒚***   |
+-------------------------+---------------+-------------------------+
| > ℎ(***𝒛***, Q***~𝒚~*** | R             | Expected cost of an     |
| > )                     |               | action ***𝒛*** under    |
|                         |               | Q***~𝒚~*** (a           |
|                         |               | distribution over       |
|                         |               | ***𝒚***)                |
+-------------------------+---------------+-------------------------+
| > 𝐻(𝜋, Q)               | R             | Expected cost of a      |
|                         |               | policy 𝜋 under Q (a     |
|                         |               | distribution over       |
|                         |               | (***𝒙***, ***𝒚***))     |
+-------------------------+---------------+-------------------------+
| > 𝑓***~𝜽~***(***𝒙***)   | ()          | Estimate of the         |
|                         |               | conditional             |
|                         |               | distribution of ***𝒚*** |
|                         |               | given ***𝒙***           |
+-------------------------+---------------+-------------------------+
| > 𝑔***~𝜽~***(***𝒙***)   | R𝑑***~𝒚~***   | Estimate of the         |
|                         |               | conditional expectation |
|                         |               | of ***𝒚*** given        |
|                         |               | ***𝒙***                 |
+-------------------------+---------------+-------------------------+

> 𝜋∗(***𝒙***)  Optimal solution of CSO under true conditional
> distribution P(***𝒚 𝒙***)
>
> 𝜋***~𝜽~***(***𝒙***)  Action prescribed by a policy parameterized by
> ***𝜽*** for context ***𝒙***
>
> 𝑧∗(***𝒙***)  Optimal solution to the CSO problem under the true
> conditional distribution P(***𝒚 𝒙***)
>
> 𝑧∗(***𝒙***, 𝑓***~𝜽~***)  Optimal solution to the CSO problem under
> the conditional distribution 𝑓***~𝜽~***(***𝒙***)
>
> 𝑧∗(***𝒙***, 𝑔***~𝜽~***)  Optimal solution to the CSO problem under
> the Dirac distribution 𝛿~𝑔~ ~(***𝒙***)~
>
> 𝜌(𝑓***~𝜽~***, P̂ 𝑁 ) R Expected prediction error for distribution model
> 𝑓***~𝜽~*** based on empirical distribution P̂ 𝑁
>
> 𝜌(𝑔***~𝜽~***, P̂ 𝑁 ) R Expected prediction error for point prediction
> model 𝑔***~𝜽~*** based on empirical distribution P̂ 𝑁

performance from using LDRs. [Ban and Rudin](#_bookmark51)
([2019](#_bookmark51)) consider un- constrained problems because it is
difficult to ensure the feasibility

given below:

> 𝐿

of policies and maintain computational tractability using the ERM
approach.

> [Bertsimas and Kallus](#_bookmark58) ([2020](#_bookmark58)) present a
> general theory for gener-
>
> 𝜑 ∶  → R ∃ 𝐿 ∈ N, ***𝒗***~1~, ***𝒗***~2~, ... , ***𝒗***~𝐿~ ∈ ,
> 𝜑(***𝒙***) = 𝑎~𝑙~𝐾(***𝒗***~𝑙~, ***𝒙***), ∀***𝒙*** ∈  ,
>
> 𝑙=1
>
> with the inner product of 𝜑~1~(***𝒙***) = ∑𝐿1 𝑎𝑖 𝐾(***𝒗***𝑖 , ***𝒙***)
> and 𝜑~2~(***𝒙***) =

alization bounds of decision rules based on Rademacher complexity

> ∑𝐿2 𝑎^𝑗^ 𝐾(***𝒗***^𝑗^ , ***𝒙***) given by:
>
> 𝑖=1 1 1

that goes beyond LDR, although their main examples in this context
pertain to LDR. Unfortunately, LDRs may not be asymptotically optimal in
general. To generalize LDRs, one can consider decision rules that

> 𝑗=1 2 2
>
> 𝐿1 𝐿2

[⟨](#_bookmark59)𝜑~1~, 𝜑~2~[⟩](#_bookmark59) = 𝑎^𝑖^ 𝑎^𝑗^ 𝐾(***𝒗***𝑖 ,
***𝒗***^𝑗^ ).

[2019](#_bookmark51)). It is also possible to lift the covariate vector
to a reproducing kernel Hilbert space (RKHS, [Aronszajn](#_bookmark46),
[1950](#_bookmark46)), as seen in the next section.

2.  *RKHS-based decision rules*

[Bertsimas and Koduri](#_bookmark59) ([2022](#_bookmark59)) approximate
the optimal policy with a linear policy in the RKHS, i.e. 𝜋 (***𝒙***) ∶=
𝜑, 𝐾(***𝒙***, ⋅) when 𝑑 = 1, and show using the representer theorem (see
Theorem 9 in [Hofmann et al.](#_bookmark122), [2008](#_bookmark122))
that the solution of the following regularized problem:

> min 𝐻(𝜋~𝜑~, P̂ 𝑁 ) + 𝜆‖𝜑‖2,

𝜑∈

takes the form 𝜋∗(***𝒙***) = ∑𝑁

> 2
>
> 𝐾(***𝒙***~𝑖~, ***𝒙***)𝑎∗. Hence, this reduces the decision

To obtain decision rules that are more flexible than linear ones with
respect to ***𝒙***, it is possible to lift the covariate vector to an
RKHS in which LDRs might achieve better performance. Let 𝐾 ∶  ×  → R
be the symmetric positive definite kernel associated with the chosen

rule problem to:

> min 𝐻

(∑𝑁

> 𝑖=1
>
> 𝐾(***𝒙***~𝑖~, ⋅)𝑎~𝑖~, P̂ 𝑁 )

\+ 𝜆

> 𝑁 𝑁
>
> 𝐾(***𝒙***~𝑖~, ***𝒙***~𝑗~ )𝑎~𝑖~𝑎~𝑗~ .

RKHS, e.g., the Gaussian kernel 𝐾(***𝒙*** , ***𝒙*** ) ∶= exp(−‖***𝒙*** −
***𝒙*** ‖2∕(2𝜎2)).

> ***𝒂***∈R𝑁
>
> 𝑖=1
>
> 𝑖=1 𝑗=1

This RKHS approach appeared earlier in [Ban and Rudin](#_bookmark51)
([2019](#_bookmark51)) and [Bazier-Matte and Delage](#_bookmark53)
([2020](#_bookmark53)) who respectively study the data-

extension to the scenario-based optimal policy. Mathematically,

> min sup {𝐻(𝜋, Q) ∶ (Q, P̂ 𝑁 ) ≤ 𝑟} ≡ min sup {𝐻(𝜋, Q) ∶ (Q, P̂ 𝑁 ) ≤
> 𝑟},

driven single item newsvendor and single risky asset portfolio problems

> 𝜋∈𝛱 Q∈(×)
>
> 𝜋∶̂→ Q∈(̂× )

[and](#_bookmark59) establish bounds on the out-of-sample performance.
[Bertsimas](#_bookmark59)

where ̂ ∶= ∪𝑁 {***𝒙*** } and (̂ × ) is the set of all distribution

[and Koduri](#_bookmark59) ([2022](#_bookmark59)) show the asymptotic
optimality of RKHS-based policies. [Notz and Pibernik](#_bookmark170)
([2022](#_bookmark170)) study a two-stage capacity plan- ning problem
with multivariate demand and vector-valued capacity decisions for which
the underlying demand distribution is difficult to estimate in practice.
Similar to [Bazier-Matte and Delage](#_bookmark53)
([2020](#_bookmark53)), the authors optimize over policies that are
linear in the RKHS associated with the Gaussian kernel and identify
generalization error bounds. For large dimensional problems, this kernel
is shown to have a slow convergence rate, and as a result, the authors
propose instead using a data-dependent random forest kernel.

3.  *Non-linear decision rules*

Many non-linear decision rule approaches have been experimented
[with.](#_bookmark226) [Huber et al.](#_bookmark124)
([2019](#_bookmark124)), [Oroojlooyjadid et al.](#_bookmark172)
([2020](#_bookmark172)), and [Zhang](#_bookmark226) [and
Gao](#_bookmark226) ([2017](#_bookmark226)) study the value of training
a DNN to learn the or- dering policy of a newsvendor problem. It is
well-known that neural networks enjoy the universal approximation
property; that is, they can approximate any continuous function
arbitrarily well ([Cybenko](#_bookmark86), [1989](#_bookmark86); [Lu et
al.](#_bookmark150), [2017](#_bookmark150)). For constrained problems,
one can use softmax as the final layer to ensure that decisions lie in a
simplex, e.g., in a portfolio optimization problem ([Zhang et
al.](#_bookmark229), [2021](#_bookmark229)). Yet, in general, the output
of a neural network might not naturally land in the feasible space . To
circumvent this issue, [Chen et al.](#_bookmark75)
([2023](#_bookmark75)) introduce an application-specific differentiable
repair layer that projects the solution back to feasibility. [Rychener
et al.](#_bookmark187) ([2023](#_bookmark187)) show that the decision
rule obtained by using the stochastic gradient descent (SGD) method to
train DNN-based policies approximately minimizes the Bayesian posterior
loss.

Exploiting the fact that the optimal solution of a newsvendor prob- lem
is a quantile of the demand distribution, [Huber et al.](#_bookmark124)
([2019](#_bookmark124)) further train an additive ensemble of decision
trees using quantile regression to produce the ordering decision. They
test these algorithms on a real- world data set from a large German
bakery chain. [Bertsimas et al.](#_bookmark57) ([2019](#_bookmark57)),
[Ciocan and Mišić](#_bookmark80) ([2022](#_bookmark80)), and
[Keshavarz](#_bookmark134) ([2022](#_bookmark134)) optimize de- cision
tree-based decision rules to address the multi-item newsvendor,
treatment planning, and optimal stopping problems, respectively. A
[tutorial](#_bookmark195) on DNN-based decision rule optimization is
given in [Shlezinger](#_bookmark195) [et al.](#_bookmark195)
([2022](#_bookmark195)).

[Zhang et al.](#_bookmark228) ([2023](#_bookmark228)) introduce
piecewise-affine decision rules and provide non-asymptotic and
asymptotic consistency results for uncon- strained and constrained
problems, respectively. The policy is learned through a stochastic
majorization-minimization algorithm, and experi- ments on a constrained
newsvendor problem show that piecewise-affine decision rules can
outperform the RKHS-based policies.

4.  *Distributionally robust decision rules*

> Most of the literature on policy learning assumes a parametric form

𝛱***𝜽*** for the policy. A notable exception is [Zhang et
al.](#_bookmark230) ([2023](#_bookmark230)), which studies a
distributionally robust contextual newsvendor problem under

> supported on ̂ 𝑖=1 . 𝑖

Prior to the work of [Zhang et al.](#_bookmark230)
([2023](#_bookmark230)), many have considered dis- tributionally robust
versions of the decision rule optimization problem [in](#_bookmark60)
the non-contextual setting ([Yanıkoğlu et al.](#_bookmark227),
[2019](#_bookmark227)) while [Bertsimas](#_bookmark60) [et
al.](#_bookmark60) ([2023](#_bookmark60)) use LDRs to solve dynamic
optimization problems with side information.

[Yang et al.](#_bookmark225) ([2023](#_bookmark225)) point out that the
perturbed distributions in the Wasserstein ambiguity set might have a
different conditional in- formation structure than the estimated
conditional distribution. They introduce a distributionally robust
optimization (DRO) problem with causal transport metric ([Backhoff et
al.](#_bookmark48), [2017](#_bookmark48); [Lassalle](#_bookmark139),
[2018](#_bookmark139)) that places an additional causality constraint on
the transport plan com- pared to the Wasserstein metric. Tractable
reformulations of the DRO problem are given under LDRs as well as for
one-dimensional convex cost functions. [Rychener et al.](#_bookmark187)
([2023](#_bookmark187)) present a Bayesian interpretation of decision
rule optimization using SGD and show that their algorithm provides an
unbiased estimate of the worst-case objective function of a DRO problem
as long as a uniqueness condition is satisfied. The authors note that
the Wasserstein ambiguity set violates this condition and thus use the
Kullback--Leibler (KL) divergence ([Kullback & Leibler](#_bookmark138),
[1951](#_bookmark138)) to train the models.

Sequential learning and optimization
------------------------------------

[]{#_bookmark24 .anchor}In reviewing contextual optimization approaches
that are based on SLO, we distinguish two settings: (i) a more
traditional setting where the conditional distribution is learned from
data and used directly in the optimization model, and (ii) a setting
that attempts to produce decisions that are robust to model
misspecification. An overview of the methods presented in this section
is given in [Table 2](#_bookmark25).

1.  *Learning conditional distributions*

Most of the recent literature has employed discrete models for
𝑓***~𝜽~***(***𝒙***). This is first motivated from a computational
perspective by the fact that the CSO Problem ([5](#_bookmark12)) is
easier to solve in this setting. In fact, more often than not, the CSO
under a continuous distribution needs to be first replaced by its SAA to
be solved ([Shapiro et al.](#_bookmark194), [2014](#_bookmark194)). From
a statistical viewpoint, it can also be difficult to assess the
probability of outcomes that are not present in the data set, thus
justifying fixing the support of ***𝒚*** to its observed values.

1.  *Residual-based distribution*

A first approach (found in [Deng & Sen](#_bookmark91),
[2022](#_bookmark91); [Kannan et al.](#_bookmark130),
[2020](#_bookmark130); [Sen & Deng](#_bookmark191),
[2017](#_bookmark191)) is to use the errors of a trained regression
model (i.e., its residuals) to construct conditional distributions. Let
𝑔***𝜽***̂ be a regression model trained to predict the response ***𝒚***
from the covariate ***𝒙***, thus minimizing an estimation error 𝜌 as in
([7](#_bookmark18)). The residual error of sample 𝑖 is given by
***𝝐***~𝑖~ = ***𝒚***~𝑖~ −𝑔***𝜽***̂ (***𝒙***~𝑖~). The set of residuals
measured on the

historical data, {***𝝐***~𝑖~}𝑁 , is then used to form the conditional
distribution,

the type-1 Wasserstein ambiguity set without assuming an explicit
structure on the policy class. The type-𝑝 Wasserstein distance (earth
mover's distance) between distributions P~1~ and P~2~ is given by:

> 𝑓***~𝜽~***(***𝒙***) = PER

𝑁

(***𝒙***) ∶=

> 𝑁 𝑖=1

𝛿proj (𝑔***𝜽***̂ (***𝒙***)+***𝝐***𝑖),

> 𝑊 (P , P ) = inf ( ‖𝑦 𝑝

1

> -- 𝑦 ‖𝑝𝛾(𝑑𝑦 , 𝑑𝑦 ) ,
>
> with proj~~ denoting the projection on the support . The
> residual-based
>
> CSO (**rCSO**) problem is now given by:

 

where 𝛾 is a joint distribution of 𝑦~1~ and 𝑦~2~ with marginals P~1~ and
P~2~.

The distributionally robust model in [Zhang et al.](#_bookmark230)
([2023](#_bookmark230)) avoids the degeneracies of ERM for generic 𝛱 by
defining an optimal ''Shapley''

> ***𝒛***∈

The advantage of residual-based methods is that they can be applied in
conjunction with any trained regression model. [Ban et
al.](#_bookmark50) ([2019](#_bookmark50))

> []{#_bookmark25 .anchor}**Table 2**
>
> [Overview of contextual optimization papers in the SLO
> framework.]{.underline} [Method]{.underline}
> [Regularization]{.underline} [Learning model]{.underline} **rCSO wSAA
> EVB** Reg. CSO DRO General Linear Kernel kNN DT RF
>
> [Hannah et al.](#_bookmark118) ([2010](#_bookmark118)) ✗ ✔ ✗ ✗ ✗ ✗ ✗ ✔
> ✗ ✗ ✗ [Ferreira et al.](#_bookmark109) ([2016](#_bookmark109)) ✗ ✗ ✔ ✗
> ✗ ✗ ✗ ✗ ✗ ✔ ✗ [Ban et al.](#_bookmark50) ([2019](#_bookmark50)) ✔ ✗ ✗
> ✗ ✗ ✗ ✔ ✗ ✗ ✗ ✗ [Chen and Paschalidis](#_bookmark74)
> ([2019](#_bookmark74)) ✗ ✔ ✗ ✗ ✔ ✗ ✗ ✗ ✔ ✗ ✗ [Bertsimas and
> Kallus](#_bookmark58) ([2020](#_bookmark58)) ✗ ✔ ✗ ✗ ✗ ✗ ✔ ✗ ✔ ✔ ✔
> [Kannan et al.](#_bookmark130) ([2020](#_bookmark130)) ✔ ✗ ✗ ✗ ✗ ✔ ✔ ✔
> ✔ ✔ ✔ [Kannan et al.](#_bookmark131) ([2021](#_bookmark131)) ✔ ✗ ✗ ✗ ✔
> ✔ ✔ ✔ ✔ ✔ ✔ [Liu et al.](#_bookmark144) ([2021](#_bookmark144)) ✗ ✗ ✔
> ✗ ✗ ✗ ✔ ✗ ✗ ✔ ✗ [Srivastava et al.](#_bookmark197)
> ([2021](#_bookmark197)) ✗ ✔ ✗ ✔ ✗ ✗ ✗ ✔ ✗ ✗ ✗ [Wang et
> al.](#_bookmark214) ([2021](#_bookmark214)) ✗ ✔ ✗ ✗ ✔ ✗ ✗ ✔ ✗ ✗ ✗
> [Bertsimas and Van Parys](#_bookmark62) ([2022](#_bookmark62)) ✗ ✔ ✗ ✗
> ✔ ✗ ✗ ✔ ✔ ✗ ✗ [Deng and Sen](#_bookmark91) ([2022](#_bookmark91)) ✔ ✗
> ✗ ✗ ✗ ✔ ✔ ✔ ✔ ✔ ✔ [Esteban-Pérez and Morales](#_bookmark103)
> ([2022](#_bookmark103)) ✗ ✔ ✗ ✗ ✔ ✗ ✗ ✔ ✔ ✗ ✗ [Kannan et
> al.](#_bookmark132) ([2022](#_bookmark132)) ✔ ✗ ✗ ✗ ✔ ✔ ✔ ✔ ✔ ✔ ✔ [Lin
> et al.](#_bookmark141) ([2022](#_bookmark141)) ✗ ✔ ✗ ✔ ✗ ✗ ✗ ✗ ✔ ✔ ✔
> [Nguyen et al.](#_bookmark167) ([2021](#_bookmark167)) ✗ ✔ ✗ ✗ ✔ ✗ ✗ ✗
> ✔ ✗ ✗ [Notz and Pibernik](#_bookmark170) ([2022](#_bookmark170)) ✗ ✔ ✗
> ✗ ✗ ✗ ✗ ✔ ✗ ✔ ✗ [Zhu et al.](#_bookmark231) ([2022](#_bookmark231)) ✗
> ✗ ✔ ✗ ✔ ✔ ✔ ✔ ✔ ✔ ✔ [Perakis et al.](#_bookmark174)
> ([2023](#_bookmark174)) ✔ ✗ ✗ ✗ ✔ ✗ ✔ ✗ ✗ ✗ ✗
>
> Note: we distinguish between regularized CSO models (Reg. CSO) and
> DRO-based regularization; an approach is classified as ''General'' if
> its learning model is not restricted to specific classes.

and [Deng and Sen](#_bookmark91) ([2022](#_bookmark91)) build
conditional distributions for two- stage and multi-stage CSO problems
using the residuals obtained from

[1964](#_bookmark163); [Watson](#_bookmark217), [1964](#_bookmark217))
employs a weight function:

> 𝑤KDE(***𝒙***) ∶= 𝐾 ((***𝒙*** − ***𝒙***~𝑖~)∕***𝜽***) )

train the regression model 𝑔***~𝜽~***, and to measure the residuals
***𝝐***~𝑖~. This can lead to an underestimation of the distribution of
the residual error. To remove this bias, [Kannan et al.](#_bookmark130)
([2020](#_bookmark130)) propose a leave-one-out model (also known as
*jackknife*). They measure the residuals as ***𝝐***̃~𝑖~ =

***𝒚***~𝑖~ −𝑔***𝜽***̂−𝑖 (***𝒙***~𝑖~), where ***𝜽***̂−𝑖 is trained using
all the historical data except the

𝑖th sample (***𝒙***~𝑖~, ***𝒚***~𝑖~). This idea can also be applied to
the heteroskedastic

case studied in [Kannan et al.](#_bookmark131) ([2021](#_bookmark131)),
where the following conditional distribution is obtained by first
estimating the conditional covariance

matrix 𝑄̂ (***𝒙***) (a positive definite matrix for almost every ***𝒙***)
and then

> forming the residuals ***𝝐***̂~𝑖~ = \[𝑄̂ (***𝒙***~𝑖~)\]−1(***𝒚***~𝑖~ −
> 𝑔***𝜽***̂ (***𝒙***~𝑖~)):

where 𝐾 is a kernel function and ***𝜽*** denotes its bandwidth
parameter. Different kernel functions can be used, e.g., the Gaussian
kernel defined as 𝐾(***𝜟***) ∝ exp(− ***𝜟*** ^2^). [Hannah et
al.](#_bookmark118) ([2010](#_bookmark118)) also use a Bayesian approach
that exploits the Dirichlet process mixture to assign sample weights.

*Weights based on random forest.* Weights can also be designed based on
random forest regressors ([Bertsimas & Kallus](#_bookmark58),
[2020](#_bookmark58)). In its simplest setting, the weight function of a
decision tree regressor is given by:

> ~𝑡~( ) ∶= 1 \[𝑡(***𝒙***) =  (***𝒙***~𝑖~)\]

 

[1]{.underline} ∑

𝑖 𝑁

𝑗=1

1 \[𝑡(***𝒙***) =  (***𝒙***~𝑗~ )\]

> 𝑓***~𝜽~***(***𝒙***) ∶= 𝑁

2.  []{#bookmark26 .anchor}*Weight-based distribution*

> 𝑖=1

𝛿proj (𝑔***𝜽***̂ (***𝒙***)+𝑄̂ (***𝒙***)***𝝐***̂𝑖 ).

where  (***𝒙***) denotes the terminal node of tree 𝑡 that contains
covari-

ate ***𝒙***. Thus, a decision tree assigns equal weights to all the
historical

samples that end in the same leaf node as ***𝒙***. The random forest
weight

A typical approach for formulating the CSO problem is to assign weights
to the observations of the uncertain parameters in the historical
[data](#_bookmark58) and solving the weighted SAA problem (**wSAA**)
given by [Bertsimas](#_bookmark58)

function generalizes this idea over many random decision trees. Its
weight function is defined as:

> 𝑤^RF^(***𝒙***) ∶= [1]{.underline} ∑ 𝑤^𝑡^(***𝒙***),
>
> (𝚠𝚂𝙰𝙰) min

𝑧∈

> (***𝒛***,
>
> 𝑁

𝑖=1

𝑤~𝑖~(***𝒙***)𝛿***~𝒚~***𝑖 )

. []{#_bookmark27 .anchor}(11)

> where 𝑤𝑡 is the weight function of tree 𝑡. Random forests are typi-
> cally trai^𝑖^ned in order to perform an inference task, e.g.
> regression, or classification, but can also be used and interpreted as
> non-parametric

In this case, the conditional distribution 𝑓***~𝜽~***(***𝒙***) = ∑𝑁
𝑤~𝑖~(***𝒙***)𝛿***~𝒚~*** is fully

conditional density estimators.

determined by the function used to assign a weigh^𝑖=^t ^1^to the
h^𝑖^istorical samples. Different approaches have been proposed to
determine the sample weights with ML methods.

*Weights based on proximity.* Sample weights can be assigned based on
the distance between a covariate ***𝒙*** and each historical sample
***𝒙***~𝑖~. For instance, a 𝑘-nearest neighbor (kNN) estimation gives
equal weight to the 𝑘 closest samples in the data set and zero weight to
all the other samples. That is, 𝑤kNN(***𝒙***) ∶= (1∕𝑘)1\[***𝒙***~𝑖~ ∈
𝑘(***𝒙***)\], where  (***𝒙***)

denotes the set of 𝑘 nearest neighbors of ***𝒙*** and 1\[⋅\] is the
indicator func-

tion. Even though it may appear simple, this non-parametric approach
benefits from asymptotic consistency guarantees on its prescriptive per-
formance. Another method to determine sample weights is to use kernel
[density](#_bookmark197) estimators ([Ban & Rudin](#_bookmark51),
[2019](#_bookmark51); [Hannah et al.](#_bookmark118),
[2010](#_bookmark118); [Srivastava](#_bookmark197) [et
al.](#_bookmark197), [2021](#_bookmark197)). The Nadaraya--Watson (NW)
kernel estimator ([Nadaraya](#_bookmark163),

[Bertsimas and Kallus](#_bookmark58) ([2020](#_bookmark58)) provide
conditions for the asymp- totic optimality and consistency of
prescriptions obtained by solving Problem ([11](#_bookmark27)) with the
weights functions given by kNN, NW kernel estimator, and local linear
regression.

3.  *Expected value-based models*

As described in [Definition](#_bookmark15) [1](#_bookmark15), when the
cost function is linear, the training pipeline of SLO reduces to
conditional mean estimation. For instance, [Ferreira et
al.](#_bookmark109) ([2016](#_bookmark109)) train regression trees to
forecast daily expected sales for different product categories in an
inventory and pricing problem for an online retailer. Alternatively, one
may attempt to approximate the conditional density 𝑓***~𝜽~***(***𝒙***)
using a point prediction

𝑔***~𝜽~***(***𝒙***). For example, [Liu et al.](#_bookmark144)
([2021](#_bookmark144)) study a last-mile delivery problem,

where customer orders are assigned to drivers, and replace the condi-
tional distribution of the stochastic travel time with a point predictor

(e.g. a linear regression or decision tree) that accounts for the number
of stops, total distance of the trip, etc.

2.  *Regularization and distributionally robust optimization*

While non-parametric conditional density estimation methods ben-
[efit](#_bookmark170) from asymptotic consistency ([Bertsimas &
Kallus](#_bookmark58), [2020](#_bookmark58); [Notz &](#_bookmark170)
[Pibernik](#_bookmark170), [2022](#_bookmark170)), they are known to
produce overly optimistic poli- cies when the size of the covariate
vector is large (see discussions in [Bertsimas & Van
Parys](#_bookmark62), [2022](#_bookmark62)). To circumvent this issue,
authors have proposed to either regularize the CSO problem ([Lin et
al.](#_bookmark141), [2022](#_bookmark141); [Srivastava et
al.](#_bookmark197), [2021](#_bookmark197)) or to cast it as a DRO
problem. In the latter case, one attempts to minimize the worst-case
expected cost over the set of distributions  (𝑓 (***𝒙***)) that lie at
a distance 𝑟 from the estimated distribution 𝑓***~𝜽~***(***𝒙***):

> min sup ℎ(***𝒛***, Q~𝑦~). (12)

with

> ̂ ̂

They show how finite-dimensional convex reformulations can be ob- tained
when 𝑔***~𝜽~***(***𝒙***) ∶= ***𝜽***𝑇 ***𝒙***, and promote the use of a
''robustness optimization'' form.

Integrated learning and optimization
------------------------------------

[]{#_bookmark28 .anchor}As discussed previously, ILO is an end-to-end
framework that in- cludes three components in the training pipeline: (i)
a prediction model that maps the covariate to a predicted distribution
(or possibly a point prediction), (ii) an optimization model that takes
as input a prediction and returns a decision, and (iii) a task-based
loss function that captures the downstream optimization problem. The
parameters of the predic- tion model are trained to maximize the
prescriptive performance of the policy, i.e., it is trained on the task
loss incurred by this induced policy

> 𝑧∈ Q𝑦∈𝑟(𝑓***𝜽*** (***𝒙***))

[Bertsimas and Van Parys](#_bookmark62) ([2022](#_bookmark62)) generate
bootstrap data from the training set and use it as a proxy for the
''out-of-sample disappoint- ment'' of an action ***𝒛*** resulting from
the out-of-sample cost exceeding

the budget given by sup~Q~𝑦∈𝑟(𝑓***𝜽*** (***𝒙***)) ℎ(***𝒛***, Q~𝑦~).
They show that for the NW kernel estimator and KNN estimator, the DRO,
under a range of ambi-

guity sets, can be reformulated as a convex optimization problem. Using
KL divergence to measure the distance between the probability distribu-
tions, they obtain guarantees (''bootstrap robustness'') with respect to
the estimate-then-optimize model taking bootstrap data as a proxy for
out-of-sample data. Taking the center of Wasserstein ambiguity set (see
[Kantorovich](#_bookmark214) [& Rubinshtein](#_bookmark133),
[1958](#_bookmark133)) to be NW kernel estimator, [Wang](#_bookmark214)
[et al.](#_bookmark214) ([2021](#_bookmark214)) show that the
distributionally robust newsvendor and conditional value at risk (CVaR)
portfolio optimization problems can be reformulated as convex programs.
They provide conditions to obtain asymptotic convergence and
out-of-sample guarantees on the solutions of the DRO model.

[Chen and Paschalidis](#_bookmark74) ([2019](#_bookmark74)) study a
distributionally robust kNN regression problem by combining point
estimation of the outcome [with](#_bookmark73) a DRO model over a
Wasserstein ambiguity set ([Chen & Pascha-](#_bookmark73)
[lidis](#_bookmark73), [2018](#_bookmark73)) and then using kNN to
predict the outcome based on the weighted distance metric constructed
from the estimates. Extending the methods developed in [Nguyen et
al.](#_bookmark167) ([2021](#_bookmark167)), and [Nguyen et
al.](#_bookmark166) ([2020](#_bookmark166)), study a distributionally
robust contextual portfolio allocation problem where worst-case
conditional return--risk tradeoff is computed over an optimal transport
ambiguity set consisting of perturbations of the joint distribution of
covariates and returns. Their approach generalizes the mean--variance
and mean-CVaR model, for which the distributionally

robust models are shown to be equivalent to semi-definite or second-

rather than the estimation loss.

Next, we discuss several methods for implementing the ILO ap- proach. We
start by describing the different models that are used in ILO (Section
[5.1](#_bookmark29)), and then we present the algorithms used to perform
the training. We divide the algorithms into four categories. Namely,
training using unrolling (Section [5.2](#_bookmark32)), implicit
differentiation (Section [5.3](#_bookmark33)), a surrogate
differentiable loss function (Section [5.4](#_bookmark34)), and a
differentiable optimizer (Section [5.5](#bookmark38)). An overview of
the methods presented in this section is given in [Table
3](#_bookmark30).

1.  *Models*

[]{#_bookmark29 .anchor}[Bengio](#_bookmark54) ([1997](#_bookmark54))
appears to be the first to train a prediction model using a loss that is
influenced by the performance of an action pre- scribed by a conditional
expected value-based decision rule. This was done in the context of
portfolio management, where an investment decision rule exploits a point
prediction of asset returns. Effective wealth accumulation is used to
steer the predictor toward predictions that lead to good investments.
More recent works attempt to integrate a full optimization model, rather
than a rule, into the training pipeline. Next, we summarize how ILO is
applied to the two types of contextual optimization models and introduce
two additional popular task models that have been considered under ILO,
replacing the traditional expected cost task.

*Expected value-based model.* To this date, most of the literature has
considered performing ILO on an expected value-based optimization model.
Namely, following the notation presented in [Definition 1](#_bookmark15)
(Sec- tion [2.2.2](#bookmark11)), this training pipeline is interested
in the loss (***𝜽***) ∶=

order cone representable programs. [Esteban-Pérez and
Morales](#_bookmark103) ([2022](#_bookmark103))

> 𝐻(𝑧∗(⋅, 𝑔***~𝜽~***), P̂ 𝑁 ) = EP̂ \[𝑐(𝑧∗(***𝒙***, 𝑔***~𝜽~***),
> ***𝒚***)\] with 𝑔***~𝜽~***(***𝒙***) as a point predictor

solve a DRO problem with a novel ambiguity set that is based on trim-
ming the empirical conditional distribution, i.e., reducing the weights
over the support points. The authors show the link between trimming a
distribution and partial mass transportation problem, and an interested
reader can refer to [Esteban-Pérez and Morales](#_bookmark104)
([2023](#_bookmark104)) for an application in the optimal power flow
problem.

for ***𝒚***, which we interp^𝑁^ret as a prediction of E\[***𝒚 𝒙***\].
This already raises challenges related to the non-convexity of the
integrated loss function

(***𝜽***) and its differentiation with respect to ***𝜽***:

> 𝑁
>
> ∇ (***𝜽***) = ∇ 𝑐(𝑧^∗^(***𝒙*** , 𝑔 ), ***𝒚*** )
>
> 𝑁 𝑖=1
>
> A distributionally robust extension of the **rCSO** model is presented
>
> 𝑁 𝑑***𝒛*** 𝑑***𝒚***
>
> 𝜕𝑐(𝑧∗(***𝒙*** , 𝑔 ), ***𝒚*** ) 𝜕𝑧∗(***𝒙***𝑖, ***𝒚***̂)

all distributions that lie in the 𝑟 radius of the (Wasserstein)
ambiguity [ ]{.underline}

> 𝑖=1 𝑗=1 𝑘=1
>
> \|***𝒚***̂=𝑔𝜃 (***𝒙***𝑖 )

ball centered at the estimated distribution PER(***𝒙***). [Perakis et
al.](#_bookmark174) ([2023](#_bookmark174))

propose a DRO model to solve a two-stage multi-item joint production

with ^𝜕𝑧^∗(***𝒙***𝑖,***𝒚***̂) as the most problematic evaluation. For
instance, when

𝑧∗(***𝒙*** , 𝑔 𝜕𝑦̂i^𝑘^s the solution of a linear program (LP), it is well
known that

and pricing problem with a partitioned-moment-based ambiguity set

> 𝑖 ***𝜽***)

constructed by clustering the residuals estimated from an additive
demand model.

[Zhu et al.](#_bookmark231) ([2022](#_bookmark231)) considers an
expected value-based model and suggests an ambiguity set that is
informed by the estimation metric used to train 𝑔***𝜽***̂ . Namely, they
consider:

> min sup 𝑐(***𝒛***, 𝑔***~𝜽~***(***𝒙***)),

its gradient is either null or non-existent as it jumps between extreme
points of the feasible polyhedron as the objective is perturbed.

*Conditional distribution-based model.* In the context of learning a
con- ditional distribution model 𝑓~𝜃~ (***𝒙***), [Donti et
al.](#_bookmark94) ([2017](#_bookmark94)) appear to be the first to
study the ILO problem. They model the distribution of the uncertain
parameters using parametric distributions (exponential

***𝒛***∈ ***𝜽***∈ (***𝜽***̂,𝑟)

> and normal). For the newsvendor problem, it is shown that the ILO
>
> []{#_bookmark30 .anchor}**Table 3**
>
> [Overview of contextual optimization papers in the ILO
> framework.]{.underline} [Objective]{.underline} [Feasible
> domain]{.underline} [Training]{.underline} LP QP Convex Non convex
> Integer Uncertain Implicit diff. Surr. loss Surr. optim.
>
> [Amos and Kolter](#_bookmark45) ([2017](#_bookmark45)) ✗ ✔ ✗ ✗ ✗ ✔ ✔ ✗
> ✔ [Donti et al.](#_bookmark94) ([2017](#_bookmark94)) ✗ ✔ ✔ ✗ ✗ ✔ ✔ ✗
> ✗ [Agrawal et al.](#_bookmark42) ([2019](#_bookmark42)) ✗ ✔ ✔ ✗ ✗ ✔ ✔
> ✗ ✔ [Vlastelica et al.](#_bookmark210) ([2019](#_bookmark210)) ✔ ✗ ✗ ✗
> ✔ ✗ ✗ ✔ ✗ [Wilder et al.](#_bookmark218) ([2019](#_bookmark218)) ✔ ✗ ✗
> ✗ ✔ ✗ ✔ ✗ ✗ [Wilder et al.](#_bookmark219) ([2019](#_bookmark219)) ✗ ✔
> ✗ ✗ ✔ ✗ ✗ ✔ ✗ [Berthet et al.](#_bookmark56) ([2020](#_bookmark56)) ✔
> ✗ ✗ ✗ ✔ ✗ ✗ ✗ ✔ [Elmachtoub et al.](#_bookmark102)
> ([2020](#_bookmark102)) ✔ ✗ ✗ ✗ ✗ ✗ ✗ ✔ ✗ [Ferber et
> al.](#_bookmark108) ([2020](#_bookmark108)) ✔ ✗ ✗ ✗ ✔ ✗ ✔ ✗ ✗ [Mandi
> and Guns](#_bookmark152) ([2020](#_bookmark152)) ✔ ✗ ✗ ✗ ✔ ✗ ✔ ✗ ✗
> [Mandi et al.](#_bookmark154) ([2020](#_bookmark154)) ✔ ✗ ✗ ✗ ✔ ✗ ✗ ✔
> ✗ [Grigas et al.](#_bookmark113) ([2021](#_bookmark113)) ✗ ✗ ✔ ✗ ✗ ✗ ✗
> ✗ ✔ [Mulamba et al.](#_bookmark161) ([2021](#_bookmark161)) ✔ ✗ ✗ ✗ ✔
> ✗ ✗ ✗ ✔ [Chung et al.](#_bookmark79) ([2022](#_bookmark79)) ✗ ✗ ✔ ✗ ✔
> ✗ ✗ ✔ ✗ [Cristian et al.](#_bookmark85) ([2022](#_bookmark85)) ✗ ✗ ✔ ✗
> ✗ ✗ ✗ ✗ ✔ [Dalle et al.](#_bookmark87) ([2022](#_bookmark87)) ✔ ✗ ✗ ✗
> ✔ ✗ ✗ ✗ ✔ [Elmachtoub and Grigas](#_bookmark100)
> ([2022](#_bookmark100)) ✔ ✗ ✗ ✗ ✔ ✗ ✗ ✔ ✗ [Jeong et
> al.](#_bookmark127) ([2022](#_bookmark127)) ✔ ✗ ✗ ✗ ✔ ✗ ✗ ✔ ✗ [Kallus
> and Mao](#_bookmark128) ([2022](#_bookmark128)) ✗ ✗ ✔ ✗ ✗ ✗ ✗ ✔ ✗
> [Kong et al.](#_bookmark135) ([2022](#_bookmark135)) ✗ ✔ ✔ ✔ ✔ ✗ ✗ ✗ ✔
> [Lawless and Zhou](#_bookmark140) ([2022](#_bookmark140)) ✔ ✗ ✗ ✗ ✔ ✗
> ✗ ✔ ✗ [Loke et al.](#_bookmark149) ([2022](#_bookmark149)) ✔ ✗ ✗ ✗ ✗ ✗
> ✗ ✔ ✗ [Mandi et al.](#_bookmark151) ([2022](#_bookmark151)) ✔ ✗ ✗ ✗ ✔
> ✗ ✗ ✗ ✔ [Muñoz et al.](#_bookmark162) ([2022](#_bookmark162)) ✔ ✗ ✗ ✗
> ✗ ✗ ✗ ✔ ✗ [Shah et al.](#_bookmark193) ([2022](#_bookmark193)) ✔ ✔ ✔ ✔
> ✔ ✗ ✗ ✗ ✔ [Butler and Kwon](#_bookmark68) ([2023a](#_bookmark68)) ✗ ✔
> ✗ ✗ ✗ ✗ ✔ ✗ ✗ [Costa and Iyengar](#_bookmark84) ([2023](#_bookmark84))
> ✗ ✔ ✔ ✗ ✗ ✔ ✔ ✔ ✗ [Estes and Richard](#_bookmark105)
> ([2023](#_bookmark105)) ✔ ✗ ✔ ✗ ✗ ✗ ✗ ✔ ✗ [Kotary et
> al.](#_bookmark137) ([2023](#_bookmark137)) ✔ ✔ ✔ ✔ ✗ ✗ ✔ ✗ ✗
> [McKenzie et al.](#_bookmark157) ([2023](#_bookmark157)) ✔ ✗ ✗ ✗ ✗ ✗ ✔
> ✗ ✗ [Sun et al.](#_bookmark201) ([2023](#_bookmark201)) ✔ ✗ ✗ ✗ ✗ ✗ ✗
> ✔ ✗ [Sun et al.](#_bookmark203) ([2023](#_bookmark203)) ✗ ✔ ✗ ✔ ✗ ✗ ✔
> ✗ ✗
>
> Notes: an approach has a ''Convex'' objective if it can handle general
> convex objective functions that are not linear or quadratic such as
> convex piecewise-linear objective functions; an ''Uncertain'' feasible
> domain denotes that some constraints are subject to uncertainty.
> Implicit diff., surr. loss and surr. optim. denote implicit
> differentiation, surrogate differentiable loss function and surrogate
> differentiable optimizer, respectively. This table covers papers that
> introduced an ILO framework to solve a class of CSO problems, papers
> focused on deriving theoretical guarantees and specific applications
> are therefore intentionally excluded. The tick marks reflect our
> understanding of the claimed scope of the contributions.

model outperforms decision rule optimization with neural networks and
SLO with maximum likelihood estimation (MLE) when there is model
misspecification. Since then, it has become more common to formulate the
CSO problem as a weighted SAA model (as discussed in Section
[4.1.2](#bookmark26)). The prediction model then amounts to identifying
a vector of weights to assign to each historical sample. This approach
is taken by [Kallus and Mao](#_bookmark128) ([2022](#_bookmark128)), who
train a random forest regressor in an integrated fashion to assign
weights, and by [Grigas et al.](#_bookmark113) ([2021](#_bookmark113)),
who show how to train general differentiable models to predict the
probabilities of an uncertain parameter ***𝒚*** with finite support.

*Optimal action imitation task.* ILO has some connections to inverse
optimization, i.e., the problem of learning the parameters of an opti-
mization model given data about its optimal solution (see [Sun et
al.](#_bookmark201), [2023](#_bookmark201), where both problems are
addressed using the same method). Indeed, one can replace the original
objective of ILO with an objective that seeks to produce a 𝑧∗(***𝒙***,
𝑓***~𝜽~***) that is as close as possible to the optimal hindsight action
and, therefore, closer to the regret objective. Specifically, to learn a
policy that ''imitates'' the optimal hindsight action, one can first
augment the data set with ***𝒛***∗ ∶= 𝑧∗(***𝒙***~𝑖~, ***𝒚***~𝑖~) to get

{(***𝒙*** , ***𝒚*** , ***𝒛***∗)}𝑁 . Thereafter, a prediction model 𝑓 𝑖
is learned in a way

tha𝑖t th𝑖 e a^𝑖^ cti^𝑖=^o^1^n

> ∗(***𝒙*** )

###  ~𝜽~ 𝒙

> ∗ for all samples in
>
> 𝑧 ~𝑖~, 𝑓***~𝜽~*** is as close as possible to ***𝒛***𝑖

*Regret minimization task.* A recent line of work has tackled the ILO
[problem](#_bookmark100) from the point of view of regret. Indeed, in
[Elmachtoub](#_bookmark100)

the training set ([Kong et al.](#_bookmark135), [2022](#_bookmark135)):

> 𝐻~Imitation~(𝜋, P̂ ′ ) ∶= EP̂ ′ \[𝑑(𝜋(***𝒙***), ***𝒛***∗)\] = EP̂
>
> \[𝑑(𝜋(***𝒙***), 𝑧^∗^(***𝒙***, ***𝒚***))\] (14)

[and Grigas](#_bookmark100) ([2022](#_bookmark100)), a contextual point
predictor 𝑔***~𝜽~***(***𝒙***) is learned by

> 𝑁 𝑁 𝑁

minimizing the regret associated with implementing the prescribed

with P̂ ′ as the empirical distribution on the lifted tuple (***𝒙***,
***𝒚***, 𝑧∗(***𝒙***, ***𝒚***))

action based on the mean estimator 𝑔***~𝜽~***(***𝒙***) instead of based
on the realized parameters ***𝒚*** (a.k.a. the optimal hindsight or
wait-and-see decision). Specifically, the value of an expected
value-based policy

𝜋***~𝜽~***(***𝒙***) ∶= 𝑧∗(***𝒙***, 𝑔***~𝜽~***) is measured as the
expected regret defined as:

> 𝐻~Regret~(𝜋***~𝜽~***, P) ∶= E~P~\[𝑐(𝜋***~𝜽~***(***𝒙***), ***𝒚***) −
> 𝑐(𝑧^∗^(***𝒙***, ***𝒚***), ***𝒚***)\]. []{#_bookmark31 .anchor}(13)

Minimizing the expected regret returns the same optimal parameter vector
***𝜽*** as the ILO problem ([9](#_bookmark16)). This is due to the fact
that:

based o^𝑁^n the augmented data set and a distance function 𝑑(***𝒛***,
***𝒛***∗). We note that there is no reason to believe that the best
imitator under a general distance function, e.g., ***𝒛*** − ***𝒛***∗ 2,
performs well under our

original metric 𝐻(𝜋, P̂ 𝑁 ). One exception is for 𝑑(***𝒛***, ***𝒛***∗) ∶=
𝑐(***𝒛***, ***𝒚***) −

𝑐(***𝒛***∗, ***𝒚***), where we allow the distance to also depend on
***𝒚***, for which

we recover the regret minimization approach, and therefore the same
solution as with 𝐻(𝜋, P̂ 𝑁 ). Readers that have an interest in general
inverse optimization methods should consult ([Chan et
al.](#_bookmark71), [2021](#_bookmark71)) for an

extensive recent review of the field.

> 𝐻Regret(𝜋, P̂

~𝑁~ ) = E~P~̂𝑁

\[𝑐(𝜋(***𝒙***), ***𝒚***) − 𝑐(𝑧^∗^(***𝒙***, ***𝒚***), ***𝒚***)\] = 𝐻(𝜋, P̂

~𝑁~ ) − E~P~̂𝑁

\[𝑐(𝑧^∗^(***𝒙***, ***𝒚***), ***𝒚***)\].

2.  *\
    > Training by unrolling*

> []{#_bookmark32 .anchor}An approach to obtain the Jacobian matrix
> [𝜕𝑧∗(***𝒙***,***𝒚***̂]{.underline})
>
> is unrolling

Hence, both 𝐻~Regret~(𝜋, P̂

ers.

~𝑁~ ) and 𝐻(𝜋, P̂

~𝑁~ ) have the same set of minimiz-

([Domke](#_bookmark92), [2012](#_bookmark92)), which involves
approximating the op^𝜕***𝒚***̂^timization prob- lem with an iterative
solver (e.g., first-order gradient-based method).

Each operation is stored on the computational graph, which then allows,
in principle, for computing gradients through classical back-
propagation methods. Unfortunately, this approach requires extensive
amounts of memory. Besides this, the large size of the computational
graph exacerbates the vanishing and exploding gradient problems typ-
ically associated with training neural networks ([Monga et
al.](#_bookmark160), [2021](#_bookmark160)).

3.  *Training using implicit differentiation*

[]{#_bookmark33 .anchor}Implicit differentiation allows for a
memory-efficient backpropaga- tion as opposed to unrolling (we refer to
[Bai et al.](#_bookmark49), [2019](#_bookmark49), for discussion on
training constant memory implicit models using a fixed-point --
[FP](#_bookmark45) -- equation and feedforward networks of infinite
depths). [Amos](#_bookmark45) [and Kolter](#_bookmark45)
([2017](#_bookmark45)) appear to be the first to have employed implicit
differentiation methods to train an ILO model, which they refer to as
**OptNet**. They consider expected value-based optimization models that
take the form of constrained quadratic programs (QP) with equality and
inequality constraints. They show how the implicit function theorem (IFT
-- [Halkin](#_bookmark117), [1974](#_bookmark117)) can be used to
differentiate 𝑧∗(***𝒙***, 𝑔***~𝜽~***) with respect to ***𝜽*** using the
Karush--Kuhn--Tucker (KKT) conditions that are satisfied at optimality.
Further, they provide a custom solver based on a primal-- dual interior
method to simultaneously solve multiple QPs on GPUs in batch form,
permitting 100-times speedups compared to Gurobi and CPLEX. This
approach is extended to conditional stochastic and strongly convex
optimization models in [Donti et al.](#_bookmark94)
([2017](#_bookmark94)). They use sequential quadratic programming
(**SQP**) to obtain quadratic approximations of the objective functions
of the convex program at each iteration until convergence to the
solution and then differentiate the last iteration of **SQP** to obtain
the Jacobian. For a broader view of implicit differenti-
[ation,](#_bookmark97) we refer to the surveys by [Blondel et
al.](#_bookmark65) ([2022](#_bookmark65)) and [Duvenaud](#_bookmark97)
[et al.](#_bookmark97) ([2020](#_bookmark97)).

To solve large-scale QPs with linear equality and box inequality
constraints, [Butler and Kwon](#_bookmark68) ([2023a](#_bookmark68)) use
the ADMM algorithm to decouple the differentiation procedure for primal
and dual variables, thereby decomposing the large problem into smaller
subproblems. Their procedure relies on implicit differentiation of the
FP equations of the alternating direction method of multipliers (ADMM)
algorithm (**ADMM-FP**). They show that unrolling the iterations of the
ADMM algo- rithm on the computational graph ([Sun et
al.](#_bookmark200), [2016](#_bookmark200); [Xie et al.](#_bookmark220),
[2019](#_bookmark220)) results in higher computation time than
**ADMM-FP**. Their empirical results on a portfolio optimization problem
with 254 assets suggest that computational time can be reduced by a
factor of almost five by using **ADMM-FP** compared to **OptNet**,
mostly due to the use of the ADMM [algorithm](#_bookmark68) in the
forward pass. Note that the experiments in [Butler and](#_bookmark68)
[Kwon](#_bookmark68) ([2023a](#_bookmark68)) were conducted on a CPU.

To extend **OptNet** to a broader class of problems, [Agrawal et
al.](#_bookmark42) ([2019](#_bookmark42)) introduce **cvxpylayers** that
relies on converting disciplined convex programs in the domain-specific
language used by CVXPY into conic programs. They implicitly
differentiate the residual map of the homogeneous self-dual embedding
associated with the conic program. [McKenzie et al.](#_bookmark157)
([2023](#_bookmark157)) note that using KKT conditions for con- strained
optimization problems with DNN-based policies is compu- tationally
costly as ''**cvxpylayers** struggles with solving problems containing
more than 100 variables'' (see also [Butler & Kwon](#_bookmark68),
[2023a](#_bookmark68)).

An alternative is to use projected gradient descent (**PGD**) where DNN-

based policies are updated using an iterative solver and projected onto
the constraint set  at each iteration and the associated FP system
([Blondel et al.](#_bookmark65), [2022](#_bookmark65); [Chen et
al.](#_bookmark72), [2021](#_bookmark72); [Donti et al.](#_bookmark95),
[2021](#_bookmark95)) is used to obtain the Jacobian.

Since a closed-form solution for the projection onto  is unavailable in
many cases, the projection step may be costly, and in some cases,

**PGD** may not even converge to a feasible point ([Rychener et
al.](#_bookmark187), [2023](#_bookmark187)). To avoid computing the
projection in the forward pass, [McKenzie et al.](#_bookmark157)

Jacobian matrix is replaced with an identity matrix [Sahoo et
al.](#_bookmark188) (see also [2023](#_bookmark188), where a similar
approach is used for expected value-based models).

To mitigate the issues with unrolling, [Kotary et al.](#_bookmark137)
([2023](#_bookmark137)) propose FP folding (**fold-opt**) that allows
analytically differentiating the FP system of general iterative solvers,
e.g., **ADMM**, **SQP**, and **PGD**. By unfold- ing (i.e., partial
unrolling), some of the steps of unrolling are grouped in analytically
differentiable update function  ∶ R^𝑑^***𝒚*** → R^𝑑^***𝒚*** :

> 𝑧~𝑘+1~(***𝒙***, ***𝒚***̂) =  (𝑧~𝑘~(***𝒙***, ***𝒚***̂), ***𝒚***̂).

Realizing that 𝑧∗(***𝒙***, ***𝒚***̂) is the FP of the above system, they
use the IFT to obtain a linear system (a differential FP condition) that
can be solved to obtain the Jacobian. This effectively decouples the
forward and backward pass enabling the use of black box solvers like
Gurobi for the forward pass while **cvxpylayers** is restricted to
operator splitting solvers like ADMM. An added benefit of using
**fold-opt** is that it can solve non-convex problems. In the case of
portfolio optimization, the authors note that the superior performance
of their model with respect to **cvxpylayers** can be explained by the
precise calculations made in the forward pass by Gurobi.

While speedups can be obtained for sparse problems, [Sun et
al.](#_bookmark203) ([2023](#_bookmark203)) remark that the complexity
associated with differentiating the KKT conditions is cubic in the total
number of decision variables and constraints in general. They propose an
alternating differenti- ation framework (called **Alt-Diff**) to solve
parameterized convex optimization problems with polyhedral constraints
using ADMM that decouples the objective and constraints. This procedure
results in a smaller Jacobian matrix when there are many constraints
since the gradient computations for primal, dual, and slack variables
are done alternatingly. The gradients are shown to converge to those
obtained by differentiating the KKT conditions. The authors employ
trunca- tion of iterations to compensate for the slow convergence of
ADMM when compared to interior-point methods and provide theoretical up-
per bounds on the error in the resulting gradients. **Alt-Diff** is
shown to achieve the same accuracy with truncation and lower computa-
tional time when compared to **cvxpylayers** for an energy generation
scheduling problem.

Motivated by **OptNet**, several extensions have been proposed to solve
linear and combinatorial problems. [Wilder et al.](#_bookmark218)
([2019](#_bookmark218)) solve LP- representable combinatorial
optimization problems and LP relaxations of combinatorial problems
during the training phase. Their model, re- ferred to as **QPTL**
(Quadratic Programming Task Loss), adds a quadratic penalty term to the
objective function of the linear problem. This has two advantages: it
recovers a differentiable linear--quadratic program, and the added term
acts as a regularizer, which might avoid overfitting. To solve a general
mixed-integer LP (MILP), [Ferber et al.](#_bookmark108)
([2020](#_bookmark108)) develop a cutting plane method **MIPaal**, which
adds a given number of cutting planes in the form of constraints
𝑆***𝒛*** ≤ ***𝒔*** to the LP relaxation of the MILP. Instead of adding a
quadratic term, [Mandi and Guns](#_bookmark152) ([2020](#_bookmark152))
propose **IntOpt** based on the interior point method to solve LPs that
adds a log barrier term to the objective function and differentiates the
homogeneous self-dual formulation of the LP. Their experimental analyses
show that this approach performs better on energy cost-aware scheduling
problems than **QPTL**.

[Costa and Iyengar](#_bookmark84) ([2023](#_bookmark84)) introduce an
ILO framework with the weighted average of Sharpe ratio and MSE loss as
a task loss and replace the optimization problem with a surrogate DRO
problem. By using convex duality, they reformulate the minimax problem
as a minimization problem and learn the parameters (e.g., size of
ambiguity set) using implicit differentiation instead of
cross-validation (CV). More specifically, the DRO model uses a deviation
risk measure (e.g., vari- ance) to control variability in the portfolio
returns associated with the prediction errors ***𝝐*** = ***𝒚*** − 𝑔
(***𝒙*** ):

([2023](#_bookmark157)) solve the expected value-based CSO problem using
Davis--Yin

operator splitting ([Davis & Yin](#_bookmark88), [2017](#_bookmark88))
while the backward pass uses

> 𝑖 𝑖

argmin

𝜃

> max
>
> 𝑖
>
> E~Q~ \[(***𝝐***⊤***𝒛*** − E~Q~\[***𝝐***⊤***𝒛***\])2\] ,

where the distribution of errors lies in 𝜙-divergence (e.g., Hellinger
loss with a convex upper bound called **SPO+**:

distance) based ambiguity set ^𝜙^(P̂

~𝑁~ ) = {Q ∶ EP̂ \[𝜙(Q∕P̂ )\] ≤ 𝑟} centered

> 𝑇 𝑇 ∗
>
> 𝑇 ∗
>
> at P̂ 𝑁 = [^\ 1^]{.underline} ∑𝑁 𝛿***~𝝐~*** .
>
> 𝓁SP𝙾+(***𝒚***̂, ***𝒚***) ∶= sup(***𝒚*** − 2***𝒚***̂) ***𝒛*** +
> 2***𝒚***̂ 𝑧 (***𝒙***, ***𝒚***) − ***𝒚*** 𝑧 (***𝒙***, ***𝒚***),

For convex problems, the optimality conditions are given by KKT
conditions, which can be represented as 𝐹 (***𝜽***, ***𝒛***) = 0 where 𝐹
∶ R𝑑***𝒙*** × R𝑑***𝒛*** → R𝑚, where 𝑚 is proportional to the number of
constraints that define . From the classical IFT ([Dontchev et
al.](#_bookmark93), [2009](#_bookmark93)), we know that if

𝐹 is continuously differentiable and the Jacobian matrix with respect to

***𝒛***, denoted by ∇***~𝒛~***𝐹 (***𝜽***, ***𝒛***(***𝜽***)), is
non-singular at the point (***𝜽***̄, ***𝒛***̄), then there exists a
neighborhood around ***𝜽***̄ for which the gradient of the optimal

solution with respect to the parameters is given by:

> 𝜕***𝒛***∗(***𝜽***) = −(∇ 𝐹 (***𝜽***, ***𝒛***(***𝜽***)))−1∇ 𝐹 (***𝜽***,
> ***𝒛***(***𝜽***)).
>
> 𝜕***𝜽***

When the Jacobian matrix ∇***~𝒛~***𝐹 (***𝜽***, ***𝒛***(***𝜽***)) is
singular, classical IFT can- not be applied. This occurs in linear
programs and can also arise in smooth QPs as shown in [Bolte et
al.](#_bookmark67) ([2021](#_bookmark67)). [Bolte et al.](#_bookmark67)
([2021](#_bookmark67)) obtain a generalization of IFT to non-smooth
functions using conservative Jacobians that generalize Clarke Jacobians
([Clarke](#_bookmark81), [1990](#_bookmark81)) for locally Lipschitz
function 𝐹 . They also derive conservative Jacobians for conic
optimization layers ([Agrawal et al.](#_bookmark42),
[2019](#_bookmark42)).

Further, [Bolte et al.](#_bookmark67) ([2021](#_bookmark67)) illustrate
using **cvxpylayers** that in a bilevel program which is a composition
of a quadratic function with the solution map of a linear program,
gradient descent does not converge but gets stuck in a ''limit cycle of
non-critical points'' even though invertibility condition does not hold
only on a set of measure 0 (defined by a line) where the solution map
moves from extreme point to another. As this example illustrates, the
convergence of gradient methods based on IFT can be impacted by the
non-invertibility of the Jacobian matrix and non-smoothness which is
difficult to verify a priori. As a result, research efforts have been
directed toward designing surrogate loss functions and
perturbation-based models for CSO problems that could circumvent the
need to use the IFT.

4.  *Training using a surrogate differentiable loss function*

[]{#_bookmark34 .anchor}As discussed in Section [5.1](#_bookmark29),
minimizing directly the task loss in ([9](#_bookmark16)) or the regret
in ([13](#_bookmark31)) is computationally difficult in most cases. For
instance, the loss may be piecewise-constant as a function of the
parameters of a prediction model and, thus, may have no informa- tive
gradient. To address this issue, several surrogate loss functions with
good properties, e.g., differentiability and convexity, have been
proposed to train ILO models.

1.  ***SPO+***

In CSO problems, [Elmachtoub and Grigas](#_bookmark100)
([2022](#_bookmark100)) first tackle the potential non-uniqueness of
𝑧∗(***𝒙***, 𝑔***~𝜽~***) by introducing a Smart ''Predict, then
Optimize'' (**SPO**) model where the decision-maker chooses to minimize
the empirical average of the regret under the worst-case optimal
solution as defined below:

> (SP𝙾) min max 𝐻~Regret~(𝜋, P̂ 𝑁 ),

which has a closed-form expression for its subgradient

> 2 𝑧^∗^(***𝒙***, ***𝒚***) − 𝑧^∗^(***𝒙***, 2***𝒚***̂ − ***𝒚***) ∈
> ∇~***𝒚***̂~𝓁SP𝙾+(***𝒚***̂, ***𝒚***). []{#_bookmark35 .anchor}(16)

[Loke et al.](#_bookmark149) ([2022](#_bookmark149)) propose a
decision-driven regularization model (**DDR**) that combines prediction
accuracy and decision quality in a single optimization problem with loss
function as follows:

> 𝓁𝙳𝙳(***𝒚***̂, ***𝒚***) = 𝑑(***𝒚***̂, ***𝒚***) − 𝜆 min{𝜇***𝒚***⊤***𝒛***
> + (1 − 𝜇)***𝒚***̂^⊤^***𝒛***},
>
> and **SPO+** being a special case with 𝜇 = −1, 𝜆 = 1, and 𝑑(***𝒚***̂,
> ***𝒚***) = 2***𝒚***̂^⊤^𝑧∗(***𝒙***, ***𝒚***) − ***𝒚***𝑇 𝑧∗(***𝒙***,
> ***𝒚***).

***SPO+** for combinatorial problems.* Evaluating the gradient of
**SPO+** loss in ([16](#_bookmark35)) requires solving the optimization
problem ([8](#_bookmark19)) to obtain 𝑧∗(***𝒙***, 2***𝒚***̂ − ***𝒚***)
for each data point. This can be computationally demanding when the
optimization model in ([8](#_bookmark19)) is an NP-hard problem. [Mandi
et al.](#_bookmark154) ([2020](#_bookmark154)) propose a **SPO-relax**
approach that computes the gradient of **SPO+** loss by solving instead
a continuous relaxation when ([8](#_bookmark19)) is a MILP. They also
suggest speeding up the resolution using a warm-start for learning with
a pre-trained model that uses MSE as the loss function. Another way
proposed to speed up the computation is warm-starting the solver. For
example, 𝑧∗(***𝒙***, ***𝒚***) can be used as a starting point for MILP
solvers or to cut away a large part of the feasible space. [Mandi et
al.](#_bookmark154) ([2020](#_bookmark154)) show that for weighted and
unweighted knapsack problems as well as energy-cost aware scheduling
problems, **SPO-relax** results in faster convergence and similar
performance compared to **SPO+** loss. Also, **SPO-relax** provides low
regret solutions and faster convergence compared to **QPTL** in the
aforementioned three problems, except in the weighted knapsack problem
with low capacity.

With a focus on exact solution approaches, [Jeong et al.](#_bookmark127)
([2022](#_bookmark127)) study the problem of minimizing the regret in
([13](#_bookmark31)) assuming a linear predic-

> tion model 𝑔***~𝜽~***(***𝒙***) = ***𝜽𝒙*** with ***𝜽*** ∈ R𝑑***𝒛***
> ×𝑑***𝒙*** . Under the assumption that

𝑧∗(***𝒙***, 𝑔***~𝜽~***) is unique for all ***𝜽*** and ***𝒙***, the
authors reformulate the bilevel

**SPO** problem as a single-level MILP using symbolic variable
elimination.

They show that their model can achieve up to two orders of magnitude
improvement in expected regret compared to **SPO+** on the training set.
[Muñoz et al.](#_bookmark162) ([2022](#_bookmark162)) applies a similar
idea of representing the set of optimal solutions with a MILP. They rely
on the KKT conditions of the problem defining 𝑧∗(***𝒙***, 𝑔***~𝜽~***) to
transform the bilevel integrated problem into a single-level MILP.
Finally, [Estes and Richard](#_bookmark105) ([2023](#_bookmark105)) use
the **SPO** loss function to solve a two-stage LP with right-hand side
uncertainty. They propose a lexicographical ordering rule to select the
minimal solution when there are multiple optima and approximate the
resulting piecewise-linear loss function, **lex-SPO**, by a convex
surrogate to find the point predictor.

***SPO** trees.* [Elmachtoub et al.](#_bookmark102)
([2020](#_bookmark102)) propose a model (**SPOT**) to con- struct
decision trees that segment the covariates based on the **SPO** loss
function while retaining the interpretability in the end-to-end learning
framework. Their model outperforms classification and regression trees

***𝜽*** 𝜋

> s.t. 𝜋(***𝒙***) ∈ argmin 𝑐(***𝒛***, 𝑔***~𝜽~***(***𝒙***)), ∀***𝒙***.

***𝒛***∈

\(15\)

> (CART) in the numerical experiments on a news recommendation prob-
>
> lem using a real-world data set and on the shortest path problem with
> synthetic data (also used in [Elmachtoub and Grigas](#_bookmark100)
> ([2022](#_bookmark100))).

In the expected value-based model, they show that the **SPO** objective

reduces to training the prediction model according to the ERM problem:

*Guarantees.* [Elmachtoub and Grigas](#_bookmark100)
([2022](#_bookmark100)) show that under certain

with:

> ***𝜽***⋆ ∈ argmin 𝜌
>
> ***𝜽***

SP𝙾

(𝑔***~𝜽~***, P̂

~𝑁~ ) ∶= E

P̂ 𝑁

\[𝓁SP𝙾

(𝑔***~𝜽~***(***𝒙***), ***𝒚***)\],

conditions, the minimizers of the **SPO** loss, **SPO+** loss and MSE
loss are almost always equal to E ~(~ ~)~\[***𝒚***\] given that E ~(~
~)~\[***𝒚***\] ∈ . Thus, **SPO+** is Fisher consistent with respect to
the **SPO** loss. This means that [minimizing](#_bookmark121) the
surrogate loss also minimizes the true loss function.
[Ho-](#_bookmark121)

> 𝓁SP𝙾(***𝒚***̂, ***𝒚***) ∶= sup
>
> ***𝒛***̄∈argmin***𝒛***∈ 𝑐(***𝒛***,***𝒚***̂)

𝑐(***𝒛***̄, ***𝒚***) − 𝑐(𝑧^∗^(***𝒙***, ***𝒚***), ***𝒚***).

> [Nguyen and Kılınç-Karzan](#_bookmark121) ([2022](#_bookmark121)) show
> that for some examples of a multiclass classification problem,
> **SPO+** is Fisher inconsistent, while

Since the **SPO** loss function is nonconvex and discontinuous in
***𝒚***̂ ([Ho-](#_bookmark121) [Nguyen & Kılınç-Karzan](#_bookmark121),
[2022](#_bookmark121), Lemma 1), [Elmachtoub and Grigas](#_bookmark100)
([2022](#_bookmark100)) focus on the linear objective 𝑐(***𝒛***,
***𝒚***) ∶= ***𝒚***𝑇 ***𝒛*** and replace the **SPO**

MSE loss is consistent. However, complete knowledge of the distribu-
tion is a limitation in practice where the decision-maker has access to
[only](#_bookmark121) the samples from the distribution. As a result,
[Ho-Nguyen and](#_bookmark121)

[Kılınç-Karzan](#_bookmark121) ([2022](#_bookmark121)) and [Liu and
Grigas](#_bookmark142) ([2021](#_bookmark142)) provide calibration
realization of the uncertain parameter, they obtain the conditional

bounds that hold for a class of distributions  on  ×  and ensure

distribution of uncertain parameter, 𝑓***~𝜽~***

> = [1]{.underline} ∑𝑇
>
> 𝛿***𝒚***̂𝑡 , where ***𝒚***̂^𝑡^ is the

translates to lower excess **SPO** risk.

> 𝑖 ***𝜽***0

they obtain an optimal allocation, 𝑧^𝑗^ = 𝑧∗(***𝒙***, 𝑓 ) for facility 𝑗

In many ML applications, one seeks to derive finite-sample guaran- tees,
which are given in the form of a generalization bound, i.e., an

minimizes the average unmet demand^𝑖^. In the last s^0^tep, they retrain
the random forest to minimize the reweighted MSE loss function:

upper bound on the difference between the true risk of a loss function

> 𝑁 𝑀 𝑇

and its empirical risk estimate for a given sample size 𝑁. A general-

min ∑ ∑ ∑ 1 \[𝑦̂^𝑡,𝑗^ ≥ 𝑠^𝑗^ + 𝑧^𝑗^ \] \|𝑓***~𝜽~***(***𝒙***~𝑖~) − 𝑦̂^𝑡,𝑗^
\|, []{#_bookmark36 .anchor}(17)

([2022](#_bookmark99)) (extension of [El Balghiti et al.](#_bookmark98),
[2019](#_bookmark98)) based on Rademacher

complexity of the **SPO** loss composed with the prediction functions

𝑔***~𝜽~*** ∈ . More [ speci]{.underline}fically, the bound achieved in
[El Balghiti et al.](#_bookmark98)

> [log(𝑁]{.underline})
>
> 𝑁

feature dimension are obtained using **SPO** function's structure and if


satisfies a ''strength'' property. [Hu et al.](#_bookmark123)
([2022](#_bookmark123)) show that for linear CSO p[ro]{.underline}blems,
the generalization bound for MSE loss and **SPO** loss is O [
1]{.underline} while faster convergence rates for the SLO model compared

[to](#_bookmark101) ILO model are obtained under certain low-noise
assumptions. [El-](#_bookmark101)

where 𝑀 is the number of facilities, 𝑦̂^𝑡,𝑗^ and 𝑠^𝑗^ denote the demand
and inventory levels, respectively, at fac^𝑖^ility 𝑗. T^𝑖^he above model
([17](#_bookmark36)) solves the optimization problem once during
training, and is shown to be scalable for a medical allocation problem
in Sierra Leone when compared to [Kallus and Mao](#_bookmark128)
([2022](#_bookmark128)) where splitting of the feature space is done
based on the task loss.

[Lawless and Zhou](#_bookmark140) ([2022](#_bookmark140)) introduce a
loss function similar to [Chung](#_bookmark79) [et al.](#_bookmark79)
([2022](#_bookmark79)) that weighs the prediction error with a regret
term as follows:

> ∗ ∗ 2

[machtoub et al.](#_bookmark101) ([2023](#_bookmark101)) show that for
non-linear optimization problems, SLO models stochastically dominate ILO
in terms of their asymptotic optimality gaps when the hypothesis class
covers the true distribution. When the model is misspecified, they show
that ILO outperforms SLO asymptotically in a general nonlinear setting.

2.  *Surrogate loss for a stochastic forest*

[Kallus and Mao](#_bookmark128) ([2022](#_bookmark128)) propose an
algorithm called **StochOpt Forest**, which generalizes the
random-forest based local parameter estimation procedure in [Athey et
al.](#_bookmark47) ([2019](#_bookmark47)). A second-order perturba- tion
analysis of stochastic optimization problems allows them to scale to
larger CSO problems since they can avoid solving an optimization problem
at each candidate split. The policies obtained using their model are
shown to be asymptotically consistent, and the benefit of ILO is
illustrated by comparing their approach to the random forests of
[Bertsimas and Kallus](#_bookmark58) ([2020](#_bookmark58)) on a set of
problems with synthetic and real-world data.

3.  *Other surrogates*

[Wilder et al.](#_bookmark219) ([2019](#_bookmark219)) introduce
**ClusterNet** to solve hard combina- torial graph optimization problems
by learning incomplete graphs. The model combines graph convolution
networks to embed the graphs in a continuous space and uses a soft
version of k-means clustering to obtain a differential proxy for the
combinatorial problems, e.g., community detection and facility location.
Numerical experiments on a synthetic data set show that **ClusterNet**
outperforms the two-stage SLO ap- proach of first learning the graph and
then optimizing, as well as other baselines used in community detection
and facility location.

Focusing on combinatorial problems, [Vlastelica et al.](#_bookmark210)
([2019](#_bookmark210)) pro- pose a differentiable black-box (**DBB**)
approach to tackle the issue that

the Jacobian of 𝑧∗(***𝒙***, 𝑔***~𝜽~***) is zero almost everywhere by
approximating

the true loss function using an interpolation controlled in a way that
balances between ''informativeness of the gradient'' and ''faithfulness
to the original function''. Algorithmically, this is done by perturbing

the prediction 𝑔***~𝜽~***(***𝒙***) in the direction
∇***~𝒛~***𝑐(𝑧∗(***𝒙***, 𝑔***~𝜽~***), ***𝒚***) and obtaining a

gradient of the surrogate loss based on the effect of this perturbation
on the resulting perturbed action.

[Chung et al.](#_bookmark79) ([2022](#_bookmark79)) introduce a
computationally tractable ILO model to solve non-linear CSO problems.
Using the first-order Taylor expansion of the task loss around the
prediction, they introduce a

reweighted MSE loss function where weights are determined by taking

> 𝑑(𝑔***~𝜽~***(***𝒙***), ***𝒚***) = \[𝑐(𝑧 (***𝒙***, 𝑔***~𝜽~***),
> ***𝒚***) − 𝑐(𝑧 (***𝒙***, ***𝒚***), ***𝒚***)\](***𝒚*** −
> 𝑔***~𝜽~***(***𝒙***)) . []{#_bookmark37 .anchor}(18)

Learning optimal ***𝜽*** from the above formulation involves an argmin
differentiation. So, the authors provide a two-step polynomial time
algorithm to approximately solve the above problem. It first computes

a pilot estimator 𝑔***~𝜽~***0 by solving ([7](#_bookmark18)) with
𝑑(𝑔***~𝜽~***(***𝒙***), ***𝒚***) = (𝑔***~𝜽~***(***𝒙***) − ***𝒚***)2 and
then solving ([7](#_bookmark18)) with the distance function in
([18](#_bookmark37)) where 𝑐(𝑧∗(***𝒙***, 𝑔***~𝜽~***), ***𝒚***) is
substituted with 𝑐(𝑧∗(***𝒙***, 𝑔***~𝜽~***0 ), ***𝒚***). The authors show
that their simple algorithm performs comparably to **SPO+**.

We conclude this subsection on surrogate loss functions by mention- ing
the efforts in [Sun et al.](#_bookmark201) ([2023](#_bookmark201)) to
learn a cost point estimator (in an expected value-based model) to
imitate the hindsight optimal solution. This is done by designing a
surrogate loss function that penalizes how much the optimal basis
optimality conditions are violated. They derive generalization error
bounds for this new loss function and employ them to provide a bound on
the sub-optimality of the minimal ***𝜽***.

5.  *Training using a surrogate differentiable optimizer*

    1.  []{#bookmark38 .anchor}*Differentiable perturbed optimizer*

One way of obtaining a differentiable optimizer is to apply a stochastic
perturbation to the parameters predicted by the ML model. Taking the
case of expected value-based models as an example, the key idea is that
although the gradient of the solution of the contextual problem with
respect to the predicted parameters ***𝒚***̂ ∶= 𝑔***~𝜽~***(***𝒙***) is
zero almost everywhere, if we perturb the predictor using a noise with
differentiable density, then the expectation of the solution of the
perturbed contextual problem,

> 𝑧̄^𝜀^(***𝒙***, 𝑔***~𝜽~***) = E~𝛹~ \[𝑧̃^𝜀^(***𝒙***, 𝑔***~𝜽~***, 𝛹 )\]
> with 𝑧̃^𝜀^(***𝒙***, 𝑔***~𝜽~***, 𝛹 ) ∶= argmin 𝑐(***𝒛***,
> 𝑔***~𝜽~***(***𝒙***) + 𝜀𝛹 ),

***𝒛***∈

where 𝜀 \> 0 controls the amount of perturbation, and more generally of
the expected cost of the associated random policy E~𝛹~ \[𝐻(𝑧̃𝜀(⋅,
𝑔***~𝜽~***, 𝛹 ), P̂ 𝑁 )\] can be shown to be smooth and differentiable.
This idea is pro-

posed and exploited in [Berthet et al.](#_bookmark56)
([2020](#_bookmark56)), which focus on a bi- linear cost 𝑐(***𝒛***,
***𝒚***) ∶= ***𝒚***𝑇 ***𝒛*** thus simplifying E~𝛹~ \[𝐻(𝑧̃𝜀(⋅, 𝑔***~𝜽~***,
𝛹 ), P̂ 𝑁 )\] =

𝐻(𝑧̄𝜀(⋅, 𝑔***~𝜽~***), P̂ 𝑁 ). Further, they show that when an imitation
ILO model

is used with a special form of Bregman divergence to capture the
difference between 𝑧∗(***𝒙***, ***𝒚***) and 𝑧̃𝜀(***𝒙***, ***𝒚***̂, 𝛹 ),
the gradient of 𝐻~Imitation~ (𝑧̃𝜀(⋅, 𝑔***~𝜽~***, 𝛹 ), P̂ ′ ) can be
computed directly without needing to determine

the Jacobian ^𝑁^of 𝑧̄𝜀(***𝒙***, 𝑔 ) ([Blondel et al.](#_bookmark66),
[2020](#_bookmark66)):

> 𝐻~Imitation~(𝑧̃^𝜀^(⋅, 𝑔***~𝜽~***, 𝛹 ), P̂ ′ ) ∶= EP̂
> \[𝓁P𝙵𝙻(𝑔***~𝜽~***(***𝒙***), ***𝒚***)\]

the gradient of task loss with respect to the prediction. To solve a
large-scale multi-facility inventory allocation problem with few samples
for each facility, they use a single random forest that can predict the
demand across facilities and products. Assuming that each tree in the
random forest provides an independent and identically distributed

where 𝓁P𝙵𝙻 is a perturbed Fenchel--Young loss (**PFYL**) given by:

> 𝓁P𝙵𝙻(***𝒚***̂, ***𝒚***) ∶= ***𝒚***̂^𝑇^ 𝑧^∗^(***𝒙***, ***𝒚***) − E~𝛹~
> \[(***𝒚***̂ + 𝜀𝛹 )𝑇 ***𝒛***̃^𝜀^(***𝒙***, ***𝒚***̂, 𝛹 )\] +
> 𝜀𝛺~P𝙵𝙻~(𝑧^∗^(***𝒙***, ***𝒚***)),

and 𝛺~P𝙵𝙻~(***𝒛***) is the Fenchel dual of 𝐹 (***𝒚***) ∶= −E~𝛹~
\[(***𝒚***+𝛹)𝑇 𝑧̃𝜀(***𝒙***, ***𝒚***, 𝛹 )\]. The gradient of the
Fenchel--Young loss with respect to the model prediction

is given by:

> ∇~***𝒚***̂~𝓁P(***𝒚***̂, ***𝒚***) = 𝑧^∗^(***𝒙***, ***𝒚***) −
> ***𝒛***̄^𝜀^(***𝒙***, ***𝒚***̂).

The regularization ensures uniqueness and Lipschitz property of
𝑧∗(***𝒙***,

𝑓***~𝜽~***) with respect to 𝑓***~𝜽~*** and leads to finite-sample
guarantees. To cir-

Evaluating this gradient can be done through Monte Carlo evaluations by
sampling perturbations and solving the corresponding perturbed problems.

[Dalle et al.](#_bookmark87) ([2022](#_bookmark87)) introduce a
multiplicative perturbation with the advantage that it preserves the
sign of 𝑔***~𝜽~***(***𝒙***) without adding any bias:

> ***𝒛***̃^𝜀^(***𝒙***, 𝑔***~𝜽~***, 𝛹 ) ∶= argmin 𝑐(***𝒛***,
> 𝑔***~𝜽~***(***𝒙***) ⊙ exp(𝜀𝛹 − 𝜀^2^∕2)),
>
> ***𝒛***∈

where ⊙ is the Hadamard dot-product and the exponential is taken
elementwise. [Dalle et al.](#_bookmark87) ([2022](#_bookmark87)) and
[Sun et al.](#_bookmark198) ([2023](#_bookmark198)) also show that there
is a one-to-one equivalence between the perturbed optimizer approach and
using a regularized randomized version of the CSO problem for
combinatorial problems with linear objective functions. Finally, [Dalle
et al.](#_bookmark87) ([2022](#_bookmark87)) show an intimate connection
between the perturbed minimizer approach proposed by [Berthet et
al.](#_bookmark56) ([2020](#_bookmark56)) and surrogate loss functions
approaches such as **SPO+** by casting them as special cases of a more
general surrogate loss formulation.

[Kong et al.](#_bookmark135) ([2022](#_bookmark135)) and [Mulamba et
al.](#_bookmark161) ([2021](#_bookmark161)) consider an ''energy-
based'' perturbed optimizer defined by its density of the form:

> ***𝒛***̃𝜀(***𝒙***, 𝑓 ) ∼ [ exp(−ℎ(***𝒛***,
> 𝑓***~𝜽~***(***𝒙***))∕𝜀)]{.underline} , []{#_bookmark39 .anchor}(19)

cumvent the challenge associated with non-differentiability of
𝑧∗(***𝒙***, 𝑓***~𝜽~***)

with respect to ***𝜽***, they replace 𝑧∗(***𝒙***, 𝑓 ) with a smooth
approxi^𝜆^mation

𝑧̃~𝜆~(***𝒙***, 𝑓***~𝜽~***) that is learned using a random data set
(***𝒑***~𝑖~, ***𝒛***~𝑖~) generated by sampling ***𝒑***~𝑖~ from the
probability simplex over the discrete support and then finding the
optimal solution ***𝒛***~𝑖~. They show asymptotic optimality and
consistency of their solutions when the hypothesis class is well-

specified. They compare their approach to other ILO pipelines and to the
SLO approach that estimates the conditional distribution using
cross-entropy.

> [Cristian et al.](#_bookmark85) ([2022](#_bookmark85)) introduce the
> **ProjectNet** model to solve

uncertain constrained linear programs in an end-to-end framework by
training an optimal policy network, which employs a differentiable
approximation of the step of projection to feasibility.

Another approach, related to [Berthet et al.](#_bookmark56)
([2020](#_bookmark56)), that generalizes beyond LPs is given in [Shah et
al.](#_bookmark193) ([2022](#_bookmark193)) that constructs locally
optimized decision losses (**LODL**) with supervised learning to
directly evaluate the performance of the predictors on the downstream
op- timization task. To learn a convex **LODL** for each data point,
this

approach first generates labels in the neighborhood of label ***𝒚***~𝑖~
in the

training set, e.g., by adding Gaussian noise, and then chooses the

> ***^𝜽^*** ∫ exp(−ℎ(***𝒛***′, 𝑓***~𝜽~***(***𝒙***))∕𝜀)𝑑***𝒛***′
>
> parameter that minimizes the MSE between **LODL** and the downstream

with 𝜀 = 1, in the context of an imitation ILO problem. This general
form of perturbed optimizer captures a varying amount of perturbation
through 𝜀, with ***𝒛***̃𝜀(***𝒙***, 𝑓***~𝜽~***) converging in
distribution to 𝑧∗(***𝒙***, 𝑓***~𝜽~***) as 𝜀 goes to zero. They employ
the negative log-likelihood to measure the di- vergence between
***𝒛***̃𝜀(***𝒙***, 𝑓***~𝜽~***) and the hindsight optimal solution
𝑧∗(***𝒙***, ***𝒚***). Given the difficulties associated with calculating
the partition function

in the denominator of ([19](#_bookmark39)), [Mulamba et
al.](#_bookmark161) ([2021](#_bookmark161)) devise a surrogate loss
function based on noise-contrastive estimation, which replaces
likelihood with relative likelihood when compared to a set of sampled
suboptimal solutions. This scheme is shown to improve the perfor- mance
over **SPO+** and **DBB** in terms of expected regret performance for
linear combinatorial CSO.

Based on the noise contrastive estimation approach of
[Mulamba](#_bookmark161) [et al.](#_bookmark161)
([2021](#_bookmark161)), [Mandi et al.](#_bookmark151)
([2022](#_bookmark151)) note that ILO for combinatorial problems can be
viewed as a learning-to-rank problem. They propose surrogate loss
functions, with closed-form expressions for gradients, that are used to
train to rank feasible points in terms of performance on the downstream
optimization problem. Unlike ([Mulamba et al.](#_bookmark161),
[2021](#_bookmark161)), [Kong et al.](#_bookmark135)
([2022](#_bookmark135)) tackles the partition function challenge by
employing a self-normalized importance sampler that provides a discrete
approximation. To avoid overfitting, the authors also intro- duce a
regularization that penalizes the KL divergence between the perturbed
optimizer distribution and a subjective posterior distribution

over perturbed optimal hindsight actions P(***𝒛***̃𝜀(***𝒙***,
***𝒚***)\|***𝒚***):

task loss. The **LODL** is used in place of the task-specific surrogate
optimization layers and outperforms SLO on three resource allocation
problems (linear top-1 item selection problem, web advertising, and
portfolio optimization). The numerical experiments indicate that hand-
crafted surrogate functions only perform better for the web advertising
problem.

6.  *Applications*

In this subsection, we discuss the applications of the ILO framework to
a wide range of real-world problems.

> [Tian et al.](#_bookmark206) ([2023](#_bookmark206)) and [Tian et
> al.](#_bookmark207) ([2023](#_bookmark207)) use **SPOT** and noise-

contrastive estimation method ([Mulamba et al.](#_bookmark161),
[2021](#_bookmark161)), respectively, to solve the maritime
transportation problem. A comprehensive tu- [torial](#_bookmark208) on
prescriptive analytics methods for logistics is given in
[Tian](#_bookmark208) [et](#_bookmark78) [al.](#_bookmark208)
([2023](#_bookmark208)). **SPO** has been used in solving last-mile
delivery ([Chu](#_bookmark78) [et al.](#_bookmark78),
[2023](#_bookmark78)) and ship inspection problems ([Yan et
al.](#_bookmark222), [2021](#_bookmark222), [2020](#_bookmark223),
[2023](#_bookmark224)). [Demirović et al.](#_bookmark89)
([2019](#_bookmark89)) and [Demirović et al.](#_bookmark90)
([2020](#_bookmark90)) mini- mize the same expected regret as **SPO**
for specific applications re- lated to ranking optimization and dynamic
programming problems, respectively.

[Perrault et al.](#_bookmark175) ([2020](#_bookmark175)) solve a
Stackelberg security game with the ILO framework by learning the attack
probability distribution over a discrete set of targets to maximize a
surrogate for the defender's expected utility. They show that their
model results in higher expected

> 𝐻~Imitation~(***𝒛***̃^𝜀^(⋅, 𝑓***~𝜽~***), P̂ ′ ) ∶= − EP̂
> \[log(P(***𝒛***̃^𝜀^(***𝒙***, 𝑓***~𝜽~***) = 𝑧^∗^(***𝒙***,
> ***𝒚***))\|***𝒙***, ***𝒚***)\]
>
> utility for the defender on synthetic and human subjects data than the
>
> sequential models that learn the attack probability by minimizing the
>
> \+ 𝜆EP̂ 𝑁 \[KL(P(***𝒛***̃ (***𝒙***, ***𝒚***)\|***𝒚***)‖***𝒛***̃
> (***𝒙***, 𝑓***~𝜽~***)\|***𝒙***, ***𝒚***)\].
>
> 𝜀 𝜀
>
> cross entropy loss. [Wang et al.](#_bookmark216)
> ([2020](#_bookmark216)) replace the large-scale optimiza-

The authors show that their model outperforms ILO trained using **SQP**

and **cvxpylayers** in terms of computational time and gives lower task
loss than sequential models trained using MLE and policy learning with
neural networks.

> *5.5.2. Supervised learning*

[Grigas et al.](#_bookmark113) ([2021](#_bookmark113)) solve a CSO
problem with a convex and non- negative decision regularizer 𝛺(***𝒛***)
assuming that the uncertain param- eter ***𝒚*** has discrete support.
Their model, called **ICEO**-𝜆, is thus trained

by solving:

tion problem with a low dimensional surrogate by reparameterizing the

feasible space of decisions. They observe significant performance im-

provements for non-convex problems compared to the strongly convex case.

[Stratigakos et al.](#_bookmark199) ([2022](#_bookmark199)) solve an
integrated forecasting and opti- mization model for trading in renewable
energy that trains an ensemble

of prescriptive trees by randomly splitting the feature space  based

on the task-specific cost function. [Sang et al.](#_bookmark189)
([2022](#_bookmark189)) introduce an ILO framework for electricity price
prediction for energy storage system arbitrage. They present a hybrid
loss function to measure prediction

and decision errors and a hybrid stochastic gradient descent learning

> (I𝙲E−𝜆) min

***𝜽***

> 𝐻(𝑧^∗^(⋅, 𝑓***~𝜽~***), P̂ 𝑁 ) + 𝜆EP̂

\[𝛺(𝑧^∗^(***𝒙***, 𝑓***~𝜽~***))\] (20a)

> method. [Sang et al.](#_bookmark190) ([2023](#_bookmark190)) solve a
> voltage regulation problem using a
>
> similar hybrid loss function, and backpropagation is done by implic-
>
> s.t. 𝑧^∗^(***𝒙***, 𝑓***~𝜽~***) = argmin 𝑐(***𝒛***,
> 𝑓***~𝜽~***(***𝒙***)) + 𝜆𝛺(***𝒛***), ∀***𝒙***.
>
> itly differentiating the optimality conditions of a second-order cone
>
> 𝜆 ***𝒛***

(20b)

> program.

[Liu et al.](#_bookmark146) ([2023](#_bookmark146)) use a DNN to model
the routing behavior of users in a transportation network and learn the
parameters by minimizing the mismatch between the flow prescribed by the
variational inequality and the observed flow. The backward pass is
obtained by applying the IFT to the variational inequality. [Wahdany et
al.](#_bookmark213) ([2023](#_bookmark213)) propose an integrated model
for wind-power forecasting that learns the parameters of a neural
network to optimize the energy system costs under the system
constraints. [Vohra et al.](#_bookmark211) ([2023](#_bookmark211)) apply
similar ideas to develop end-to- end renewable energy generation
forecasts, using multiple contextual sources such as satellite images
and meteorological time series.

[Butler and Kwon](#_bookmark69) ([2023b](#_bookmark69)) solves the
contextual mean--variance port- folio (MVP) optimization problem by
learning the parameters of the linear prediction model using the ILO
framework. The covariance ma- trix is estimated using the exponentially
weighted moving average model. They provide analytical solutions to
unconstrained and equality- constrained MVP optimization problems and
show that they outperform SLO models based on ordinary least squares
regression. These analytical solutions lead to lower variance when
compared with the exact so- lutions of the corresponding
inequality-constrained MVP optimization problem.

Active research directions
--------------------------

[]{#_bookmark40 .anchor}We now summarize active and future research
directions for further work in contextual optimization.

*Uncertainty in constraints.* Most studies on contextual optimization
as- sume that there is no uncertainty in the constraints. If constraints
are also uncertain, the SAA solutions that ignore the covariates
informa- [tion](#_bookmark58) might not be feasible ([Rahimian &
Pagnoncelli](#_bookmark180), [2022](#_bookmark180)).
[Bertsimas](#_bookmark58) [and Kallus](#_bookmark58)
([2020](#_bookmark58)) have highlighted the challenges in using ERM in a
constrained CSO problem. [Rahimian and Pagnoncelli](#_bookmark180)
([2022](#_bookmark180)) solve a conditional chance-constrained program
that ensures with a high probability that the solution remains feasible
under the conditional distribution given the realized covariates.
Although they do not focus on contextual optimization, interesting links
can be found with the literature on constraint learning ([Fajemisin et
al.](#_bookmark106), [2023](#_bookmark106)) and inverse optimization
([Chan et al.](#_bookmark71), [2021](#_bookmark71)).

*Risk aversion.* There has been a growing interest in studying
contextual optimization in the risk-averse setting. Specifically, one
can consider replacing the risk-neutral expectation from
([1](#_bookmark7)) with a risk measure such as value-at-risk. By doing
so, one would expect, with a high probability, that a decision-maker's
loss is lower than a particular threshold. One can easily represent such
a risk measure using an uncertainty set which represents the set of all
possible outcomes that may occur in the future. The resulting
uncertainty set should be carefully chosen. It should capture the most
relevant scenarios to balance the trade-off between avoiding risks and
obtaining returns. The recently proposed Conditional Robust Optimization
(CRO) paradigm, pioneered by [Chenreddy et al.](#_bookmark76)
([2022](#_bookmark76)) (see also [Ohmori](#_bookmark171),
[2021](#_bookmark171); [Patel et al.](#_bookmark173),
[2023](#_bookmark173); [Peršak & Anjos](#_bookmark176),
[2023](#_bookmark176); [Sun et al.](#_bookmark202),
[2024](#_bookmark202)), consists in learning a conditional set 
(***𝒙***) to solve the following problem:

> (CRO) min max 𝑐(***𝒛***, ***𝒚***), (21)
>
> ***𝒚 𝒙***

where  (***𝒙***) is an uncertainty set designed to contain with high
proba- bility the realization of ***𝒚*** conditionally on observing
***𝒙***. Their approach solves the CRO problem sequentially where 
(***𝒙***) is learned first and is subsequently used to solve the
downstream RO problem. A challenging problem is to learn the uncertainty
set to minimize the downstream cost function (see recent developments in
[Chenreddy and Delage](#_bookmark77) ([2024](#_bookmark77))).

*Toolboxes and benchmarking.* Several toolboxes and packages have been
proposed recently to train decision pipelines. [Agrawal et
al.](#_bookmark42) ([2019](#_bookmark42)) provide the **cvxpylayers**
library, which includes a subclass of convex optimization problems as
differentiable layers in auto-

differentiation libraries in PyTorch, TensorFlow, and JAX. Other li-
braries for differentiating non-linear optimization problems for end-to-
[end](#_bookmark65) learning include higher ([Grefenstette et
al.](#_bookmark112), [2019](#_bookmark112)), JAXopt
([Blondel](#_bookmark65) [et al.](#_bookmark65), [2022](#_bookmark65)),
TorchOpt ([Ren et al.](#_bookmark181), [2022](#_bookmark181)), and
Theseus ([Pineda et al.](#_bookmark177), [2022](#_bookmark177)). [Tang
and Khalil](#_bookmark205) ([2022](#_bookmark205)) introduce an
open-source software pack- age called PyEPO (Pytorch-based End-to-End
Predict-then-Optimize) implemented in Python for ILO of problems that
are linear in uncertain parameters. They implement various existing
methods, such as **SPO+**, **DBB**, and **PFYL**. They also include new
benchmarks and comprehensive [experiments](#_bookmark87) highlighting
the advantages of integrated learning. [Dalle](#_bookmark87) [et
al.](#_bookmark87) ([2022](#_bookmark87)) provide similar tools for
combinatorial problems in Julia.

Comparisons of existing approaches in fixed simulation settings are
scarce, especially with real-world data. [Buttler et al.](#_bookmark70)
([2022](#_bookmark70)) provide a meta-analysis of selected methods on an
unconstrained newsvendor problem on four data sets from the retail and
food sectors. They highlight that there is no single method that clearly
outperforms all the others on the four data sets. [Mandi et
al.](#_bookmark153) ([2023](#_bookmark153)) carried out a comprehensive
benchmarking of ILO frameworks tailored for expected value-based models
on seven distinct problems using public data sets.

*Endogenous uncertainty.* While there has been some progress in study-
[ing](#_bookmark52) problems where the decision affects the uncertain
parameters ([Bas-](#_bookmark52) [ciftci et al.](#_bookmark52),
[2021](#_bookmark52); [Liu et al.](#_bookmark145),
[2022](#_bookmark145)), the literature on decision-dependent
[uncertainty](#_bookmark59) with covariates is sparse ([Bertsimas &
Kallus](#_bookmark58), [2020](#_bookmark58); [Bertsi-](#_bookmark59)
[mas & Koduri](#_bookmark59), [2022](#_bookmark59)). An example could be
a facility location problem where demand changes once a facility is
located in a region or a price- [setting](#_bookmark147) newsvendor
problem whose demand depends on the price ([Liu](#_bookmark147) [&
Zhang](#_bookmark147), [2023](#_bookmark147)). In these problems, the
causal relationship between de- mand and prices is unknown. These
examples offer interesting parallels [with](#_bookmark212) the research
on heterogeneous treatment effects such as [Wager](#_bookmark212) [and
Athey](#_bookmark212) ([2018](#_bookmark212)), which introduce causal
forests for estimating treat- ment effects and provide asymptotic
consistency results. [Alley et al.](#_bookmark44) ([2023](#_bookmark44))
study a price-setting problem and provide a new loss function to isolate
the causal effects of price on demand from the conditional effects due
to other covariates.

*Data privacy.* Another issue is that the data might come from multiple
sources and contain sensitive private information, so it cannot be
directly provided in its original form to the system operator. Differ-
ential privacy techniques (see, e.g., [Abadi et al.](#_bookmark43),
[2016](#_bookmark43)) can be used to obfuscate data but may impact
predictive and prescriptive perfor- mance. [Mieth et al.](#_bookmark158)
([2023](#_bookmark158)) determine the data quality after obfuscation in
an optimal power flow problem with a Wasserstein ambiguity set and use a
DRO model to determine the data value for decision-making.

*Interpretability & explainability.* Decision pipelines must be trusted
to be implemented. This is evident from the European Union legislation
''General Data Protection Regulation'' that requires entities using au-
tomated systems to provide ''meaningful information about the logic
involved'' in making decisions, known popularly as the ''right to ex-
planation'' ([Doshi-Velez & Kim](#_bookmark96), [2017](#_bookmark96);
[Kaminski](#_bookmark129), [2019](#_bookmark129)). For instance, a
citizen has the right to ask a bank for an explanation in the case of
loan denial. While interpretability has received much attention in
predictive ML applications ([Rudin](#_bookmark184),
[2019](#_bookmark184)), it remains largely unexplored in a contextual
optimization, i.e., prescriptive context. Interpretability requires
transparent decision pipelines that are intelligible to users, e.g.,
built over simple models such as decision trees or rule lists. In
contrast, explainability may be achieved with an additional algorithm on
top of a black box or complex model. Feature importance has been
analyzed in a prescriptive context by [Serrano et al.](#_bookmark192)
([2022](#_bookmark192)). They introduce an integrated approach that
solves a bilevel program with an integer master problem optimizing
(cross-)validation accuracy. To achieve explainability, [Forel et
al.](#_bookmark110) ([2023](#_bookmark110)) adapt the concept of coun-
terfactual explanations to explain a given data-driven decision through
differences of context that make this decision optimal, or better suited
than a given expert decision. Having identified these differences, it
becomes possible to correct or complete the contextual information,

if necessary, or otherwise to give explanative elements supporting
different decisions. Another research direction could be to train tree-
based models (such as optimal classification trees) to approximate the
policy of a complex learning-and-optimization pipeline. This has
interesting connections with model distillation, i.e., the idea in the
ML community of approximating a large model by a smaller one, and the
work of [Bertsimas and Stellato](#_bookmark61) ([2021](#_bookmark61)),
which learns the mapping from the problem parameters to optimal
decisions through interpretable models.

*Fairness.* Applying decisions based on contextual information can raise
fairness issues when the context is made of protected attributes. This
has been studied especially in pricing problems, to ensure that
different customers or groups of customers are proposed prices that do
not differ significantly ([Cohen et al.](#_bookmark82),
[2022](#_bookmark82), [2021](#_bookmark83)).

*Finite sample guarantees for ILO.* In [Grigas et al.](#_bookmark113)
([2021](#_bookmark113)), the authors derive finite-sample guarantees for
ILO under the assumption of dis- crete support for the uncertain
parameter. An open problem is to derive generalization bounds on the
performance of ILO models for non-linear problems where the uncertain
parameters have continuous support.

*Correcting for in-sample bias of data-driven optimization.* When
devising an optimal policy based on a finite number of samples, it is
desired that low in-sample risk translates to low out-of-sample risk.
However, decision rule optimization in ([4](#_bookmark14)) or learning
and optimization model in ([5](#_bookmark12)) are known to produce
optimistically biased estimates of the [true](#_bookmark84) expected
cost of the prescribed policy ([Ban & Rudin](#_bookmark51),
[2019](#_bookmark51); [Costa](#_bookmark84) [& Iyengar](#_bookmark84),
[2023](#_bookmark84); [Gupta et al.](#_bookmark115),
[2022](#_bookmark115)). While one can replace this estimation with an
unbiased one if data was reserved for this purpose, this is usually
considered a wasteful use of data given that it could instead have been
used to obtain a better-performing policy. Recent research has
identified ways of circumventing this issue by estimating and correcting
for the in-sample bias in contextual ([Gupta et al.](#_bookmark115),
[2022](#_bookmark115)) and non-contextual ([Gupta &
Rusmevichientong](#_bookmark116), [2021](#_bookmark116); [Ito et
al.](#_bookmark125), [2018](#_bookmark125); [Iyengar et
al.](#_bookmark126), [2023](#_bookmark126)) stochastic optimization
problems under the assumption that errors in the estimation of uncertain
parameters are normally distributed. A promising future research
direction could be to build a general framework to learn the in-sample
policies that directly minimize the debiased objective functions. In
this regard, one might find inspiration from the work of [Gupta and
Rusmevichientong](#_bookmark116) ([2021](#_bookmark116)) addressing a
similar issue in the non-contextual setting.

*Costly label acquisition.* In many applications, it is costly to gather
observations of uncertain vectors and covariate pairs. For instance, in
personalized pricing, surveys can be sent to customers to obtain
information on the sensitivity of purchasing an item with respect to its
price. However, creating, sending, and collecting the surveys may have a
cost. [Liu et al.](#_bookmark143) ([2023](#_bookmark143)) develop an
active learning approach to obtain labels to solve the **SPO** problem,
while the more general case of developing active learning methods for
non-linear contextual opti- mization is an interesting future direction.
[Besbes et al.](#_bookmark63) ([2023](#_bookmark63)) provide theoretical
results on the trade-off between the quality and quantity of data in a
newsvendor problem, thus guiding decision-makers on how to invest in
data acquisition strategies.

*Multi-agent decision-making.* A multi-agent perspective becomes neces-
sary in transportation and operations management problems, where dif-
ferent agents have access to different sources of information (i.e.
covari- ates). In this regard, some recent work by [Heaton et
al.](#_bookmark120) ([2022](#_bookmark120)) identifies the Nash
equilibrium of contextual games using implicit differentiation of
variational inequalities and jacobian-free backpropagation.

*Multi-stage contextual optimization.* Most works on contextual
optimiza- [tion](#_bookmark182) focus on single and two-stage problems.
[Ban et al.](#_bookmark50) ([2019](#_bookmark50)) and
[Rios](#_bookmark182) [et al.](#_bookmark182) ([2015](#_bookmark182))
use the residuals of the regression model to build multi-
[stage](#_bookmark60) scenario trees and solve multi-stage CSO problems.
[Bertsimas](#_bookmark60)

[et al.](#_bookmark60) ([2023](#_bookmark60)) generalize the weighted
SAA model for multi-stage prob- lems. [Qi et al.](#_bookmark179)
([2023](#_bookmark179)) propose an end-to-end learning framework to
solve a real-world multistage inventory replenishment problem. An
interesting research direction is to challenge the assumption that the
joint distribution of the covariate and uncertain parameters is sta-
tionary. [Neghab et al.](#_bookmark164) ([2022](#_bookmark164)) study a
newsvendor model with a hid- den Markov model underlying the
distribution of the covariates and demand.

An active area of research is sequential decision-making with un-
certainty. Inverse reinforcement learning ([Ng et al.](#_bookmark165),
[2000](#_bookmark165)) focuses on learning rewards that are consistent
with observed trajectories. In the econometrics literature on dynamic
discrete choice modeling the focus lies more broadly on estimating
structural parameters of Markov decision processes (MDPs)
([Rust](#_bookmark186), [1988](#_bookmark186)) including rewards,
transition functions and discount factors. On both topics, estimates are
typically obtained through MLE employing a soft version of the Bellman
opera- tor (e.g., [Rust](#_bookmark185), [1987](#_bookmark185); [Ziebart
et al.](#_bookmark232), [2008](#_bookmark232)). In the context of
model-based reinforcement learning, so-called decision awareness (i.e.
explicitly training components of a reinforcement learning system to
help the agent improve the total amount of collected reward, [Nikishin
et al.](#_bookmark169), [2022](#_bookmark114)) is receiving increasing
attention (e.g., [Farahmand](#_bookmark107), [2018](#_bookmark107);
[Grimm](#_bookmark114) [et al.](#_bookmark114), [2020](#_bookmark114)).
For example, ([Nikishin et al.](#_bookmark168), [2022](#_bookmark168))
introduce an approach that combines learning and planning to optimize
expected returns for both tabular and non-tabular MDPs. Finally, an area
that requires attention is the deployment of models for real-world
applications by tackling computational hurdles associated with
decision-aware learning in MDPs, such as large state--action pairs and
high-dimensional policy spaces ([Wang et al.](#_bookmark215),
[2023](#_bookmark215)). An example is a service call scheduling problem
that is formulated as a restless multi-armed bandit problem in [Mate et
al.](#_bookmark156) ([2022](#_bookmark156)) to improve maternal and
child health in a non-profit organization.

Conclusions
-----------

[]{#_bookmark41 .anchor}In this survey, we summarize advancements in
contextual opti- mization methods developed to solve decision-making
problems under uncertainty. A salient feature of the problems studied is
that a covariate that is correlated with the uncertain parameter is
revealed to the decision-maker before a decision is made. Therefore,
historical data on both covariates and corresponding uncertain parameter
values can be used to prescribe decisions based on the covariate
information. We showed that contextual optimization literature can be
categorized into three data-driven frameworks for learning policies,
namely, de- cision rule optimization, sequential learning and
optimization, and integrated learning and optimization. While decision
rule optimization explicitly parameterizes the policy as a function of
the covariates, learning and optimization require estimating the
conditional distri- bution (or a sufficient statistic in the case of
expected value-based models) of the uncertain parameter given the
covariates in a sequential or integrated manner. By providing a
parametric description of these three data-driven frameworks, we have
introduced a uniform nota- tion and terminology for analyzing different
methods. In particular, we have elaborately described integrated
learning and optimization models using both their training pipeline and
modeling choice, further emphasizing that these two aspects are
intertwined. Furthermore, we have provided a list of active fields of
research in CSO problems.

Acknowledgments
---------------

Utsav Sadana was supported by the GERAD postdoctoral fellowship and
FRQNT postdoctoral research scholarship \[Grant 301065\]. Erick Delage
was partially supported by the Canadian Natural Sciences and Engineering
Research Council \[Grant RGPIN-2022-05261\] and by the Canada Research
Chair program \[950-23005\]. Emma Frejinger was par- tially supported by
the Canada Research Chair program \[950-232244\]. Finally, the authors
also acknowledge the support of IVADO, SCALE-AI

through its AI Research Chair Program, and the Canada First Research
Excellence Fund (Apogée/CFREF).

References
----------

> []{#_bookmark43 .anchor}[Abadi, M., Chu, A., Goodfellow, I., McMahan,
> H. B., Mironov, I., Talwar, K., & Zhang,
> L.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb1) [(2016).
> Deep learning with differential privacy. In *Proceedings of the 2016
> ACM*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb1) [*SIGSAC
> conference on computer and communications security* (pp. 308--318).
> NY, USA:](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb1)
> [Association for Computing
> Machinery.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb1)
>
> [Agrawal,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb2) [A.,
> Amos, B., Barratt, S., Boyd, S., Diamond, S., & Kolter, J. Z. (2019).
> Dif-](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb2)
> [ferentiable convex optimization layers. In *Advances in Neural
> Information
> Processing*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb2)
> [*Systems*: *vol. 32*, Curran Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb2)
>
> [Alley,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb3) [M.,
> Biggs, M., Hariss, R., Herrmann, C., Li, M. L., & Perakis, G. (2023).
> Pricing](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb3) [for
> heterogeneous products: Analytics for ticket reselling. *Manufacturing
> & Service*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb3)
> [*Operations Management*, *25*(2),
> 409--426.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb3)
>
> [Amos, B., & Kolter, J. Z. (2017). OptNet: Differentiable optimization
> as a layer in
> neural](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb4)
> [networks. *Vol. 70*, In *International Conference on Machine
> Learning* (pp.
> 136--145).](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb4)
> [PMLR.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb4)
>
> [Aronszajn,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb5) [N.
> (1950). Theory of reproducing kernels. *Transactions of the
> American*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb5)
> [*Mathematical Society*, *68*(3),
> 337--404.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb5)
>
> [Athey,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb6) [S.,
> Tibshirani, J., & Wager, S. (2019). Generalized random forests. *The
> Annals*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb6) [*of
> Statistics*, *47*(2),
> 1148--1178.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb6)
>
> [Backhoff,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb7) [J.,
> Beiglbock, M., Lin, Y., & Zalashko, A. (2017). Causal transport in
> discrete](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb7) [time
> and applications. *SIAM Journal on Optimization*, *27*(4),
> 2528--2562.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb7)
>
> [Bai,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb8) [S.,
> Kolter, J. Z., & Koltun, V. (2019). Deep equilibrium models. In
> *Advances in*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb8)
> [*Neural Information Processing Systems*: *Vol. 32*, Curran
> Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb8)
>
> [Ban,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb9) [G.-Y.,
> Gallien, J., & Mersereau, A. J. (2019). Dynamic procurement of
> new](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb9) [products
> with covariate information: The residual tree method. *Manufacturing
> &*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb9) [*Service
> Operations Management*, *21*(4),
> 798--815.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb9)
>
> [Ban,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb10) [G.-Y.,
> & Rudin, C. (2019). The Big Data Newsvendor: Practical Insights
> from](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb10) [Machine
> Learning. *Operations Research*, *67*(1),
> 90--108.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb10)
>
> [Basciftci, B., Ahmed, S., & Shen, S. (2021). Distributionally robust
> facility location
> prob-](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb11) [lem
> under decision-dependent stochastic demand. *European Journal of
> Operational*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb11)
> [*Research*, *292*(2),
> 548--561.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb11)
>
> [Bazier-Matte,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb12)
> [T., & Delage, E. (2020). Generalization bounds for regularized
> portfolio](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb12)
> [selection with market side information. *INFOR: Information Systems
> and
> Operational*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb12)
> [*Research*, *58*(2),
> 374--401.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb12)
>
> [Bengio, Y. (1997). Using a financial training criterion rather than a
> prediction
> criterion.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb13)
>
> []{#_bookmark55 .anchor}[*International Journal of Neural Systems*,
> *8*(4),
> 433--443.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb13)
>
> [Bengio,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb14) [Y.,
> Lodi, A., & Prouvost, A. (2021). Machine learning for
> combinatorial](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb14)
> [optimization: A methodological tour d'horizon. *European Journal of
> Operational*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb14)
> [*Research*, *290*(2),
> 405--421.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb14)
>
> [Berthet,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb15) [Q.,
> Blondel, M., Teboul, O., Cuturi, M., Vert, J.-P., & Bach, F.
> (2020).](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb15)
> [Learning with differentiable perturbed optimizers. In *Advances in
> Neural
> Information*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb15)
> [*Processing Systems*: *Vol. 33*, (pp. 9508--9519). Curran Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb15)
>
> [Bertsimas,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb16)
> [D., Dunn, J., & Mundru, N. (2019). Optimal prescriptive trees.
> *INFORMS*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb16)
> [*Journal on Optimization*, *1*(2),
> 164--183.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb16)
>
> [Bertsimas,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb17)
> [D., & Kallus, N. (2020). From predictive to prescriptive
> analytics.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb17)
>
> []{#_bookmark59 .anchor}[*Management Science*, *66*(3),
> 1025--1044.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb17)
>
> [Bertsimas,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb18)
> [D., & Koduri, N. (2022). Data-driven optimization: A reproducing
> kernel](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb18)
> [Hilbert space approach. *Operations Research*, *70*(1),
> 454--471.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb18)
>
> [Bertsimas,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb19)
> [D., McCord, C., & Sturt, B. (2023). Dynamic optimization with
> side](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb19)
> [information. *European Journal of Operational Research*, *304*(2),
> 634--651.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb19)
>
> [Bertsimas, D., & Stellato, B. (2021). The voice of optimization.
> *Machine Learning*,
> *110*(2),](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb20)
> [249--277.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb20)
>
> [Bertsimas,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb21)
> [D., & Van Parys, B. (2022). Bootstrap robust prescriptive
> analytics.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb21)
>
> []{#_bookmark63 .anchor}[*Mathematical Programming*, *195*(1--2),
> 39--78.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb21)
>
> Besbes, O., Ma, W., & Mouchtaki, O. (2023). Quality vs. Quantity of
> data in contextual decision-making: Exact analysis under newsvendor
> loss. arXiv preprint [arXiv:2302.](http://arxiv.org/abs/2302.08424)
> [08424](http://arxiv.org/abs/2302.08424).
>
> [Birge, J. R., & Louveaux, F. (2011). *Introduction to stochastic
> programming*. New
> York,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb23) [NY:
> Springer.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb23)
>
> [Blondel, M., Berthet, Q., Cuturi, M., Frostig, R., Hoyer, S.,
> Llinares-López, F.,
> Pe-](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb24)
>
> [dregosa, F., & Vert, J.-P. (2022). Efficient and modular implicit
> differentiation.
> In](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb24) [*Advances
> in Neural Information Processing Systems*: *Vol. 35*, (pp.
> 5230--5242).
> Curran](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb24)
> [Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb24)
>
> [Blondel, M., Martins, A. F., & Niculae, V. (2020). Learning with
> Fenchel-Young
> losses.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb25)
>
> []{#_bookmark67 .anchor}[*Journal of Machine Learning Research*,
> *21*(1),
> 1314--1382.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb25)
>
> [Bolte, J., Le, T., Pauwels, E., & Silveti-Falls, T. (2021). Nonsmooth
> implicit
> differen-](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb26)
> [tiation for machine-learning and optimization. In *Advances in Neural
> Information*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb26)
> [*Processing Systems*: *Vol. 34*, (pp.
> 13537--13549).](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb26)
>
> [Butler, A., & Kwon, R. H. (2023a). Efficient differentiable quadratic
> programming](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb27)
> [layers: an ADMM approach. *Computational Optimization and
> Applications*,
> *84*(2),](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb27)
> [449--476.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb27)
>
> [Butler, A., & Kwon, R. H. (2023b). Integrating prediction in
> mean-variance
> portfolio](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb28)
> [optimization. *Quantitative Finance*, *23*(3),
> 429--452.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb28)
>
> [Buttler, S., Philippi, A., Stein, N., & Pibernik, R. (2022). A meta
> analysis of
> data-driven](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb29)
> [newsvendor approaches. In *ICLR 2022 workshop on setting up ML
> evaluation
> standards*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb29)
> [*to accelerate
> progress*.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb29)
>
> []{#_bookmark71
> .anchor}[Chan,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb30)
> [T. C., Mahmood, R., & Zhu, I. Y. (2021). Inverse optimization: Theory
> and](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb30)
> [applications. *Operations
> Research*.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb30)
>
> [Chen,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb31) [B.,
> Donti, P. L., Baker, K., Kolter, J. Z., & Bergés, M. (2021). Enforcing
> policy](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb31)
>
> [feasibility constraints through differentiable projection for energy
> optimization.
> In](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb31)
> [*Proceedings of the Twelfth ACM International Conference on Future
> Energy Systems*
> (pp.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb31)
> [199--210).
> ACM.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb31)
>
> [Chen,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb32) [R., &
> Paschalidis, I. C. (2018). A robust learning approach for regression
> models](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb32) [based
> on distributionally robust optimization. *Journal of Machine Learning
> Research*,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb32)
> [*19*(13),
> 1--48.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb32)
>
> [Chen, R., & Paschalidis, I. (2019). Selecting optimal decisions via
> distributionally
> robust](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb33)
> [nearest-neighbor regression. In *Advances in Neural Information
> Processing
> Systems*:](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb33)
> [*Vol. 32*, Curran Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb33)
>
> Chen, W., Tanneau, M., & Van Hentenryck, P. (2023). End-to-end
> feasible optimization
>
> []{#_bookmark76 .anchor}proxies for large-scale economic dispatch.
> arXiv preprint [arXiv:2304.11726](http://arxiv.org/abs/2304.11726).
>
> [Chenreddy,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb35)
> [A. R., Bandi, N., & Delage, E. (2022). Data-driven conditional
> robust](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb35)
> [optimization. In *Advances in Neural Information Processing Systems*:
> *Vol. 35*,
> (pp.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb35)
> [9525--9537). Curran Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb35)
>
> Chenreddy, A., & Delage, E. 2024. End-to-end Conditional Robust
> Optimization, arXiv
>
> []{#_bookmark78 .anchor}preprint
> [arXiv:2403.04670](http://arxiv.org/abs/2403.04670).
>
> [Chu,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb37) [H.,
> Zhang, W., Bai, P., & Chen, Y. (2023). Data-driven optimization for
> last-mile](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb37)
> [delivery. *Complex & Intelligent Systems*, *9*(9),
> 2271--2284.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb37)
>
> Chung, T.-H., Rostami, V., Bastani, H., & Bastani, O. (2022).
> Decision-aware learning
>
> []{#_bookmark80 .anchor}for optimizing health supply chains. arXiv
> preprint [arXiv:2211.08507](http://arxiv.org/abs/2211.08507).
>
> [Ciocan, D. F., & Mišić, V. V. (2022). Interpretable optimal stopping.
> *Management
> Science*,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb39)
> [*68*(3),
> 1616--1638.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb39)

[]{#_bookmark82 .anchor}[Clarke, F. H. (1990). *Optimization and
nonsmooth analysis*.
SIAM.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb40)

> [Cohen,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb41) [M.
> C., Elmachtoub, A. N., & Lei, X. (2022). Price discrimination with
> fairness](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb41)
> [constraints. *Management Science*, *68*(12),
> 8536--8552.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb41)
>
> Cohen, M. C., Miao, S., & Wang, Y. (2021). Dynamic pricing with
> fairness constraints.
>
> []{#_bookmark84 .anchor}Available at
> <https://doi.org/10.2139/ssrn.3930622>.
>
> [Costa,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb43) [G., &
> Iyengar, G. N. (2023). Distributionally robust end-to-end
> portfolio](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb43)
> [construction. *Quantitative Finance*, *23*(10),
> 1465--1482.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb43)
>
> [Cristian,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb44)
> [R., Harsha, P., Perakis, G., Quanz, B. L., & Spantidakis, I. (2022).
> End-](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb44)
>
> [to-end learning via constraint-enforcing approximators for linear
> programs with](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb44)
> [applications to supply chains. In *AI for Decision Optimization
> Workshop of the
> AAAI*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb44)
> [*Conference on Artificial
> Intelligence*.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb44)
>
> [Cybenko,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb45) [G.
> (1989). Approximation by superpositions of a sigmoidal
> function.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb45)

[]{#_bookmark87 .anchor}[*Mathematics of Control, Signals, and Systems*,
*2*(4),
303--314.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb45)

> Dalle, G., Baty, L., Bouvier, L., & Parmentier, A. (2022). Learning
> with combinatorial []{#_bookmark88 .anchor}optimization layers: a
> probabilistic approach. arXiv preprint
> [arXiv:2207.13513](http://arxiv.org/abs/2207.13513).
>
> [Davis,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb47) [D., &
> Yin, W. (2017). A three-operator splitting scheme and its
> optimization](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb47)
> [applications. *Set-Valued and Variational Analysis*, *25*(4),
> 829--858.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb47)
>
> [Demirović,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb48)
> [E., J. Stuckey, P., Bailey, J., Chan, J., Leckie, C., Ramamohanarao,
> K.,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb48)
>
> [Guns, T., & Kraus, S. (2019). Predict+ optimise with ranking
> objectives:
> Ex-](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb48)
> [haustively learning linear functions. In *International Joint
> Conferences on
> Artificial*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb48)
> [*Intelligence* (pp.
> 1078--1085).](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb48)
>
> [Demirović,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb49)
> [E., Stuckey, P. J., Guns, T., Bailey, J., Leckie, C., Ramamohanarao,
> K., &](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb49) [Chan,
> J. (2020). Dynamic programming for predict+optimise. *Vol. 34*, In
> *AAAI*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb49)
> [*Conference on Artificial Intelligence* (0202), (pp.
> 1444--1451).](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb49)
>
> [Deng,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb50) [Y., &
> Sen, S. (2022). Predictive stochastic programming.
> *Computational*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb50)
> [*Management Science*, *19*(1),
> 65--98.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb50)
>
> [Domke,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb51) [J.
> (2012). Generic methods for optimization-based modeling. In
> *International*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb51)
> [*Conference on Artificial Intelligence and Statistics* (pp.
> 318--326).
> PMLR.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb51)
>
> [Dontchev,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb52) [A.
> L., Rockafellar, R. T., & Rockafellar, R. T. (2009). *Vol. 616*,
> *Implicit*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb52)
> [*functions and solution mappings: A view from variational analysis*.
> Springer.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb52)
>
> [Donti,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb53) [P.,
> Amos, B., & Kolter, J. Z. (2017). Task-based end-to-end model learning
> in](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb53) [stochastic
> optimization. In *Advances in Neural Information Processing Systems*:
> *vol.*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb53) [*30*,
> Curran Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb53)
>
> [Donti,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb54) [P.
> L., Roderick, M., Fazlyab, M., & Kolter, J. Z. (2021). Enforcing
> robust control](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb54)
> [guarantees within neural network policies. In *International
> Conference on
> Learning*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb54)
> [*Representations*.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb54)
>
> Doshi-Velez, F., & Kim, B. (2017). Towards a rigorous science of
> interpretable machine
>
> []{#_bookmark97 .anchor}learning. arXiv preprint
> [arXiv:1702.08608](http://arxiv.org/abs/1702.08608).
>
> [Duvenaud,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb56)
> [D., Kolter, J. Z., & Johnson, M. (2020). Deep implicit layers
> tutorial -](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb56)
> [neural ODEs, deep equilibrium models, and beyond. In *Neural
> Information
> Processing*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb56)
> [*Systems
> Tutorial*.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb56)
>
> [El](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb57) [Balghiti,
> O., Elmachtoub, A. N., Grigas, P., & Tewari, A. (2019).
> Generalization](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb57)
> [bounds in the predict-then-optimize framework. In *Advances in Neural
> Information*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb57)
> [*Processing Systems*: *vol. 32*, Curran Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb57)
>
> [El](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb58) [Balghiti,
> O., Elmachtoub, A. N., Grigas, P., & Tewari, A. (2022).
> Generalization](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb58)
> [bounds in the predict-then-optimize framework. *Mathematics of
> Operations
> Research*,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb58)
> [*48*(4),
> 1811--2382.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb58)
>
> [Elmachtoub, A. N., & Grigas, P. (2022). Smart ''Predict, then
> Optimize''.
> *Management*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb59)
> [*Science*, *68*(1),
> 9--26.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb59)
>
> Elmachtoub, A. N., Lam, H., Zhang, H., & Zhao, Y. (2023).
> Estimate-then-optimize ver-
>
> sus integrated-estimation-optimization: A stochastic dominance
> perspective. arXiv []{#_bookmark102 .anchor}preprint
> [arXiv:2304.06833](http://arxiv.org/abs/2304.06833).
>
> [Elmachtoub, A. N., Liang, J. C. N., & McNellis, R. (2020). Decision
> trees for
> decision-](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb61)
> [making under the predict-then-optimize framework. In *International
> Conference on*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb61)
> [*Machine Learning* (pp. 2858--2867).
> PMLR.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb61)
>
> []{#_bookmark103
> .anchor}[Esteban-Pérez,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb62)
> [A., & Morales, J. M. (2022). Distributionally robust stochastic
> programs](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb62) [with
> side information based on trimmings. *Mathematical Programming*,
> *195*(1),](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb62)
> [1069--1105.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb62)
>
> [Esteban-Pérez,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb63)
> [A., & Morales, J. M. (2023). Distributionally robust optimal
> power](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb63) [flow
> with contextual information. *European Journal of Operational
> Research*,
> *306*(3),](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb63)
> [1047--1058.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb63)

[Estes, A. S., & Richard, J.-P. P. (2023). Smart predict-then-optimize
for two-stage
linear](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb64) [programs
with side information. *INFORMS Journal on Optimization*, *5*(3),
233--320.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb64)

> [Fajemisin,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb65)
> [A. O., Maragno, D., & den Hertog, D. (2023). Optimization with
> constraint](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb65)
> [learning: a framework and survey. *European Journal of Operational
> Research*.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb65)
>
> [Farahmand,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb66)
> [A.-M. (2018). Iterative value-aware model learning. In S. Bengio, H.
> Wal-](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb66) [lach, H.
> Larochelle, K. Grauman, N. Cesa-Bianchi, & R. Garnett (Eds.),
> *Advances in*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb66)
> [*Neural Information Processing Systems*: *vol.
> 31*.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb66)
>
> [Ferber,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb67) [A.,
> Wilder, B., Dilkina, B., & Tambe, M. (2020). MIPaaL: Mixed
> integer](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb67)
> [program as a layer. *Vol. 34*, In *AAAI Conference on Artificial
> Intelligence* (02),
> (pp.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb67)
> [1504--1511).](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb67)
>
> [Ferreira,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb68) [K.
> J., Lee, B. H., & Simchi-Levi, D. (2016). Analytics for an online
> retailer:](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb68)
> [Demand forecasting and price optimization. *Manufacturing & Service
> Operations*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb68)
> [*Management*, *18*(1),
> 69--88.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb68)
>
> [Forel,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb69) [A.,
> Parmentier, A., & Vidal, T. (2023). Explainable data-driven
> optimization:](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb69)
> [From context to decision and back again. In *International Conference
> on Machine*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb69)
> [*Learning* (pp. 10170--10187).
> PMLR.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb69)
>
> [Fung,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb70) [S. W.,
> Heaton, H., Li, Q., Mckenzie, D., Osher, S., & Yin, W. (2022).
> JFB:](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb70)
> [Jacobian-free backpropagation for implicit networks. *Vol. 36*, In
> *AAAI
> Conference*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb70)
> [*on Artificial Intelligence* (66), (pp.
> 6648--6656).](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb70)
>
> Grefenstette, E., Amos, B., Yarats, D., Htut, P. M., Molchanov, A.,
> Meier, F., Kiela, D.,
>
> Cho, K., & Chintala, S. (2019). Generalized inner loop meta-learning.
> arXiv preprint [arXiv:1910.01727](http://arxiv.org/abs/1910.01727).
>
> Grigas, P., Qi, M., & Shen, M. (2021). Integrated conditional
> estimation-optimization. []{#_bookmark114 .anchor}arXiv preprint
> [arXiv:2110.12351](http://arxiv.org/abs/2110.12351).
>
> [Grimm,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb73) [C.,
> Barreto, A., Singh, S., & Silver, D. (2020). The value equivalence
> principle](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb73) [for
> model-based reinforcement learning. In *Advances in Neural Information
> Processing*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb73)
> [*Systems*: *vol.
> 33*.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb73)
>
> [Gupta,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb74) [V.,
> Huang, M., & Rusmevichientong, P. (2022). Debiasing in-sample
> policy](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb74)
> [performance for small-data, large-scale optimization. *Operations
> Research*.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb74)
>
> [Gupta,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb75) [V., &
> Rusmevichientong, P. (2021). Small-data, large-scale linear
> optimization](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb75)
> [with uncertain objectives. *Management Science*, *67*(1),
> 220--241.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb75)
>
> [Halkin,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb76) [H.
> (1974). Implicit functions and optimization problems without
> continuous](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb76)
> [differentiability of the data. *SIAM Journal on Control*, *12*(2),
> 229--236.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb76)
>
> [Hannah,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb77) [L.,
> Powell, W., & Blei, D. (2010). Nonparametric density estimation
> for](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb77)
> [stochastic optimization with an observable state variable. In
> *Advances in
> Neural*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb77)
> [*Information Processing Systems*: *vol. 23*, Curran Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb77)
>
> [Hastie,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb78) [T.,
> Tibshirani, R., & Friedman, J. H. (2009). *Vol. 2*, *The elements of
> statistical*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb78)
> [*learning: data mining, inference, and prediction*.
> Springer.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb78)
>
> Heaton, H., McKenzie, D., Li, Q., Fung, S. W., Osher, S., & Yin, W.
> (2022). Learn to
>
> []{#_bookmark121 .anchor}predict equilibria via fixed point networks.
> [arXiv:2106.00906](http://arxiv.org/abs/2106.00906).
>
> [Ho-Nguyen,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb80)
> [N., & Kılınç-Karzan, F. (2022). Risk guarantees for end-to-end
> prediction](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb80)
> [and optimization processes. *Management Science*, *68*(12),
> 8680--8698.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb80)
>
> [Hofmann,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb81) [T.,
> Schölkopf, B., & Smola, A. (2008). Kernel methods in machine
> learning.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb81)
>
> []{#_bookmark123 .anchor}[*The Annals of Statistics*, *36*(3),
> 1171--1220.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb81)
>
> [Hu, Y., Kallus, N., & Mao, X. (2022). Fast rates for contextual
> linear
> optimization.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb82)
>
> []{#_bookmark124 .anchor}[*Management Science*, *68*(6),
> 4236--4245.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb82)
>
> [Huber, J., Müller, S., Fleischmann, M., & Stuckenschmidt, H. (2019).
> A data-driven](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb83)
> [newsvendor problem: From data to decision. *European Journal of
> Operational*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb83)
> [*Research*, *278*(3),
> 904--915.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb83)
>
> [Ito, S., Yabe, A., & Fujimaki, R. (2018). Unbiased objective
> estimation in
> predictive](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb84)
> [optimization. In *International Conference on Machine Learning* (pp.
> 2176--2185).](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb84)
> [PMLR.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb84)

Iyengar, G., Lam, H., & Wang, T. (2023). Optimizer's information
criterion: Dissecting

> []{#_bookmark127 .anchor}and correcting bias in data-driven
> optimization. arXiv preprint
> [arXiv:2306.10081](http://arxiv.org/abs/2306.10081). [Jeong, J.,
> Jaggi, P., Butler, A., & Sanner, S. (2022). An exact symbolic
> reduction of](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb86)
> [linear smart predict+optimize to mixed integer linear programming.
> *Vol. 162*, In](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb86)
>
> []{#_bookmark128 .anchor}[*International Conference on Machine
> Learning* (pp. 10053--10067).
> PMLR.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb86)
>
> [Kallus, N., & Mao, X. (2022). Stochastic optimization forests.
> *Management Science*,
> *69*(4),](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb87)
> [1975--1994.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb87)
>
> [Kaminski, M. E. (2019). The right to explanation, explained.
> *Berkeley Technology
> Law*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb88)
> [*Journal*, *34*(1),
> 189--218.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb88)
>
> [Kannan, R., Bayraksan, G., & Luedtke, J. R. (2020). Residuals-based
> distributionally](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb89)
> [robust optimization with covariate information. *Mathematical
> Programming*.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb89)
>
> Kannan, R., Bayraksan, G., & Luedtke, J. (2021).
> Heteroscedasticity-aware residuals-
>
> []{#_bookmark132 .anchor}based contextual stochastic optimization.
> arXiv preprint [arXiv:2101.03139](http://arxiv.org/abs/2101.03139).
>
> Kannan, R., Bayraksan, G., & Luedtke, J. R. (2022). Data-driven sample
> average []{#_bookmark133 .anchor}approximation with covariate
> information. arXiv preprint
> [arXiv:2207.13554](http://arxiv.org/abs/2207.13554).
>
> [Kantorovich, L. V., & Rubinshtein, G. S. (1958). On a space of
> totally additive
> functions.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb92)
>
> []{#_bookmark134 .anchor}[*Vestnik Leningradskogo Universiteta*,
> *13*(7),
> 52--59.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb92)
>
> [Keshavarz, P. (2022). *Interpretable contextual newsvendor models: A
> tree-based method
> to*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb93) [*solving
> data-driven newsvendor problems* (pp. 1616--1638). University of
> Ottawa.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb93)
>
> [Kong, L., Cui, J., Zhuang, Y., Feng, R., Prakash, B. A., & Zhang, C.
> (2022).
> End-to-end](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb94)
> [stochastic optimization with energy-based model. In *Advances in
> Neural
> Information*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb94)
> [*Processing Systems*: *vol. 35*, (pp. 11341--11354). Curran
> Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb94)
>
> []{#_bookmark137 .anchor}[Kotary, J., Dinh, M. H., & Fioretto, F.
> (2023). Folded optimization for
> end-to-end](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb95)
> [model-based learning. In *International Joint Conference on
> Artificial
> Intelligence*.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb95)
>
> [Kotary, J., Fioretto, F., Van Hentenryck, P., & Wilder, B. (2021).
> End-to-end
> constrained](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb96)
> [optimization learning: A survey. In *International Joint Conferences
> on Artificial*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb96)
> [*Intelligence* (pp.
> 4475--4482).](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb96)
>
> [Kullback, S., & Leibler, R. A. (1951). On information and
> sufficiency. *The Annals
> of*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb97)
>
> []{#_bookmark139 .anchor}[*Mathematical Statistics*, *22*(1),
> 79--86.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb97)
>
> [Lassalle, R. (2018). Causal transport plans and their
> Monge--Kantorovich
> problems.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb98)
>
> []{#_bookmark140 .anchor}[*Stochastic Analysis and Applications*,
> *36*(3),
> 452--484.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb98)
>
> Lawless, C., & Zhou, A. (2022). A note on task-aware loss via
> reweighing prediction []{#_bookmark141 .anchor}loss by
> decision-regret. arXiv preprint
> [arXiv:2211.05116](http://arxiv.org/abs/2211.05116).
>
> [Lin, S., Chen, Y., Li, Y., & Shen, Z.-J. M. (2022). Data-driven
> newsvendor
> problems](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb100)
> [regularized by a profit risk constraint. *Production and Operations
> Management*,
> *31*(4),](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb100)
>
> []{#_bookmark142
> .anchor}[1630--1644.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb100)
>
> [Liu, H., & Grigas, P. (2021). Risk bounds and calibration for a smart
> predict-then-](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb101)
> [optimize method. In *Advances in Neural Information Processing
> Systems*: *vol. 34*,
> (pp.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb101)
> [22083--22094). Curran Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb101)
>
> Liu, M., Grigas, P., Liu, H., & Shen, Z.-J. M. (2023). Active learning
> in the predict-
>
> then-optimize framework: A margin-based approach. arXiv preprint
> [arXiv:2305.](http://arxiv.org/abs/2305.06584)
> [06584](http://arxiv.org/abs/2305.06584).
>
> [Liu, S., He, L., & Max Shen, Z.-J. (2021). On-time last-mile
> delivery: Order
> assignment](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb103)
> [with travel-time predictors. *Management Science*, *67*(7),
> 4095--4119.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb103)
>
> [Liu, J., Li, G., & Sen, S. (2022). Coupled learning enabled
> stochastic programming
> with](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb104)
>
> []{#_bookmark146 .anchor}[endogenous uncertainty. *Mathematics of
> Operations Research*, *47*(2),
> 1681--1705.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb104)
>
> [Liu, Z., Yin, Y., Bai, F., & Grimm, D. K. (2023). End-to-end learning
> of user](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb105)
> [equilibrium with implicit neural networks. *Transportation Research
> Part C
> (Emerging*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb105)
> [*Technologies)*, *150*, Article
> 104085.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb105)
>
> Liu, W., & Zhang, Z. (2023). Solving data-driven newsvendor pricing
> problems with
>
> []{#_bookmark148 .anchor}decision-dependent effect. arXiv preprint
> [arXiv:2304.13924](http://arxiv.org/abs/2304.13924).
>
> [Liyanage, L. H., & Shanthikumar, J. G. (2005). A practical inventory
> control
> policy](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb107) [using
> operational statistics. *Operations Research Letters*, *33*(4),
> 341--348.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb107)
>
> Loke, G. G., Tang, Q., & Xiao, Y. (2022). Decision-driven
> regularization: A
>
> blended model for predict-then-optimize. Available at
> [https://doi.org/10.2139/ssrn.](https://doi.org/10.2139/ssrn.3623006)
> [3623006](https://doi.org/10.2139/ssrn.3623006).
>
> [Lu, Z., Pu, H., Wang, F., Hu, Z., & Wang, L. (2017). The expressive
> power of
> neural](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb109)
> [networks: A view from the width. In *Advances in Neural Information
> Processing*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb109)
> [*Systems*, Curran Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb109)
>
> [Mandi, J., Bucarey, V., Tchomba, M. M. K., & Guns, T. (2022).
> Decision-focused](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb110)
> [learning: Through the lens of learning to rank. In *International
> Conference
> on*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb110)
>
> []{#_bookmark152 .anchor}[*Machine Learning* (pp. 14935--14947).
> PMLR.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb110)
>
> [Mandi, J., & Guns, T. (2020). Interior point solving for LP-based
> predic-](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb111)
> [tion+optimisation. In *Advances in Neural Information Processing
> Systems*: *vol.
> 33*,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb111) [(pp.
> 7272--7282). Curran Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb111)
>
> Mandi, J., Kotary, J., Berden, S., Mulamba, M., Bucarey, V., Guns, T.,
> & Fioretto, F.
>
> (2023). Decision-focused learning: Foundations, state of the art,
> benchmark and []{#_bookmark154 .anchor}future opportunities. arXiv
> preprint [arXiv:2307.13565](http://arxiv.org/abs/2307.13565).
>
> [Mandi, J., Stuckey, P. J., & Guns, T. (2020). Smart
> predict-and-optimize for
> hard](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb113)
> [combinatorial optimization problems. *Vol. 34*, In *AAAI Conference
> on
> Artificial*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb113)
> [*Intelligence* (02), (pp.
> 1603--1610).](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb113)
>
> [Martínez-de Albeniz, V., & Belkaid, A. (2021). Here comes the sun:
> Fashion goods](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb114)
> [retailing under weather fluctuations. *European Journal of
> Operational
> Research*,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb114)
>
> []{#_bookmark156 .anchor}[*294*(3),
> 820--830.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb114)
>
> [Mate, A., Madaan, L., Taneja, A., Madhiwalla, N., Verma, S., Singh,
> G., Hegde,
> A.,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb115)
> [Varakantham, P., & Tambe, M. (2022). Field study in deploying
> restless
> multi-](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb115) [armed
> bandits: Assisting non-profits in improving maternal and child health.
> *Vol.*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb115) [*36*,
> In *AAAI Conference on Artificial Intelligence* (1111), (pp.
> 12017--12025).](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb115)
>
> McKenzie, D., Fung, S. W., & Heaton, H. (2023). Faster
> predict-and-optimize with
>
> []{#_bookmark158 .anchor}three-operator splitting. arXiv preprint
> [arXiv:2301.13395](http://arxiv.org/abs/2301.13395).
>
> Mieth, R., Morales, J. M., & Poor, H. V. (2023). Data valuation from
> data-driven []{#_bookmark159 .anchor}optimization. arXiv preprint
> [arXiv:2305.01775](http://arxiv.org/abs/2305.01775).
>
> [Mišić, V. V., & Perakis, G. (2020). Data analytics in operations
> management: A
> review.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb118)
>
> []{#_bookmark160 .anchor}[*Manufacturing & Service Operations
> Management*, *22*(1),
> 158--169.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb118)
>
> [Monga, V., Li, Y., & Eldar, Y. C. (2021). Algorithm unrolling:
> Interpretable,
> efficient](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb119)
> [deep learning for signal and image processing. *IEEE Signal
> Processing
> Magazine*,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb119)
> [*38*(2),
> 18--44.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb119)
>
> [Mulamba, M., Mandi, J., Diligenti, M., Lombardi, M., Lopez, V. B., &
> Guns, T.
> (2021).](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb120)
> [Contrastive losses and solution caching for predict-and-optimize. In
> *International*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb120)
>
> []{#_bookmark162 .anchor}[*Joint Conference on Artificial
> Intelligence* (pp.
> 2833--2840).](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb120)
>
> [Muñoz, M. A., Pineda, S., & Morales, J. M. (2022). A bilevel
> framework for](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb121)
> [decision-making under uncertainty with contextual information.
> *Omega*, *108*,
> Article](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb121)
> [102575.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb121)
>
> [Nadaraya, E. (1964). On estimating regression. *Theory of Probability
> & its
> Applications*,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb122)
>
> []{#_bookmark164 .anchor}[*9*(1),
> 141--142.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb122)
>
> [Neghab, D. P., Khayyati, S., & Karaesmen, F. (2022). An integrated
> data-driven
> method](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb123) [using
> deep learning for a newsvendor problem with unobservable features.
> *European*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb123)
> [*Journal of Operational Research*, *302*(2),
> 482--496.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb123)
>
> [Ng, A. Y., & Russell, S. (2000). Algorithms for inverse reinforcement
> learning. *Vol.
> 1*,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb124)
>
> []{#_bookmark166 .anchor}[In *International Conference on Machine
> Learning* (p.
> 2).](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb124)
>
> [Nguyen, V. A., Zhang, F., Blanchet, J., Delage, E., & Ye, Y. (2020).
> Distributionally
> ro-](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb125) [bust
> local non-parametric conditional estimation. In *Advances in Neural
> Information*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb125)
> [*Processing Systems*: *vol. 33*, (pp. 15232--15242). Curran
> Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb125)
>
> []{#_bookmark167 .anchor}Nguyen, V. A., Zhang, F., Blanchet, J.,
> Delage, E., & Ye, Y. (2021). Robustifying conditional portfolio
> decisions via optimal transport. arXiv preprint
> [arXiv:2103.](http://arxiv.org/abs/2103.16451)
> [16451](http://arxiv.org/abs/2103.16451).
>
> [Nikishin,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb127)
> [E., Abachi, R., Agarwal, R., & Bacon, P.-L. (2022). Control-oriented
> model-](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb127) [based
> reinforcement learning with implicit differentiation. *Vol. 36*, In
> *AAAI*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb127)
> [*Conference on Artificial Intelligence* (7), (pp.
> 7886--7894).](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb127)
>
> [Nikishin,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb128)
> [E., D'Oro, P., Precup, D., Barreto, A., massoud Farahmand, A., Bacon,
> P.-L., &](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb128)
> [Hall, G. (2022). Decision awareness in reinforcement learning. In
> *Workshop
> abstract.*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb128)
> [*International Conference on Machine
> Learning*.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb128)
>
> [Notz,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb129) [P.
> M., & Pibernik, R. (2022). Prescriptive analytics for flexible
> capacity](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb129)
> [management. *Management Science*, *68*(3),
> 1756--1775.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb129)
>
> [Ohmori, S. (2021). A predictive prescription using minimum volume
> k-nearest
> neighbor](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb130)
> [enclosing ellipsoid and robust optimization. *Mathematics*, *9*(2),
> 119.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb130)
>
> [Oroojlooyjadid,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb131)
> [A., Snyder, L. V., & Takáč, M. (2020). Applying deep learning to
> the](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb131)
> [newsvendor problem. *IISE Transactions*, *52*(4),
> 444--463.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb131)
>
> Patel, Y., Rayan, S., & Tewari, A. (2023). Conformal contextual robust
> optimization.
>
> []{#_bookmark174 .anchor}arXiv preprint
> [arXiv:2310.10003](http://arxiv.org/abs/2310.10003).
>
> [Perakis,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb133)
> [G., Sim, M., Tang, Q., & Xiong, P. (2023). Robust pricing and
> production
> with](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb133)
> [information partitioning and adaptation. *Management Science*,
> *69*(3),
> 1398--1419.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb133)
>
> [Perrault,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb134)
> [A., Wilder, B., Ewing, E., Mate, A., Dilkina, B., & Tambe, M. (2020).
> End-](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb134) [to-end
> game-focused learning of adversary behavior in security games. *Vol.
> 34*, In](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb134)
> [*AAAI Conference on Artificial Intelligence* (02), (pp.
> 1378--1386).](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb134)
>
> [Peršak, E., & Anjos, M. F. (2023). Contextual robust optimisation
> with uncertainty
> quan-](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb135)
> [tification. In *International Conference on the Integration of
> Constraint
> Programming,*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb135)
> [*Artificial Intelligence, and Operations Research* (pp. 124--132).
> Springer.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb135)
>
> [Pineda,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb136) [L.,
> Fan, T., Monge, M., Venkataraman, S., Sodhi, P., Chen, R. T., Ortiz,
> J.,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb136)
>
> [DeTone, D., Wang, A., & Anderson, S. (2022). Theseus: A library for
> differentiable](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb136)
> [nonlinear optimization. In *Advances in Neural Information Processing
> Systems*:
> *vol.*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb136) [*35*,
> (pp. 3801--3818). Curran Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb136)
>
> [Qi,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb137) [M., &
> Shen, Z.-J. (2022). Integrating prediction/estimation and optimization
> with](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb137)
> [applications in operations management. In *Tutorials in operations
> research:
> emerging*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb137)
> [*and impactful topics in operations* (pp. 36--58).
> INFORMS.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb137)
>
> [Qi,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb138) [M.,
> Shi, Y., Qi, Y., Ma, C., Yuan, R., Wu, D., & Shen, Z.-J. M. (2023). A
> practical](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb138)
> [end-to-end inventory management model with deep learning. *Management
> Science*,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb138)
> [*69*(2),
> 759--773.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb138)
>
> [Rahimian,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb139)
> [H., & Pagnoncelli, B. (2022). Data-driven approximation of
> contextual](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb139)
> [chance-constrained stochastic programs. *SIAM Journal on
> Optimization*,
> *33*(3),](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb139)
> [2248--2274.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb139)
>
> Ren, J., Feng, X., Liu, B., Pan, X., Fu, Y., Mai, L., & Yang, Y.
> (2022). TorchOpt: An
>
> []{#_bookmark182 .anchor}efficient library for differentiable
> optimization. arXiv preprint
> [arXiv:2211.06934](http://arxiv.org/abs/2211.06934).
>
> [Rios,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb141) [I.,
> Wets, R. J., & Woodruff, D. L. (2015). Multi-period forecasting and
> scenario](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb141)
> [generation with limited data. *Computational Management Science*,
> *12*(2),
> 267--295.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb141)
>
> []{#_bookmark184 .anchor}[Rockafellar, R. T., & Wets, R. J.-B. (2009).
> *Variational analysis*. Berlin:
> Springer.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb142)
>
> [Rudin,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb143) [C.
> (2019). Stop explaining black box machine learning models for high
> stakes](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb143)
> [decisions and use interpretable models instead. *Nature Machine
> Intelligence*,
> *1*(5),](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb143)
> [206--215.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb143)
>
> [Rust,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb144) [J.
> (1987). Optimal replacement of GMC bus engines: An empirical model
> of](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb144) [Harold
> Zurcher. *Econometrica*,
> 999--1033.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb144)
>
> [Rust,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb145) [J.
> (1988). Maximum likelihood estimation of discrete control processes.
> *SIAM*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb145)
> [*Journal on Control and Optimization*, *26*(5),
> 1006--1024.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb145)
>
> [Rychener,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb146)
> [Y., Kuhn, D., & Sutter, T. (2023). End-to-end learning for
> stochastic](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb146)
> [optimization: A Bayesian perspective. In *International Conference on
> Machine*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb146)
> [*Learning*.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb146)
>
> [Sahoo,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb147) [S.
> S., Paulus, A., Vlastelica, M., Musil, V., Kuleshov, V., & Martius, G.
> (2023).](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb147)
>
> [Backpropagation through combinatorial algorithms: Identity with
> projection
> works.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb147) [In
> *International Conference on Learning
> Representations*.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb147)
>
> [Sang,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb148) [L.,
> Xu, Y., Long, H., Hu, Q., & Sun, H. (2022). Electricity price
> prediction
> for](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb148) [energy
> storage system arbitrage: A decision-focused approach. *IEEE
> Transactions
> on*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb148) [*Smart
> Grid*, *13*(4),
> 2822--2832.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb148)
>
> [Sang,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb149) [L.,
> Xu, Y., Long, H., & Wu, W. (2023). Safety-aware semi-end-to-end
> coordi-](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb149)
> [nated decision model for voltage regulation in active distribution
> network.
> *IEEE*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb149)
> [*Transactions on Smart Grid*, *14*(3),
> 1814--1826.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb149)
>
> Sen, S., & Deng, Y. (2017). Learning enabled optimization: Towards a
> fusion of
>
> statistical learning and stochastic programming. Available at
> [https://optimization-](https://optimization-online.org/?p=14456)
> [online.org/?p=14456](https://optimization-online.org/?p=14456).
>
> Serrano, B., Minner, S., Schiffer, M., & Vidal, T. (2022). Bilevel
> optimization for feature []{#_bookmark193 .anchor}selection in the
> data-driven newsvendor problem. arXiv preprint
> [arXiv:2209.05093](http://arxiv.org/abs/2209.05093).
>
> [Shah,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb152) [S.,
> Wang, K., Wilder, B., Perrault, A., & Tambe, M. (2022).
> Decision-focused](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb152)
> [learning without decision-making: Learning locally optimized decision
> losses. In](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb152)
> [*Advances in Neural Information Processing Systems*: *vol. 35*, (pp.
> 1320--1332).
> Curran](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb152)
> [Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb152)
>
> [Shapiro, A., Dentcheva, D., & Ruszczyński, A. (2014). *Lectures on
> stochastic
> programming:*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb153)
> [*modeling and theory* (second ed.). Philadelphia:
> SIAM.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb153)
>
> [Shlezinger,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb154)
> [N., Eldar, Y. C., & Boyd, S. P. (2022). Model-based deep learning: On
> the](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb154)
> [intersection of deep learning and optimization. *IEEE Access*, *10*,
> 115384--115398.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb154)
>
> [Smith, J. E., & Winkler, R. L. (2006). The optimizer's curse:
> Skepticism and
> postdecision](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb155)
> [surprise in decision analysis. *Management Science*, *52*(3),
> 311--322.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb155)
>
> Srivastava, P. R., Wang, Y., Hanasusanto, G. A., & Ho, C. P. (2021).
> On data-
>
> driven prescriptive analytics with side information: A regularized
> Nadaraya-Watson approach. arXiv preprint
> [arXiv:2110.04855](http://arxiv.org/abs/2110.04855).
>
> []{#_bookmark199
> .anchor}[Stratigakos,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb157)
> [A., Camal, S., Michiorri, A., & Kariniotakis, G. (2022). Prescriptive
> trees](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb157) [for
> integrated forecasting and optimization applied in trading of
> renewable
> energy.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb157)
> [*IEEE Transactions on Power Systems*, *37*(6),
> 4696--4708.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb157)
>
> [Sun,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb158) [X.,
> Leung, C. H., Li, Y., & Wu, Q. (2023). A unified perspective on
> regularization](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb158)
> [and perturbation in differentiable subset selection. In
> *International Conference
> on*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb158)
> [*Artificial Intelligence and Statistics* (pp. 4629--4642).
> PMLR.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb158)
>
> [Sun,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb159) [J.,
> Li, H., & Xu, Z. (2016). Deep ADMM-Net for compressive sensing MRI.
> In](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb159)
>
> []{#_bookmark201 .anchor}[*Advances in Neural Information Processing
> Systems*: *vol. 29*, Curran Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb159)
>
> [Sun,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb160) [C.,
> Liu, S., & Li, X. (2023). Maximum optimality margin: A unified
> approach for](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb160)
> [contextual linear programming and inverse linear programming. In
> *International*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb160)
> [*Conference on Machine Learning* (pp. 32886--32912).
> PMLR.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb160)
>
> [Sun,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb161) [C.,
> Liu, L., & Li, X. (2024). Predict-then-calibrate: A new perspective of
> robust](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb161)
> [contextual LP. *36*, In *Advances in Neural Information Processing
> Systems*.
> Curran](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb161)
> [Associates,
> Inc..](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb161)
>
> [Sun,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb162) [H.,
> Shi, Y., Wang, J., Tuan, H. D., Poor, H. V., & Tao, D. (2023).
> Alternating](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb162)
> [differentiation for optimization layers. In *International Conference
> on Learning*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb162)
> [*Representations*.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb162)
>
> [Sutton,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb163) [R.
> S., McAllester, D., Singh, S., & Mansour, Y. (1999). Policy gradient
> methods](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb163) [for
> reinforcement learning with function approximation. *vol. 12*, In
> *Advances in*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb163)
> [*Neural Information Processing Systems*. MIT
> Press.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb163)
>
> Tang, B., & Khalil, E. B. (2022). PyEPO: A PyTorch-based end-to-end
> predict-then-
>
> optimize library for linear and integer programming. arXiv preprint
> [arXiv:2206.](http://arxiv.org/abs/2206.14234)
> [14234](http://arxiv.org/abs/2206.14234).
>
> [Tian,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb165) [X.,
> Yan, R., Liu, Y., & Wang, S. (2023). A smart predict-then-optimize
> method for](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb165)
> [targeted and cost-effective maritime transportation. *Transportation
> Research, Part
> B*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb165)
> [*(Methodological)*, *172*,
> 32--52.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb165)
>
> [Tian,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb166) [X.,
> Yan, R., Wang, S., & Laporte, G. (2023). Prescriptive analytics for a
> maritime](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb166)
> [routing problem. *Ocean & Coastal Management*, *242*, Article
> 106695.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb166)
>
> [Tian, X., Yan, R., Wang, S., Liu, Y., & Zhen, L. (2023). Tutorial on
> prescriptive
> analytics](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb167)
> [for logistics: What to predict and how to predict. *Electronic
> Research Archive*,
> *31*(4),](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb167)
> [2265--2285.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb167)
>
> [Van](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb168) [Parys,
> B. P., Esfahani, P. M., & Kuhn, D. (2021). From data to
> deci-](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb168) [sions:
> Distributionally robust optimization is optimal. *Management Science*,
> *67*(6),](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb168)
> [3387--3402.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb168)
>
> [Vlastelica,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb169)
> [M., Paulus, A., Musil, V., Martius, G., & Rolinek, M. (2019).
> Differenti-](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb169)
> [ation of blackbox combinatorial solvers. In *International Conference
> on Learning*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb169)
> [*Representations*.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb169)
>
> Vohra, R., Rajaei, A., & Cremer, J. L. (2023). End-to-end learning
> with multiple
>
> modalities for system-optimised renewables nowcasting. arXiv preprint
> [arXiv:2304.](http://arxiv.org/abs/2304.07151)
> [07151](http://arxiv.org/abs/2304.07151).
>
> [Wager,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb171) [S.,
> & Athey, S. (2018). Estimation and inference of heterogeneous
> treatment](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb171)
> [effects using random forests. *Journal of the American Statistical
> Association*,
> *113*(523),](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb171)
> [1228--1242.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb171)
>
> [Wahdany, D., Schmitt, C., & Cremer, J. L. (2023). More than accuracy:
> end-to-end
> wind](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb172) [power
> forecasting that optimises the energy system. *Electric Power Systems
> Research*,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb172)
> [*221*, Article
> 109384.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb172)
>
> Wang, T., Chen, N., & Wang, C. (2021). Distributionally robust
> prescriptive analytics
>
> []{#_bookmark215 .anchor}with Wasserstein distance. arXiv preprint
> [arXiv:2106.05724](http://arxiv.org/abs/2106.05724).
>
> Wang, K., Verma, S., Mate, A., Shah, S., Taneja, A., Madhiwalla, N.,
> Hegde, A., & Tambe, M. (2023). Scalable decision-focused learning in
> restless multi-armed bandits with application to maternal and child
> health. arXiv preprint [arXiv:2202.](http://arxiv.org/abs/2202.00916)
> [00916](http://arxiv.org/abs/2202.00916).
>
> [Wang, K., Wilder, B., Perrault, A., & Tambe, M. (2020). Automatically
> learning
> compact](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb175)
> [quality-aware surrogates for optimization problems. *vol. 33*, In
> *Advances in
> Neural*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb175)
> [*Information Processing Systems* (pp. 9586--9596). Curran Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb175)
>
> [Watson,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb176) [G.
> (1964). Smooth regression analysis. *Sankhya¯ : The Indian Journal of
> Statistics,*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb176)
> []{#_bookmark218 .anchor}[*Series A*, *26*(4),
> 359--372.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb176)
>
> [Wilder,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb177) [B.,
> Dilkina, B., & Tambe, M. (2019). Melding the data-decisions
> pipeline:](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb177)
> [Decision-focused learning for combinatorial optimization. *vol. 33*,
> In *AAAI*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb177)
> [*Conference on Artificial Intelligence* (01), (pp.
> 1658--1665).](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb177)
>
> [Wilder,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb178) [B.,
> Ewing, E., Dilkina, B., & Tambe, M. (2019). End to end learning
> and](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb178)
> [optimization on graphs. *vol. 32*, In *Advances in Neural Information
> Processing
> Systems*.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb178)
> [Curran Associates,
> Inc.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb178)
>
> [Xie,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb179) [X.,
> Wu, J., Liu, G., Zhong, Z., & Lin, Z. (2019). Differentiable
> linearized
> ADMM.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb179) [In
> *International Conference on Machine Learning* (pp. 6902--6911).
> PMLR.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb179)
>
> [Xu,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb180) [Y., &
> Cohen, S. B. (2018). Stock movement prediction from tweets and
> historical](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb180)
> [prices. In *Proceedings of the 56th Annual Meeting of the Association
> for
> Computational*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb180)
> [*Linguistics* (pp. 1970--1979). Association for Computational
> Linguistics.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb180)
>
> [Yan,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb181) [R.,
> Wang, S., Cao, J., & Sun, D. (2021). Shipping domain knowledge
> informed](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb181)
> [prediction and optimization in port state control. *Transportation
> Research, Part
> B*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb181)
> [*(Methodological)*, *149*,
> 52--78.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb181)
>
> [Yan,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb182) [R.,
> Wang, S., & Fagerholt, K. (2020). A semi-''smart predict then
> optimize''](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb182)
> [(semi-SPO) method for efficient ship inspection. *Transportation
> Research, Part
> B*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb182)
> [*(Methodological)*, *142*,
> 100--125.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb182)
>
> [Yan,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb183) [R.,
> Wang, S., & Zhen, L. (2023). An extended smart ''predict, and
> optimize''](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb183)
> [(SPO) framework based on similar sets for ship inspection planning.
> *Transportation*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb183)
> [*Research Part E: Logistics and Transportation Review*, *173*,
> Article
> 103109.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb183)
>
> Yang, J., Zhang, L., Chen, N., Gao, R., & Hu, M. (2023).
> Decision-making with side
>
> information: A causal transport robust approach. Available at
> [https://optimization-](https://optimization-online.org/?p=20639)
> [online.org/?p=20639](https://optimization-online.org/?p=20639).
>
> []{#_bookmark227
> .anchor}[Yanıkoğlu,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb185)
> [I., Gorissen, B. L., & den Hertog, D. (2019). A survey of adjustable
> robust](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb185)
> [optimization. *European Journal of Operational Research*, *277*(3),
> 799--813.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb185)
>
> [Zhang,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb186) [Y.,
> & Gao, J. (2017). Assessing the performance of deep learning
> algorithms](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb186)
>
> [for newsvendor problem. In D. Liu, S. Xie, Y. Li, D. Zhao, & E.-S. M.
> El-Alfy](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb186)
> [(Eds.), *Neural information processing* (pp. 912--921). Cham:
> Springer
> International](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb186)
> [Publishing.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb186)
>
> Zhang, Y., Liu, J., & Zhao, X. (2023). Data-driven piecewise affine
> decision rules for stochastic programming with covariate information.
> arXiv preprint [arXiv:2304.](http://arxiv.org/abs/2304.13646)
> [13646](http://arxiv.org/abs/2304.13646).
>
> []{#_bookmark230
> .anchor}[Zhang,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb188)
> [L., Yang, J., & Gao, R. (2023). Optimal robust policy for
> feature-based](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb188)
> [newsvendor. *Management
> Science*.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb188)
>
> Zhang, C., Zhang, Z., Cucuringu, M., & Zohren, S. (2021). A universal
> end-to-end
>
> approach to portfolio optimization via deep learning. arXiv preprint
> [arXiv:2111.](http://arxiv.org/abs/2111.09170)
> [09170](http://arxiv.org/abs/2111.09170).
>
> [Zhu,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb190) [T.,
> Xie, J., & Sim, M. (2022). Joint estimation and robustness
> optimization.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb190)
>
> []{#_bookmark232 .anchor}[*Management Science*, *68*(3),
> 1659--1677.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb190)
>
> [Ziebart,](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb191) [B.
> D., Maas, A. L., Bagnell, J. A., & Dey, A. K. (2008). Maximum
> entropy](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb191)
> [inverse reinforcement learning. *Vol. 8*, In *AAAI Conference on
> Artificial
> Intelligence*](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb191)
> [(pp. 1433--1438). Chicago, IL,
> USA.](http://refhub.elsevier.com/S0377-2217(24)00220-0/sb191)
