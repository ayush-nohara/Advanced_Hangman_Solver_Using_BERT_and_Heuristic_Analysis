# Advanced_Hangman_Solver_Using_BERT_and_Heuristic_Analysis

## Hangman Challange
       
# Approach Overview:
# Tokenizer Training: 
                The script employs a Byte-Level Byte Pair Encoding (ByteLevelBPE) tokenizer trained on the words_250000_train.txt dataset. This tokenizer is essential for segmenting text into subword units, enabling more efficient processing and representation of the vocabulary.

# Roberta Model Configuration:
# Custom Parameters:
                The Roberta model is configured with specific parameters to optimize its performance for masked language modeling (MLM). 

# These parameters include:
# vocab_size: Set to accommodate the vocabulary derived from the tokenizer.

# max_position_embeddings:
               Defines the maximum sequence length the model can handle.
               num_attention_heads and num_hidden_layers: Control the model's capacity for attention and depth of representation, respectively.

# type_vocab_size: 
               Specifies the number of different token types the model can distinguish.

# Data Preparation:
# Tokenization: 
               The dataset (words_250000_train.txt) is tokenized using LineByLineTextDataset, which prepares the data for MLM training by breaking it into manageable sequences suitable for training the Roberta model.

# Training and Saving:
# Model Training: 
               The Roberta model is trained using the Trainer class, which facilitates efficient training with features like data collation and gradient optimization. The training process is tailored for MLM tasks, where the model learns to predict masked tokens within text sequences.

# Checkpointing:  
              After training, the final trained model is saved to a specified checkpoint directory (/kaggle/working/checkpoints), ensuring that the trained weights and configurations can be retrieved and reused for subsequent tasks or deployments.

# Inference Pipeline:
              Prediction Capability: An fill-mask pipeline is set up post-training to enable the model to predict masked tokens in new input text. This functionality is crucial for applications requiring automated text completion or prediction tasks, such as filling in missing words in sentences.


# Heuristic approach:
              In my project, I use a heuristic approach to predict letters in masked word patterns by analyzing their frequency within a large dataset of words. Specifically, the function most_frequent_letter_in_masked_positions takes a word pattern with unknown letters, represented by dots (e.g., ".o..s"), and searches through the word list to find all matches. For each matching word, it counts the frequency of letters that appear in the masked positions. This frequency data is then used to identify which letters are most likely to fit the pattern. By leveraging this heuristic method, I can make informed guesses about the masked letters efficiently. This approach is particularly useful in applications such as word-guessing games, text analysis tools, and predictive text systems, where quick and reasonably accurate predictions are more valuable than exhaustive searches. This method balances computational efficiency and practical accuracy, making it a robust solution for real-time decision-making scenarios.
### Approach 
        # 1. Getting the first word right 
                # use heuristics to guess the most likely letter given the length of the word

        # 2. If not first word, then use BERT to guess the most likely letter with high confidence
                # using 0.6 as probability masking
                    # using 0.15 as the threshold for high confidence : 42 - 48.05% accuracy
                    # using 0.10 as the threshold for high confidence : 46 - 50.5% accuracy
                # using 0.75 as probability masking
                    # using 0.15 as the threshold for high confidence : 43 - 47.83% accuracy
                    # using 0.10 as the threshold for high confidence : 45 - 49.88% accuracy

        # 3. When BERT runs out of ideas or has less confidence, search through the dictionary for the most common letter in the masked positions
                # use 0.00 as the threshold for percentage of occurances : 50 - 55.5% accuracy
                # use 0.10 as the threshold for percentage of occurances : 51 - 54.77% accuracy

        # 4. Use low confidence BERT guesses, if any
                # use the remaining guesses from BERT

        # 5. Use the ordering of full dictionary as a last resort
                # here it's as good as random guessing

### Experiments

There are different ways to mix LM and heuristic approach

- Thresholding at 0.15 confidence, use word match only if greater than 0.10 confidence results in 

> 48 - 51.02% success

- Use only BERT

> 48 - 49.78% success

- Use heuristics first to unlock a fraction of the word (25 - 35)% and then use BERT

> 47 - 50.08% success
