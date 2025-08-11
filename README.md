![image](https://github.com/user-attachments/assets/362f9f1c-c5f7-4cbc-9416-e2cf087f7cde)

# Tic-Tac-Toe Best Move Prediction with GPT-2
This project aims to evaluate how well GPT-2 can generate optimal Tic-Tac-Toe moves compared to the classical minimax algorithm.

# Overview
The goal of the project is to prompt GPT-2 with a Tic-Tac-Toe board state and determine if it can output the best possible move. I will compare GPT-2’s predictions against the minimax algorithm, which always plays optimally, and measure the accuracy as well as the number of invalid moves.
The data consists of pre-generated board states with corresponding best moves, including cases where GPT-2’s outputs are replaced with minimax moves if invalid.
While GPT-2 is not trained specifically for game strategy, this project will help evaluate whether a language model can reason about a structured board game without fine-tuning. Unfortunatly, the accuracy was 0%.

# Summary
- ## Data
  - Dataset consists of board states stored as text prompts and labeled optimal moves.
  - Each board includes:
    - X positions
    - O positions
    - Empty spaces
    - Current player
  - Labels are the best moves calculated via minimax.
  - Data format: JSON Lines (.jsonl) with prompt and completion.
  - Dataset size: thousands of states, generated from all possible valid game states.
- ## Preprocessing
    - Parsed text prompts into structured 3×3 board arrays
    - Converted board to integer encoding (0 empty, 1 X, 2 O) for minimax checks.
    - Extracted legal moves and validated both GPT-2 predictions and ground truth moves.
    - If a predicted or labeled move was illegal, replaced with minimax best move.
- ## Visualization
    - ### Prompt:
        X: (0,1), (0,2), (1,0), (1,1), (1,2), (2,2)
        O: (0,0), (2,0)
        Empty: (2,1)
        Player: O
        Best move (row, col) is:
      - As you can see there is a few problems here I will make apparent. The generation of the data was correct and very thoroughly tested, however, due to what I believe was inncorrect prompts, many problems arose.
    - ### Outputs:
        Best Move: (2, 1) [INVALID MOVE - replaced with minimax best move]
        Ground Truth: (2, 1) [INVALID GT - replaced with minimax best move]
      -Due to improper prompts, we get this Invalid move notification, leading too the terrible accuracy


  ## Problem Formulation
    - The original goal was to determine if GPT-2 could generate valid and optimal Tic-Tac-Toe moves from textual board     descriptions. Initially, the task was straightforward: give GPT-2 a formatted board state and measure how often it matches the optimal minimax move. Early results showed GPT-2 often produced invalid moves, requiring a fallback to minimax.
    - The goal became:
        - Measure GPT-2’s raw accuracy vs. minimax.
        - Track invalid move frequency.
        - Explore whether prompt adjustments improve move validity.


## Training
- Software: Jupyter Notebook, Python 3, Hugging Face transformers, distilgpt2, custom minimax implementation.


## Performance 
- Unfortunalty, the performance was terrible. GPT-2 produced invalid moves 100% of the time. All ground truths were also invalid, as they would choose spots that had already been filled. Without more work being done, the invalid outputs were replaced with empty spots on the board. This means instead of predicting the best move, a random empty spot would be picked.

## Conclusions
Initial results show that GPT-2 struggles to produce valid Tic-Tac-Toe moves when given a textual board description. The model often outputs coordinates corresponding to already-filled squares. This suggests GPT-2 lacks the implicit “board legality” reasoning without explicit instruction or fine-tuning. However, GPT-2 can still be used as a move generator if paired with a minimax validation/fallback system, ensuring all moves are legal and optimal.

## Future Plans
- Fine-tune GPT-2 on the Tic-Tac-Toe dataset to teach it move legality and strategy.
- Explore even more prompts to reduce invalid move rate (e.g., explicitly listing legal moves in prompt).
- Compare GPT-2 performance to smaller and larger transformer models.
- Extend to more complex games (e.g., Connect Four) to see if similar patterns emerge.

## Downlaoding Data
The dataset (tictactoe_bestmove.jsonl) is locally generated using minimax for ground truth labeling. It can be regenerated from scratch using the provided Python scripts to enumerate valid board states and calculate optimal moves.

## Notebook 
The notebook LLM_Chess.ipynb connected to the github is where all the work was done. Work needs to be done on organization, and clearing out old code.
