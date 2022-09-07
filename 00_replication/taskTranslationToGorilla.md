# Translating Psychtoolbox task into Gorilla

- Approach: Gorila Task Builder 1 with Task Builder Script  

- Task details to be translated:  
  - Random ITI for fixation cross (4-7 secs)  
  - Selection of fractal images per subject
  - Drifting fractal reward probabilities
  - Generation of lottery images
    - Function to convert lottery probability into pie chart? Or upload stimuli images for all possible pies?
  - Where is the instruction clarifying the reference lottery?
    - Immediately after the initial instructions. See `RunSession.m` L44. Presented for 4 seconds as a pie chart with the value in the bottom just like all other left lotteries in the rest of the task
  - Add "prepare to begin screen" after the reference lottery presentation
  - Is pFractal determined in advance or selected randomly in each trial?
    - Block of 60 probabilities for each run is shuffled in `SetupSession.m` L129
    - Lotteries are determined by first appending the 20 lotteries to make an array with sufficient values for the number of trials in the run and then shuffling this new array. Each lottery appears 3 times in each run but with a randomly chosen relevance
    - Reward probabilities for both fractals are pre-computed (independently for each fractal) before each run for the 60 trials of the run within the drift boundaries
    - ITIs for the 60 trials of the run are also determined in advance from a random shuffling of an array with 60 values ranging between 4 and 7. Then, in `RunTrial.m` they are incremented by the remainder from the previous trial when presenting the fixation screen
  - Red box around the rewarded attribute of chosen bundle
  - Fractal outcomes after each trial
  - Trial reward after each trial
  - Is total reward up to that point presented during the task?
    - Reward is computed after each run by randomly picking 35 trials from the run and summing the reward from those trials
    - After each run the reward for that run only is presented as `Amount earned: ...`
    - Each session in the scanner ends with `End of session. Please wait for experimenter` but this is not necessary for Gorilla. Replace it with something like `End of block. Please click continue when ready to begin the next block`. Maybe add a mandatory wait period for this too before the continue button appears.
    - Total reward for the experiment is the sum of rewarded trials from each run
    - At the end of all sessions check to make sure the total reward is within the min and max reward bounds. These are set as $80 and $120 in `SetupExperiment.m`.
    - If no response within 4 seconds show `No response recorded!`
  - Compute task reward

- Things that can be precomputed  
  - probFractalDraw
  - fractal reward probs
  - lotteries
  - fractal images

- Things that need to be updated   
  - trialwise: ITI  
  - runwise: run reward  
  - experimentwise: total reward

- How long does the task take?
