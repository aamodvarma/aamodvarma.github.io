---
permalink: /research/funglab
title: Introduction
author_profile: true
---
{% include base_path %}

The goal of this research project is to leverage large language models to conditionally and unconditionally generate novel chemical compositions aiding in material discovery. These generated compositions can then be ran through a structure prediction algorithm. In this project I fine tune GPT-2 to generate chemical compositions using the mp20 dataset. 

## Tokenizer
As I am working with a custom dataset of compositions, I trained the following tokenizers on our dataset,

1. BPE Tokenizer
2. Word Level Tokenizer

Out of the two the BPE tokenizer performed poorly for our dataset as it generated compositions with invalid elements which was expected. However the Word Level tokenizer worked in generating compositions with valid elements/tokens. I also added a END token to our tokenizer as we wanted our model to stop when an END token was printed out rather than giving it a specific length crietera.

## Model
I used the `AutoModalForCausalLM` with the GPT-2 config for this task. The model was fine tuned on our dataset of around 20,000 compositions using our word level tokenizer. Each composition was padded to length 21 using padding tokens which in our case was the same as our end token.

**Unconditional Generation**
For unconditional generation the goal was to generate compositions without a specific input token. The model was trained for 100 epochs and for the generation I used a `temperature` of 0.9 to ensure we have novel compositions.

Initially, starting token, length and number of compositions are taken as input to generate the compositions. However to make sure we do not lose information because of the specified length, END tokens were used and the model would automatically generate the END token which we would consider the end of the composition.


**Conditional Generation**
Rather than have the model randomly start with a token, if we want to generate a composition according to our liking we can use conditional generation. Each composition in the mp20 dataset was assigned an embedding vector generated through a Graph Neural Network. The idea was to use this vector as the initial starting token having the compositions assigned to it following this token.

The associated GNN embedding was 384 dimensional so first this was projected onto a 768 dimensional space as this is the embedding dimension for GPT2. For this purpose I created a pytorch `DenseModel` with randomly initialized weights taking in our input embedding and outputting a 768 dimensional vector. Then each embedding was passed through this layer to generate our projection. 

Once I had my high dimensional dense vector during the training loop while iterating over each batch in my train dataloder I extract the input embedding. I then iterate over the 8 input embeddings and query my dataset for the associated dense vector and input that append that to the beginning of my sequence of vectors. A similar process was done to calculate the validation loss. 

While generating new compositions I am able to input a dense vector as the input embedding to prompt the model to generate compositions.

# Validity
To validate generated compositions we used the smact library in python. First all generated composition are added to a dictionary in the following format,

```python
"H H O" -> {(("H", "O"), 0)) : [2, 1]}
"C O O" -> {(("C", "O"), 1)) : [1, 2]}
```

Using the smact library we also conduct a pauling test and finally validated the composition. Across our model we got a validity score of up to 75% for the newly generated composition.





