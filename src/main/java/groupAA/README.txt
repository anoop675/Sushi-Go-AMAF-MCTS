GroupAA MCTS Agent (Sushi Go)
=============================

This project implements a Monte Carlo Tree Search (MCTS) agent with progressive bias
and optional RAVE (Rapid Action Value Estimation) enhancements for the game Sushi Go!
as part of the AI Tournament framework.

--------------------------------------------------------------------
PROJECT STRUCTURE
--------------------------------------------------------------------

src/
 └── groupAA/
     ├── SushiGoAgentGroupAA.java          -> Main agent class (entry point for tournament)
     ├── GroupAATreeNode.java              -> Core MCTS node with UCB, RAVE, and rollout logic
     ├── GroupAABasicTreeNode.java         -> Simplified baseline MCTS node (for testing)
     ├── GroupAAParams.java                -> Parameter tuning and configuration
     ├── GroupAAHeuristic.java             -> Domain-specific heuristic for evaluating states
     ├── GroupAARolloutPolicy.java         -> Interface for rollout policy
     ├── GroupAAGreedyRolloutPolicy.java   -> Default greedy rollout using heuristic
     ├── GroupAARandomRolloutPolicy.java   -> Optional purely random rollout

--------------------------------------------------------------------
AGENT INITIALIZATION
--------------------------------------------------------------------

Entry point class:
  SushiGoAgentGroupAA.java

Initialization:
  The tournament engine automatically creates the agent instance by calling:

      new SushiGoAgentGroupAA(new GroupAAParams(), "GroupAA MCTS Agent");

GroupAAParams loads the following configurations:
  - Exploration constant (K)
  - Progressive bias weight (biasWeight)
  - Maximum tree depth
  - Rollout policy (default: GroupAAGreedyRolloutPolicy)
  - State heuristic (GroupAAHeuristic)

--------------------------------------------------------------------
ALGORITHM OVERVIEW
--------------------------------------------------------------------

Monte Carlo Tree Search (MCTS) is used as the decision-making algorithm.
The process includes:

1. Selection:
   Nodes are selected using UCB with progressive bias (and optional RAVE):

       UCT = Q + K * sqrt(ln(N_parent) / (N_child + ε))

2. Expansion:
   A new child node is created for the chosen action.

3. Simulation (Rollout):
   The GreedyRolloutPolicy simulates the rest of the game using a heuristic:

       score = heuristic.evaluateState(state, playerId);

4. Backpropagation:
   The results are propagated up the tree.
   When RAVE is enabled, AMAF statistics are also updated.

--------------------------------------------------------------------
RAVE (Rapid Action Value Estimation)
--------------------------------------------------------------------

RAVE combines direct node statistics with AMAF (All Moves As First) values
to improve early estimations. Controlled via parameter "raveK" in code.
Setting raveK = 0.0 disables RAVE.

--------------------------------------------------------------------
RUNNING THE AGENT IN TOURNAMENT
--------------------------------------------------------------------

1. Compile the project:

       javac -d out -cp "lib/*:src" src/groupAA/*.java

2. Package into a JAR file:

       jar cf GroupAAAgent.jar -C out .

3. Run the tournament:

       java -cp "GroupAAAgent.jar:lib/*" games.sushigo.SushiGoTournament

The tournament engine automatically detects all agents in package "groupAA"
and pits them against others.

--------------------------------------------------------------------
RUNNING A SINGLE MATCH
--------------------------------------------------------------------

You can run a quick match manually:

       java -cp "GroupAAAgent.jar:lib/*" core.Tournament --game SushiGo --players groupAA.SushiGoAgentGroupAA otherpackage.RandomAgent

Or:

       java -cp "GroupAAAgent.jar:lib/*" games.sushigo.SushiGoGame --player groupAA.SushiGoAgentGroupAA

--------------------------------------------------------------------
CONFIGURATION PARAMETERS
--------------------------------------------------------------------

All parameters are defined in GroupAAParams.java as TunableParameters.

Parameter      Description                        Default
--------------------------------------------------------------------
K              Exploration constant               0.7
biasWeight     Progressive bias weight             0.6
rolloutLength  Maximum rollout depth               20
maxTreeDepth   Tree search limit                   100
raveK          RAVE mixing constant (optional)     0.5

--------------------------------------------------------------------
LOGGING
--------------------------------------------------------------------

Logging is handled with java.util.logging.Logger.
Examples of log outputs:
  - "Performing selection using UCB with progressive bias"
  - "Selected action leading to better heuristic value"
  - "Best action selected after MCTS"

To enable logs, run Java with:
  -Djava.util.logging.config.file=logging.properties

--------------------------------------------------------------------
TIPS
--------------------------------------------------------------------

- If RAVE underperforms, set raveK = 0.0 to disable it.
- If exploration is too high, reduce K to 0.5–0.7.
- Use GroupAAGreedyRolloutPolicy for heuristic-guided rollouts.
- Use GroupAARandomRolloutPolicy for more stochastic rollouts.

--------------------------------------------------------------------
AUTHORS
--------------------------------------------------------------------

Team GroupAA
  - Anoop Senthil
  - Vinh Bao Huan Hoang
  - Trijit Reet Adhikary