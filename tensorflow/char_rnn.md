# Char RNN

In this tutorial we will implement a simple Character Recurrent Neural network that Predicts the next character
given a set of characters.

We will use tensorflow to implement this Recurrent neural network.

The full code is available in github [Simple word and char rnn](https://github.com/Shivakishore14/simple-word-and-char-rnn-tensorflow)

## imports
we need tensorflow and numpy to be installed for this tutorial.

```
import tensorflow as tf
import numpy as np
import random
from tensorflow.contrib import rnn
```

## Data for training
We will need a text file for training. in this tutorial we will read the current python script and use it for training.
```
filename = __file__

def read_data(fname):
    with open(fname) as f:
        content = f.readlines()
    content = [x.strip() for x in content]  # remove start and end spaces in each line
    content = [list(i) for i in content]    # spliting into chrs
    content = np.hstack(content)            # flatteing the array to 1d
    return content

training_data = read_data(filename)
```

The `read_data()` function takes in a filename and returns a 1 dimensional list of characters 
```
>>> print training_data
['i' 'm' 'p' ..., 'd' '"' ')']
```
## Data Processing
RNN cannot take in characters or words as input they can only take in float or integer values.
so we should create a map to each character and an integer (like a lookup table)

first lets create a variable vocab witch we will set to unique elements of the training data.
```
vocab = set(training_data)
vocab_size = len(vocab)
```
Now lets create a mapping for each unique character to an integer
```
dictionary = {data: i for i, data in enumerate(vocab)}
reverse_dictionary = {i: data for i, data in enumerate(vocab)}
```
here we have 2 mappings 

1. dictionary => which maps char to int eg 'm'=>2
2. reverse_dictionay => which maps int to char eg 2='m'

## Hyper parameters
Now lets define the hyperparameters for our model

```
# learing rate for the model
learning_rate = 0.001

# total train steps
training_iters = 50000

# display loss, accuracy etc on each display step
display_step = 500

# number of previous input for which an output depends on
n_input = 4

# number of units in RNN cell
n_hidden = 512
``` 
Please feel free to tweak the parameters.

## Model Constuction
The model constuction will have the following steps

1. Define input and output placeholders
2. Define Weight and Bias for output layer
3. Reshape the input to match the specification of the rnn cell.
4. Create the rnn cell
5. use static rnn to generate output, states
6. use the last layer of output for prediction
7. Define loss and optimiser
8. Have a model evaluation operation

```
# tf Graph input 
# step 1
x = tf.placeholder("float", [None, n_input, 1])
y = tf.placeholder("float", [None, vocab_size])


def RNN(x, n_hidden, vocab_size):
    # RNN output node weights and biases
    # step 2
    weight = tf.Variable(tf.random_normal([n_hidden, vocab_size]))
    bias = tf.Variable(tf.random_normal([vocab_size]))

    # reshape to [1, n_input]
    # step 3
    x = tf.reshape(x, [-1, n_input])
    x = tf.split(x, n_input, 1)

    # step 4 
    rnn_cell = rnn.BasicLSTMCell(n_hidden)

    # generate prediction
    # step 5
    outputs, states = rnn.static_rnn(rnn_cell, x, dtype=tf.float32)

    # step 6
    return tf.matmul(outputs[-1], weight) + bias


pred = RNN(x, n_hidden, vocab_size)

# Loss and optimizer
# step 7
cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(logits=pred, labels=y))
optimizer = tf.train.RMSPropOptimizer(learning_rate=learning_rate).minimize(cost)

# Model evaluation
# step 8
correct_pred = tf.equal(tf.argmax(pred,1), tf.argmax(y,1))
accuracy = tf.reduce_mean(tf.cast(correct_pred, tf.float32))
```

## Run the model
First we should initialize all variables.<br>
```
init = tf.global_variables_initializer()
```
Now lets create a session and start the training
```
with tf.Session() as session:
    session.run(init)
    # define all the necessary variables 
    step = 0
    acc_total = 0
    loss_total = 0
    # start position
    offset = random.randint(0, n_input+1)
```
Next we will define the training loop,<br>
we need to prepare the data in a format that will be accepted by `x` and `y`

`x` will take inputs like <br>
`[ [[1], [12], [3], [9]], ...]`<br>
`y` is a one hot encoded output <br>
`[[0 ,0, 0, 0, 1, 0, 0, ...], ...]`
```
# for x
symbols_in_keys = [ [dictionary[ str(training_data[i])]] for i in range(offset, offset+n_input) ]
symbols_in_keys = np.reshape(np.array(symbols_in_keys), [-1, n_input, 1])

# for y
symbols_out_onehot = np.zeros([vocab_size], dtype=float)
symbols_out_onehot[dictionary[str(training_data[offset+n_input])]] = 1.0
symbols_out_onehot = np.reshape(symbols_out_onehot, [1, -1])
```
Now lets feed the processed data to the session for running the optimizer to learn and predict the next char
```
_, acc, loss, onehot_pred = session.run([optimizer, accuracy, cost, pred], \
    feed_dict={x: symbols_in_keys, y: symbols_out_onehot})
```
Now that we got our prediction lets print the stats every display steps in our training loop
```
loss_total += loss
acc_total += acc
if (step+1) % display_step == 0:
    print("Iter= " + str(step+1) + ", Average Loss= " + \
            "{:.6f}".format(loss_total/display_step) + ", Average Accuracy= " + \
            "{:.2f}%".format(100*acc_total/display_step))
    acc_total = 0
    loss_total = 0
    symbols_in = [training_data[i] for i in range(offset, offset + n_input)]
    symbols_out = training_data[offset + n_input]
    symbols_out_pred = reverse_dictionary[int(tf.argmax(onehot_pred, 1).eval())]
    print("%s - [%s] vs [%s]" % (symbols_in, symbols_out, symbols_out_pred))
```

Checkout the full code for [char-rnn.py](https://github.com/Shivakishore14/simple-word-and-char-rnn-tensorflow/blob/master/char-rnn.py) 