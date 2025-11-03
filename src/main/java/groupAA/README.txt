GroupAA MCTS Agent (Sushi Go)
=============================

This project implements a Monte Carlo Tree Search (MCTS) agent with progressive bias augmented UCB as the tree policy
and a Greedy One-Step Lookahead Rollout Policy with an Expected Utility Maximization (EUM) Heuristic + light-weight Opponent Modelling
as part of the AI Tournament framework.

--------------------------------------------------------------------
PROJECT STRUCTURE
--------------------------------------------------------------------

src/
 └── main/java/
           └── groupAA/
               ├── SushiGoAgentGroupAA.java          -> Main agent class (entry point for tournament)
               ├── GroupAATreeNode.java              -> Core MCTS node with UCB, Progressive Biasing, and rollout logic
               ├── GroupAAParams.java                -> Parameter tuning and configuration
               ├── GroupAAHeuristic.java             -> Domain-specific heuristic for evaluating states
               ├── GroupAARolloutPolicy.java         -> Interface for rollout policy
               ├── GroupAAGreedyRolloutPolicy.java   -> Default greedy rollout using heuristic
               ├── groupAA_mcts.json                 -> Agent configuration file (used in tournament)

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
  - Epsilon noise constant
  - Rollout policy (default: GroupAAGreedyRolloutPolicy)
  - State heuristic (GroupAAHeuristic)

--------------------------------------------------------------------
RUNNING THE TOURNAMENT
--------------------------------------------------------------------

1. Ensure the tournament configuration file is created:
   (e.g., `config/json/RunGames/GroupAA_Tournament.json`)

2. From the project root, execute the following command:

       java -cp "target/classes:lib/*" evaluation.RunGames config=json/config/RunGames/GroupAA_Tournament.json

   (You can also run it directly from your IDE by setting the CLI argument:)

       config=json/config/RunGames/GroupAA_Tournament.json

   The TAG framework will automatically:
     - Parse the JSON configuration
     - Load the agents defined in the `config/json/agents` folder
     - Run all matchups as per the tournament mode (e.g., Random or Exhaustive)
     - Generate a summary of results (win rates, mean ordinals, etc.) in TournamentResults.txt and GAME_OVER.csv

--------------------------------------------------------------------
CONFIGURATION PARAMETERS
--------------------------------------------------------------------

All parameters are defined in GroupAAParams.java as TunableParameters.

K              - Exploration constant for UCB                  (fine-tuned: 0.7)
biasWeight     - Weight of heuristic progressive bias          (default: 0.6)
rolloutLength  - Max rollout simulation depth                  (fine-tuned: 20)
maxTreeDepth   - Maximum MCTS tree depth                       (fine-tuned: 100)
epsilon        - Small numeric constant to avoid division by 0 (default: 1e-6)
heuristic      - Domain heuristic (GroupAAHeuristic)
rolloutPolicy  - Rollout strategy (default: GroupAAGreedyRolloutPolicy)

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
AUTHORS
--------------------------------------------------------------------

Team GroupAA
  - Anoop Senthil
  - Vinh Bao Huan Hoang
  - Trijit Reet Adhikary