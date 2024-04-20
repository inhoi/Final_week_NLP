# Final_week_NLP

## Normal Mode

* The model predicts the next token based on the sentence it is currently generating.
* This mode can generate a continuous text flow, even if the input sentence is grammatically incorrect or nonsensical.

$$ p(x_t | x_1, ..., x_{t-1}) $$

** Advantages **

1. Able to predict the next word or symbol regardless of the form or quality of the input generated so far.

2. Helps in maintaining a natural flow of the sentence by continuing the context, even if the input doesn't make complete sense.

** Disadvantages **

1. Relies solely on the data it generates, which can degrade the quality of the output if the input data is of low quality or the training data is insufficient 

2. Known issue where the model might generate errors that accumulate over time due to the discrepancy between training data and real-world application


## Teacher Forcing

* A method where the model predicts the next token based on the actual previous token from the training data.
* Primarily used during training, this method ensures high efficiency by providing the model with the correct previous token.

$$ p(x_t | x_1, ..., x_{t-1}, y_{t-1}) $$

** Advantages **

1. Ensures the use of correct data for training, allowing the model to learn patterns more precisely.
   
2. As the model already knows the correct next word, the training process progresses quickly.
   
3. By using accurate next tokens during training, the model can avoid the cascading errors that lead to gradient explosion, maintaining stability in the training process.

** Disadvantages **

1. Since the model always relies on the correct input during training, it may not develop the ability to correct its own mistakes when deployed

## Scheduled Sampling

* A hybrid training methodology that blends Teacher Forcing and Normal Mode, gradually transitioning from the former to the latter during the training process.
* This approach begins with a high reliance on Teacher Forcing to establish accurate data patterns and gradually increases the use of Normal Mode, allowing the model to use its own predictions as inputs.

$$ p_t = decaying function(t) $$

$$ x_t = y_{t-1} -->     p_t $$

$$ x_t = \hat{x}_{t-1} -->       (1 - p_t) $$


##  Why do we choose decoder only model over encoder only model for text generation?

1. Cost of Training

* Multitask Finetuning: Encoder-Decoder (ED) models require multitask finetuning on labeled data to achieve maximum potential, which can be costly, especially for larger models.
* Zero-Shot Generalization: Causal Decoder (CD) models excel in performance due to their strong zero-shot generalization capabilities, aligning well with self-supervised learning on large-scale corpora, thus potentially reducing training costs.

2. In-Context Learning from Prompts

* Prompt Engineering: Applying few-shot examples through prompt engineering helps Large Language Models (LLMs) understand context or tasks.
* Decoder vs. Encoder-Decoder: Prompting has a more straightforward effect on decoder-only models due to their lack of need for translating context into an intermediate form before generation. Encoder-decoder models can also benefit, but optimal performance requires precise tuning of the encoder.

3. Efficiency Optimization

* Reuse of K and V Matrices: Decoder-only models enhance efficiency by reusing Key (K) and Value (V) matrices from previous tokens during the decoding process.
* Causal Attention Mechanism: Due to causal attention, K and V matrices for past tokens remain unchanged, reducing the need for recomputation and thus lowering computational costs and speeding up generation during inference.

4. Autoregressive vs. Bidirectional Attention

* Attention Mechanism: Decoder-only models use autoregressive (causal) attention, leading to a constrained, lower triangular attention matrix which maintains full rank status, suggesting a theoretically stronger expressive capability.
* Bidirectional Attention Limitations: Bidirectional attention in encoder-decoder architectures does not guarantee full rank status of the attention matrix, which may limit model performance.
* Experimental Findings: An experiment splitting bidirectional attention into Forward-Backward (FB) attention showed only marginal improvements over full bidirectional attention, suggesting minimal impact on performance when models are well-trained.
* Learning Implications: Bidirectional attention can expedite learning but may inhibit the acquisition of deeper predictive patterns necessary for effective generation.


