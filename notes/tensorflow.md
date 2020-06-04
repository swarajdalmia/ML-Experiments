# Tensorflow Concepts 

TensorFlow is an open source machine learning framework that Google created and it is used to design, build, and train deep learning models. TensorFlow is a python library that used to implement deep networks and performs numerical computations with the help of tensors and computational graphs/data flow graphs.  In TF2.0, the modern tf.keras API bRINGS the simplicity and ease of use of Keras to the TensorFlow project. 

## Tensors 

What is a tensor ? and how does it differ from matrices or numpy arrays ?

Tensors in tensorflow/deep learning:
- tensors are a popular way of representing data in deep learning frameworks
- Tensors in tensorflow are implemented as multidimensional data arrays.

Rank of a tensor is the dimensionality of the multi-dim array. 
Shape of a tensor depicts the exact size of the milti dim array.


## Data types

There are 3 data types in tf each of which are tensors.
Now, these are not to be confused with data types of int/float/bool etc. Each of the 3 below, can have any of the int/float/bool etc values that tf supports which encompasses almost everything.

- constants 
```
hello_constant = tf.constant("Hello World!")  # dtype = string
# on printing hello_constant it doesn't display hello world unless it is called in a sess.run(). 
node1 = tf.constant(3.0) # by default, dtype = float32
node2 = tf.constant(3) # by default, dtype = int32
``` 

- place holders : a place holder is a promise to provide a value later
```
# the size of the placeholder is not specified just that it will contain a float tensor. The tensor can be of any rank
a = tf.placeholder(tf.float32)
b = 5*a
with tf.Session() as sess:
    print(sess.run(b,feed_dict={a:[1,7,6,]}))
# feed_dict must be used whenever one wants to assign values to placeholders in python
```

- variables : allows us to add trainable parameters to a graph. Typically used for weights and biases.

```
var = tf.Variable(tf.truncated_normal([3]))
with tf.Session() as sess:
    sess.run(tf.global_variables_initializer()) #One has to call, glocal_variables_initializer when one uses tf.Varaible
    print(sess.run(var)) # [ 0.50536436 0.56707895 0.98391068 ]
```

## Operators 

- Tensorflow has basic math operations in the form of tf.add(), tf.multiply()
- tf.cast() : to convert one datatype into another 

## Computational Graph 

In these graphs, nodes represent mathematical operations, while the edges represent the data, which usually are multidimensional data arrays or tensors. Tensor's flow through the computational graph. 

Tensorflow programs consist of two seperate sections: 
- building a computational graph 
- running the computational graph

### Executing the computational graph

After making a computational graph one needs to run it. One doesn't get an output for all the variables in the computational graph, unless one specifies them explitly in the running session. To run the entire graph, one calls the last node and tensorflow automatically executes the entire graph including all the assignments and other transformations to the tensors.

### Session 
A session is basically the backbone of a tensorflow program. A session must be called and run to execute/run a computational graph.
```
with tf.Session as sess:
    output,_ = sess.run([variable_name1,var2])
    print(var)

# when one uses with, one doesnt need to explicitly called sess.close() otherwise it is important
```

## Building and using a model 

The steps in using a model using tf.keras can be broken down into 5: [from here](https://machinelearningmastery.com/tensorflow-tutorial-deep-learning-with-tf-keras/). 

- Define the model. Models can be defined either with the Sequential API or the Functional API,
- Compile the model. Reuires selecting the loss function and the optimiser.
```
opt = SGD(learning_rate=0.01, momentum=0.9)
model.compile(optimizer=opt, loss='binary_crossentropy', metrics=['accuracy'])
```
- Fit the model. The most time taking part. Batch size and num. epochs are specified.
```
# verbose = 2(reports model performance after each epoch), verbose = 0(means no output).
model.fit(X, y, epochs=100, batch_size=32, verbose=0)
```
- Evaluate the model.
```
# other metrics along with loss can be evaluated as well
loss = model.evaluate(X, y, verbose=0)
```
- Make predictions.
```
y = model.predict(X)
```

## Types of Models 

### Sequential Models

The simpler API, which consists in first defining a sequential model and then linearly adding layers to it. 
The visible layer of the network is defined by the “input\_shape” argument on the first hidden layer.

```
from tensorflow.keras import Sequential
from tensorflow.keras.layers import Dense

model = Sequential()
model.add(Dense(100, input_shape=(8,)))
model.add(Dense(50))
model.add(Dense(20))
model.add(Dense(1))
```

### Functional Models

This one is more flexible and involves connected the output of one layer into the input of another. 

```
from tensorflow.keras import Model
from tensorflow.keras import Input
from tensorflow.keras.layers import Dense

x_in = Input(shape=(8,))
x = Dense(10)(x_in)
x_out = Dense(1)(x)

model = Model(inputs=x_in, outputs=x_out)
```

## Visualization 

### Model Visualization

- model.summary() : Outputs a text description of the model.
- One can create a plot of the model by calling the plot\_model() function.
```
plot_model(model, 'model.png', show_shapes=True)
```

### Plot Learning Curves

The fit function returns a history object that contains a trace of performance metrics recorded at the end of each training epoch. This includes the chosen loss function and each configured metric, such as accuracy, and each loss and metric is calculated for the training and validation datasets.

A learning curve is a plot of the loss on the training dataset and the validation dataset. We can create this plot from the history object using the Matplotlib library. [Example](https://machinelearningmastery.com/tensorflow-tutorial-deep-learning-with-tf-keras/)


## Saving and Loading the Model

A model can be saved using the model.save() function. It can be loaded later using the load\_model() function.

The model is saved in H5 format, an efficient array storage format. As such, you must ensure that the h5py library is installed on your workstation.

- The weights can be saved and loaded as well. For details look [here](https://cv-tricks.com/tensorflow-tutorial/save-restore-tensorflow-models-quick-complete-tutorial/)
    - the checkpoint binary file contains all the values of the weights, biases, gradients and all the other variables saved.
    - in this case the meta file can be imported to import the meta computational graph. 
    - along with these, tf also has a file named checkpoint which simply keeps a record of latest checkpoint files saved.
- saver.save and saver.restore can be used as well. 









