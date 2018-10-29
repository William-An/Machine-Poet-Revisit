# Machine Poet
## Mechanism
1. Language Model
1. Word Embedding
1. LSTM NN
1. Sampling
## Model
1. Data Input
    1. [x]Textline Reader (TF)
        1. Every poem is in one line
        1. Special Tokens
            1. `xxxnewlinexxx`
            1. `xxxstartxxx`
            1. `xxxendxxx`
            1. `xxxendofstanzaxxx`
    1. [x]Words2id
        1. Dictionary
            1. https://www.tensorflow.org/api_docs/python/tf/contrib/lookup
        1. Yes Punctuation
            1. Preprocess
                1. `re.sub`
                1. Insert ` ` around punctuations?
1. Word Embedding (Tensorflow)
    1. Model based on [Tensorflow-hub](https://www.tensorflow.org/hub)
    1. Tansfer Learning based on text materials from poet
1. Model (Tensorflow, LSTMCell)
    1. ~~Structure~~
        1. [x]Input Function
            1. Word
        1. Embedding
            1. Lookup in the dictionary
            1. Embed using the existing model from Tensorflow Hub
        1. LSTM layer
        1. Dropout
        1. LSTM
        1. Dropout
        1. Dense
            1. To Size of the dictionary
        1. Softmax
            1. Output Shape shall be the size of dictionary
    1. Variables initialization
    1. Train (Cell based)
        1. For-loop as Steps
            1. [x]Initialize States for LSTM Cell
            1. For-loop for time step
                1. Input - batch with padding/mask
                    1. Shape
                        1. [Batch_size, Sequence_length, Embedding_Size]
                    1. ~~`tf.pad`~~
                        1. Should pad until the longest sequence end in the batch
                        1. Pad at when D=1 after the sequence
                        1. Question: How to ensure the padded will have the same sequence_length?
                    1. [tf.keras.preprocessing.sequence.pad_sequences](https://www.tensorflow.org/api_docs/python/tf/keras/preprocessing/sequence/pad_sequences)
                        1. Need to convert words back to indices
                    1. [tf.nn.dynamic_rnn](https://www.tensorflow.org/api_docs/python/tf/nn/dynamic_rnn)
                        1. Input as placeholder with shape [Batch_size, Max_length, Embedding_Size]
                        1. So input should be initialize as all zeros? then fill with data?
                    1. [x]Or `Batch_size` = 1
                1. [x]State
                1. [x]LSTM cell
                1. [x]Dense
                1. [x]Calculate State
                1. Calculate Loss
        1. Loss
        1. Train_Ops
    1. Sampling
        1. Initialize States for LSTM Cell
        1. Initialize input for LSTM Cell 
        1. Run once and randomly select words as y_0
        1. Calculate state
        1. While-loop until `xxxendxxx` token is output
            1. Input
            1. State
            1. LSTM cell
            1. Dense
            1. Calculate State
            1. Calculate y_hat as next input
            1. Print y_hat
    1. TODO 
        1. BidirectionalLSTM
        1. GRU
1. Model (TF Estimator)
    1. Data input label will be series of vectors as indices from the dictionary
    1. Sentence go through text_feature_columns from tf hub
    1. The embedding vectors thus enter LSTM layers 
    1. The output should then be compared with label from input_fn
## Data Format
1. Sonnets
    1. `xxxnewlinexxx` means new line
    1. Every line in the txt file is one sonnet
1. Poems
## TODO
1. Train own embedding matrix
    * `xxxnewlinexxx` is much more closer, causing low loss value
    * The network then recognize it to be more suitable
    * Same for punctuations
## Note
1. Build Graph first
## Reference
1. https://www.tensorflow.org/tutorials/text_classification_with_tf_hub