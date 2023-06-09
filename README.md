Download Link: https://assignmentchef.com/product/solved-comp9444-project-1-basic-tensorflow-and-digit-recognition
<br>
<h3>Introduction</h3>

This and all following assignments will use the Python API for the TensorFlow library. TensorFlow (TF) is an opensource library primarily used to construct, train and evaluate machine learning models. TF allows rapid development and supports automatic differentiation – meaning backprop is able to be done automatically for any model adequately defined. TF also abstracts away much of the low-level code required to set up training on GPU’s; in many cases TF will automatically detect and utilize your computer’s GPU if it has one. Central to the design of TF is the concept of a ‘graph’ – a low level representation of a model consisting of nodes and tensors. Broadly, implementing a TF model can be broken down into two sections; creating the graph, and training/testing it. This assignment is mainly concerned with graph creation. You can read more about the general structure of TensorFlow <a href="https://www.tensorflow.org/guide/">here</a>.




This assignment is split into two parts. Part 1 contains basic introductory exercises using TensorFlow’s core API. In Part 2, you will be implementing a single layer network, a two layer network and a convolutional network to classify handwritten digits. We will work with the <a href="https://en.wikipedia.org/wiki/MNIST_database">MNIST dataset</a>, a common dataset used to evaluate Machine Learning models.




Be sure to read this document in it’s entirety before starting the assignment.

<h4>Notes/Common Problems</h4>

<ol>

 <li>Tensorflow has several levels of abstraction in it’s API, with some sections designed for rapid prototyping (Keras, Estimators), others for more mid-level functionality (<code>tf.layers,tf.nn</code>), and some exposing base functionality. For this course we will mostly be focusing on the core and mid-level APIs. For this Assignment you may <b>NOT</b> make calls to the following parts of the API;

  <ul>

   <li><code>tf.contrib</code> (including eager). By extension: do not follow any example that uses the following call at any point in the code. <code>tf.enable_eager_execution()</code>.</li>

   <li><code>tf.keras</code></li>

   <li>Anything to do with estimators (<code>tf.estimator</code>)</li>

  </ul>Calls from these sections of the API will cause your code to fail the autotests and for you to receive a mark of 0 for that section of the assignment. It is suggested to call functions from:

  <ul>

   <li><code>tf.layers</code> (this is mostly wrappers for <code>tf.nn</code> but with nicer parameter structuring)</li>

   <li><code>tf.nn</code></li>

  </ul>Calls from other parts of the API such as <code>tf.math</code> are allowed but discouraged – there is probably an easier way to do what you are trying to do.</li>

 <li>Tensorflow is under extremely active development, and many Google searches will point to answers that use old versions of the API, or older version of the docs. Ensure you are always viewing information relevant to 1.9</li>

 <li>There a great many tutorials dealing with MNIST and TensorFlow. Because of the above point their utility is limited for this assignment. It is suggested to read and understand the docs themselves as opposed to blindly following tutorials.</li>

 <li>In general, you should never call functions that have depreciation warnings in the documentation (e.g. <code>tf.nn.softmax_cross_entropy_with_logits()</code>)</li>

</ol>




<h4>Preliminaries</h4>

Before commencing this assignment, you should download and install TensorFlow (instructions <a href="https://www.tensorflow.org/install/">here</a>) and the appropriate python version (3.5 – 3.7 are all ok, do not use 2.7x). It is also helpful to complete the ‘Low level Introduction’ tutorial located on the TensorFlow website <a href="https://www.tensorflow.org/guide/low_level_intro">here</a>.

Copy the archive <a href="https://www.cse.unsw.edu.au/~cs9444/18s2/hw1/src.zip"><code>src.zip</code></a> into your own filespace and unzip it. Then type

<pre>cd src</pre>

You will see two files: <code>train.py</code>  and  <code>hw1.py</code>

<h3>Part I [3 marks]</h3>

Implement the following functions within <code>hw1.py</code>. You do not need to use <code>train.py</code> in Part I. Note that the example function <code>add_consts()</code> has been completed for you.

<ol>

 <li><code>add_consts()</code> (*already implemented): Construct a TensorFlow graph that declares 3 constants, 5.1, 1.0 and 5.9 and adds these together, and returns the resulting tensor.</li>

 <li><code>add_consts_with_placeholder() [1 mark]</code>: Construct a TensorFlow graph that constructs 2 constants, 5.1, 1.0 and one TensorFlow placeholder of type <code>tf.float32</code> that accepts a scalar input, and adds these three values together, returning as a tuple (and in the following order), the final sum and created placeholder.</li>

 <li><code>my_relu() [1 mark]</code>: Implement a ReLU activation function that takes a scalar <code>tf.placeholder</code> as input and returns the appropriate output. A ReLU is one of the simplest activation functions, simply setting all negative values passed into it to 0, and passing positive values through unchanged. For more information, see <a href="https://en.wikipedia.org/wiki/Rectifier_(neural_networks)">here</a>.</li>

 <li><code>my_perceptron() [1 mark]</code> Implement a single perceptron that takes <code>x</code> inputs and produces one output, using the RelU activation function you defined previously.: Specifically, implement a function that takes an argument <code>x</code>, and creates a <code>tf.placeholder</code> of length <code>x</code>. Then create a trainable TF variable for the weights <code>W</code> (<i>hint: look at tf.get_variable() and the initalizer argument.</i>). Ensure this variable is set to be initialized as all ones. Multiply and sum the weights and inputs following the peceptron outlined in the lecture slides. Finally, call your ReLU activation function. Return the placeholder and output in that order as a tuple. The code will be tested using the following init scheme<pre><code>        # graph def (your code called)        init = tf.global_variables_initializer()        self.sess.run(init)        # tests here            </code></pre></li>

</ol>

<h3>Part II [7 marks]</h3>

<h4>Stage 0: Provided Code and Getting Started</h4>

Now run  <code>train.py</code>  by typing

<pre>python train.py your_student_id onelayer</pre>

The student number argument is not actually important for local testing – it’s use is for automarking. When run for the first time,  <code>train.py</code>  should create a new folder called  <code>data</code>  and download a copy of the MNIST dataset into this folder. All subsequent runs of  <code>train.py</code>  will use this local data. (Don’t worry about the  <code>ValueError</code>  at this stage. Deprecation warnings can also be ignored).

The file  <code>train.py</code>  contains the TensorFlow code required to create a session, build the graph, and run training and test iterations. It has been provided to assist you with the testing and evaluation of your model. While it is not required for this assignment to have a detailed understanding of this code, it will be useful when implementing your own models, and for later assignments.

<b>IMPORTANT:</b>  <code>train.py</code>  should not be modified at any point. Only <code>hw1.py</code> requires modification. A submission that does not run correctly with an unaltered  <code>train.py</code>  will lose marks.




The file  <code>hw1.py</code>  contains function definitions for the three networks to be created. You may also define helper functions in this file if necessary, as long as the original function names and arguments are not modified. Changing the function name, argument list, or return value will cause all tests to fail for that function. Your marks will be automatically generated by a test script, which will evaluate the correctness of the implemented networks. For this reason, it is important that you stick to the specification exactly. Networks that do not meet the specifications but otherwise function accurately, will be marked as incorrect.




The functions  <code>input_placeholder()</code>  and <code>target_placeholder()</code>  specify the inputs and outputs of your networks in the TensorFlow graph. They have been implemented for you.

In addition, there is a function  <code>train_step()</code>  that passes batches of images to the constructed TensorFlow Graph during training. It’s implementation should help you understand the shape and structure of the actual data that is being provided to the model.

Unless otherwise specified, the underlying type (<code>dtype</code>) for each TF object should be  <code>tf.float32</code>.  <code>INPUT_SIZE</code>, where it appears in comments, refers to the length of a flattened single image; in this case 784.  <code>OUTPUT_SIZE</code>, where it appears in comments, refers to the length of a one-hot output vector; in this case 10.

In the provided file  <code>hw1.py</code>, detailed specifications are provided in the comments for each function.




<h4>Stage 1: Single-Layer Network [2 marks]</h4>

Write a function  <code>onelayer(X, Y, layersize=10)</code>  which creates a TensorFlow model for a one layer neural network (sometimes also called logistic regression). Your model should consist of one fully connected layer with weights <code>w</code>  and biases  <code>b</code>, using <a href="https://en.wikipedia.org/wiki/Softmax_function">softmax</a> activation.

Your function should take two parameters  <code>X</code>  and  <code>Y</code> that are TensorFlow placeholders as defined in  <code>input_placeholder()</code>  and <code>target_placeholder()</code>. It should return varibles  <code>w, b, logits, preds</code>, <code>batch_xentropy</code>  and  <code>batch_loss</code>, where:

<ul>

 <li> <code>w</code>  and  <code>b</code>  are TensorFlow variables representing the weights and biases, respectively</li>

 <li> <code>logits</code>  and  <code>preds</code>  are the input to the activation function and its output</li>

 <li> <code>xentropy_loss</code>  is the cross-entropy loss for each image in the batch</li>

 <li> <code>batch_loss</code>  is the average of the cross-entropy loss for all images in the batch</li>

</ul>

Note that to return these variables you cannot simply call <code>tf.layers.dense</code>. A good approach might be to define your own dense layer using TF primitives.




Test your network on the MNIST dataset by typing

<pre>python train.py student_id onelayer</pre>

It should achieve about 92% accuracy after 5 epochs of training.

It is a good idea to submit your code after completing Stage 1, because the submit script will run some simple tests and give you some feedback on whether your model is correctly structured.

<h4>Stage 2: Two-Layer Network [2 marks]</h4>

Create a TensorFlow model for a Neural Network with two fully connected layers of weights  <code>w1, w2</code>  and biases  <code>b1, b2</code>, with &lt;a “href=”https://en.wikipedia.org/wiki/Rectifier_(neural_networks)””&gt;ReLU activation functions on the first layer, and softmax on the second. Your function should take two parameters  <code>X</code>  and  <code>Y</code> that are TensorFlow placeholders as defined in  <code>input_placeholder()</code>  and <code>target_placeholder()</code>. It should return varibles  <code>w1, b1, w2, b2, logits, preds</code>, <code>batch_xentropy</code>  and  <code>batch_loss</code>, where:

<ul>

 <li> <code>w1</code>  and  <code>b1</code>  are TensorFlow variables representing the weights and biases of the first layer</li>

 <li> <code>w2</code>  and  <code>b2</code>  are TensorFlow variables representing the weights and biases of the second layer</li>

 <li> <code>logits</code>  and  <code>preds</code>  are the inputs to the final activation functions and their output</li>

 <li> <code>xentropy_loss</code>  is the cross-entropy loss for each image in the batch</li>

 <li> <code>batch_loss</code>  is the average of the cross-entropy loss for all images in the batch.</li>

</ul>

To test, run;

<pre>python train.py student_id twolayer</pre>

<h4>Stage 3: Convolutional Network [3 marks]</h4>

Create a TensorFlow model for a Convolutional Neural Network. This network should consist of two convolutional layers followed by a fully connected layer of the form:

<blockquote>

 conv_layer1 → conv_layer2 → fully-connected → output

</blockquote>

Note that there are <b>no pooling</b> layers present in this model. Your function should take two parameters  <code>X</code>  and  <code>Y</code> that are TensorFlow placeholders as defined in  <code>input_placeholder()</code>  and <code>target_placeholder()</code>. It should return varibles  <code>conv1, conv2, w, b, logits, preds</code>, <code>batch_xentropy</code>  and  <code>batch_loss</code>, where:

<ul>

 <li> <code>conv1</code>  is a convolutional layer of  <code>convlayer_sizes[0]</code>  filters of shape  <code>filter_shape</code></li>

 <li> <code>conv2</code>  is a convolutional layer of  <code>convlayer_sizes[1]</code>  filters of shape  <code>filter_shape</code></li>

 <li> <code>w</code>  and  <code>b</code>  are TensorFlow variables representing the weights and biases of the final fully connected layer</li>

 <li> <code>logits</code>  and  <code>preds</code>  are the inputs to the final activation functions and their output</li>

 <li> <code>xentropy_loss</code>  is the cross-entropy loss for each image in the batch</li>

 <li> <code>batch_loss</code>  is the average of the cross-entropy loss for all images in the batch</li>

</ul>

Hints:

<ol>

 <li>use <code>tf.layer.conv2d</code></li>

 <li>the final layer is very similar to the  <code>onelayer</code>  network, except that the input will be from the  <code>conv2</code>  layer. If you reshape the  <code>conv2</code>  output using  <code>tf.reshape</code>, you should be able to call  <code>onelayer()</code>  to get the final layer of your network.</li>

</ol>

To test your implementation run;

<pre>python train.py student_id conv</pre>

It may take several minutes to train, depending on your processor.

<h4>Notes</h4>

All TensorFlow objects, if not otherwise specified, should be explicity created with  <code>tf.float32</code>  datatypes. Not specifying this datatype for variables and placeholders will cause your code to fail some tests.

The majority of this assignment can be implemented extremely compactly. If you find yourself writing 50+ line methods, it may be a good idea to look for a simpler solution. You do not need to make any additional imports, and code that makes use of external libraries such as  <code>numpy</code>, will fail tests.




<h4>Visualizing Your Models</h4>

In addition to the output of  <code>train.py</code>, you can view the progress of your models and the created TensorFlow graph using the TensorFlow visualization platform, TensorBoard. After beginning training, run the following command from the src directory:

<pre>tensorboard --logdir=./summaries</pre>

<ol>

 <li>open a Web browser and navigate to  <code>http://localhost:6006</code></li>

 <li>you should be able to see a plot of the train and test accuracies in TensorBoard</li>

 <li>if you click on the histogram tab you’ll also see some histograms of your weights, biases and the pre-activation inputs to the softmax in the final layer</li>

</ol>

Make sure you are in the same directory from which train.py is running. Don’t worry if you are unable to get TensorBoard working; it is not required to complete the assignment, but it can be a useful tool to monitor training, so it is probably worth your while becoming familiar with it. Click <a href="https://www.tensorflow.org/get_started/summaries_and_tensorboard">here</a> for more information:

<h4>Submission</h4>

You can test your code by typing

<pre>python train.py</pre>

Once submissions are open, you should submit by typing

<code>give cs9444 hw1 hw1.py</code>

When you submit, you will see some feedback for Stages 1 and 2. This is a series of Unit-tests that verify we can train your implementation. Passing all Part 1 tests on submission means you will receive all 3 marks, we will not use any additional tests for this part. For Part 2, clearing the unit-tests simply means you model is able to be evaluated, not that it has been. These tests are intended to be checks only, however you may find the error messages somewhat informative. You can make use of these unittests to check that you are structuring your code correctly.

You can submit as many times as you like – later submissions will overwrite earlier ones. You can check that your submission has been received by using the following command:

<code>9444 classrun -check</code>

The submission deadline is Sunday 26 August, 23:59.15% penalty will be applied to the (maximum) mark for every 24 hours late after the deadline.

Additional information may be found in the <a href="https://www.cse.unsw.edu.au/~cs9444/18s2/hw1/faq.shtml">FAQ</a> and will be considered as part of the specification for the project. You should check this page regularly.

Note that the tests run at submission time will be using TensorFlow 1.3. Since we are using only the core API, this should not cause any problems. However, if you believe you are getting an error due to version incompatibility, or if you feel there is genuine ambiguity or an error in the specification or provided code, you can post to the Forums on the course Web page. Due to the higher than expected enrollments for this course, please only post queries after you have made a reasonable effort to resolve the problem yourself. If you have a generic request for help you should attend one of the lab consultation sessions during the week.




This assignment will be marked on functionality in the first instance. You should always adhere to good coding practices and style. In general, a program that attempts a substantial part of the job but does that part correctly will receive more marks than one attempting to do the entire job but with many errors.




<hr>