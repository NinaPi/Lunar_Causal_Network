# Lunar_Causal_Network
Created for Stony Brook University's Simons Summer Research Program in 2021.  

**Problem:**   
Reinforcement Learning models are ubiquitous, but many are black box models, meaning they are created directly from data by an algorithm without insight into their structure. With these models, humans, even those who design them, cannot understand how variables are being combined to make predictions. What if we want to know why a model took an action? Why a RL-powered military weapon decides to make a launch? Investors are less likely to invest in and users are less likely to deploy a model they can’t understand, trust, or control.  

**Objective:**   
To use algorithmic interference to reconstruct the path of decision making. The goal is to make causal network that visualizes the effect of actions on state and the effect of state on reward. The network would act as a cause and effect chart for the model decision making process.  
  
**Background:**  
_The Environment:_ Many applications of RL are in robotics, especially in space and the military, so I chose to use Open AI Gym’s simulation of the 70s-era Lunar Lander game to make the causal network because it is a simple rendering of a typical robotics environment. The Lunar Lander's goal is to to land between the flags, and it receives a reward for landing and leg contact and a penalty for crashing or firing the engine. There are 4 distinct Actions: do nothing (0), fire left engine (1), fire main engine (2), fire right engine (3). There are 8 state variables that describe the environment: x/y position, x/y velocity, lander angle, angular velocity, L/R contact point.  

_RL Model:_ Used the a DQN (Deep Q Network) to train an RL model on the environment. Problem considered 'solved' if the model acheives a reward of +200 for 100 consecutive episodes. Solved in 310 episodes.  

_Replay Data:_ As the model was training, I saved Replay Data, representing the impact of each action. For each step, it records the tuple (S_t, A_t, R_t, S_t+1) where S_t represents state at time T, action taken at time T, reward for taking action at time T, and state at time T+1, state after action is taken.   
  
**Findings:**  
_Project Step 1_: Finding impact of each individual action on the new state. Splitting replay data by action to determine its effects on each state. Storing the delta value for each state variable before and after the action |St - St+1 |. Therefore, the delta value for each state represents the change caused by the action. To determine correlation between nodes, I used the Fast Conditional Independence Test (FCIT). Measuring fcit correlation between every state and its delta value. Using the 5% p-value test to determine threshold for statistical significance for each state. If the correlation between nodes was significant, I drew an edge. Each edge asks the question: "Is the current state of varA the trigger for the model deciding to take an action that changes varB?"  

_Project Step 2_: Computing a compressed representation by merging "state" and "state_delta" nodes. While Project Step 1 shows a series of calculations, Project Step 2 whows the true decision making process for each action with the real state names.  

_Project Step 3_: Compressing all 4 action graphs to visualize the effect of each action on the environment states. Instead of having 4 different action graphs, we have one graph with edges labeled by the actions corresponding to them. Computing the effect of state values on the total return (reward). Reward node is yellow. This network is a thorough representation, but not super readable.  

_Project Step 4_: Making the network more readable. The User can decide to look at a certain percent of the most significant edges (ordered by statistical significance). Although including fewer edges makes the network a bit less thorough, it allows for increased readability and understanding for the user. The user also has the ability to highlight each action's impact within the larger network.  
  
**Next Steps:**     
Continue to increase readability by create an interactive interface so an user can easily toggle between highlighting actions and simplifying/complicating the graph view. Auto-print explanations rather than the user having to interpret the graph on their own.  

**Conclusions:**   
By tracking the impact of each action, casual networking for RL models helps a user understand their model choices, making it more reliable and marketable. By converting complex data patterns into interpretable graphs, we are more able to bridge the gap between coding and its social implications. In a larger context, perhaps if we are able to explain algorithmic decision making, we can explain human thinking and predict what actions people will take in certain environments.  

**Acknowledgments:**    
Nina would like to thank the Stony Brook Simons Summer Research Program and the VAI Lab for providing her with this wonderful opportunity to conduct research. She would like to give her most sincere thanks to Dr. Klaus Mueller and Dr. Eric Papenhausen for their instruction this summer.  

**References:**  
Madumal, P; Miller, T; Sonenberg, L; & Vetere, F. (2019) Explainable Reinforcement Learning Through a Causal Lens. Retrieved from https://arxiv.org/abs/1905.10958.
Jun Wang and Klaus Mueller.   
Wang, J & Mueller, K. (2016) The Visual Causality Analyst: An Interactive Interface for Causal Reasoning. Retrieved from https://www3.cs.stonybrook.edu/~mueller/papers/causalAnalyst.pdf.   
Lunar Landing Environment: https://gym.openai.com/envs/LunarLander-v2/  
Lunar RL model: https://github.com/shivaverma/OpenAIGym/blob/master/lunar-lander/discrete/lunar_lander.py  
FCIT: https://github.com/kjchalup/fcit  
Schneier, B. (2021, April 19). Hackers used to be Humans. Soon, AIs will Hack Humanity. Wired. https://www.wired.com/story/opinion-hackers-used-to-be-humans-soon-ais-will-hack-humanity/?bxid=5cec270f3f92a45b30f0ccfd&cndid=28931753&esrc=Wired_etl_load&source=EDT_WIR_NEWSLETTER_0_DAILY_ZZ&utm_brand=wired&utm_campaign=aud-dev&utm_mailing=WIR_Daily_041921&utm_medium=email&utm_source=nl&utm_term=list2_p3   
