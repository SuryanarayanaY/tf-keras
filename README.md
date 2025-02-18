# Keras: Deep Learning for humans

![Keras logo](https://s3.amazonaws.com/keras.io/img/keras-logo-2018-large-1200.png)

This repository hosts the development of the TF-Keras library.
Read the documentation at [keras.io](https://keras.io/).

## About Keras

Keras is a deep learning API written in Python,
running on top of the machine learning platform [TensorFlow](https://github.com/tensorflow/tensorflow).
It was developed with a focus on enabling fast experimentation and
providing a delightful developer experience.

**The purpose of TF-Keras is to give an *unfair advantage* to any developer looking to ship ML-powered apps.**

Keras is:

-   **Simple** -- but not simplistic. TF-Keras reduces developer *cognitive load*
    to free you to focus on the parts of the problem that really matter.
    TF-Keras focuses on ease of use, debugging speed, code elegance & conciseness,
    maintainability, and deployability (via TFServing, TFLite, TF.js).
-   **Flexible** -- TF-Keras adopts the principle of *progressive disclosure of
    complexity*: simple workflows should be quick and easy, while arbitrarily
    advanced workflows should be *possible* via a clear path that builds upon
    what you've already learned.
-   **Powerful** -- TF-Keras provides industry-strength performance and
    scalability: it is used by organizations and companies including NASA,
    YouTube, and Waymo. That's right -- your YouTube recommendations are
    powered by Keras, and so is the world's most advanced driverless vehicle.

---

## TF-Keras & TensorFlow 2

[TensorFlow 2](https://www.tensorflow.org/) is an end-to-end, open-source machine learning platform.
You can think of it as an infrastructure layer for
[differentiable programming](https://en.wikipedia.org/wiki/Differentiable_programming).
It combines four key abilities:

- Efficiently executing low-level tensor operations on CPU, GPU, or TPU.
- Computing the gradient of arbitrary differentiable expressions.
- Scaling computation to many devices, such as clusters of hundreds of GPUs.
- Exporting programs ("graphs") to external runtimes such as servers, browsers, mobile and embedded devices.

Keras is the high-level API of TensorFlow 2: an approachable, highly-productive interface
for solving machine learning problems,
with a focus on modern deep learning. It provides essential abstractions and building blocks for developing
and shipping machine learning solutions with high iteration velocity.

Keras empowers engineers and researchers to take full advantage of the scalability
and cross-platform capabilities of TensorFlow 2: you can run TF-Keras on TPU or on large clusters of GPUs,
and you can export your TF-Keras models to run in the browser or on a mobile device.

---

## First contact with Keras

The core data structures of TF-Keras are __layers__ and __models__.
The simplest type of model is the [`Sequential` model](https://keras.io/guides/sequential_model/), a linear stack of layers.
For more complex architectures, you should use the [Keras functional API](https://keras.io/guides/functional_api/),
which allows you to build arbitrary graphs of layers or [write models entirely from scratch via subclassing](https://keras.io/guides/making_new_layers_and_models_via_subclassing/).

Here is the `Sequential` model:

```python
from tensorflow.keras.models import Sequential

model = Sequential()
```

Stacking layers is as easy as `.add()`:

```python
from tensorflow.keras.layers import Dense

model.add(Dense(units=64, activation='relu'))
model.add(Dense(units=10, activation='softmax'))
```

Once your model looks good, configure its learning process with `.compile()`:

```python
model.compile(loss='categorical_crossentropy',
              optimizer='sgd',
              metrics=['accuracy'])
```

If you need to, you can further configure your optimizer. The TF-Keras philosophy is to keep simple things simple,
while allowing the user to be fully in control when they need to be (the ultimate control being the easy extensibility of the source code via subclassing).

```python
model.compile(loss=tf.keras.losses.categorical_crossentropy,
              optimizer=tf.keras.optimizers.SGD(
                  learning_rate=0.01, momentum=0.9, nesterov=True))
```

You can now iterate on your training data in batches:

```python
# x_train and y_train are Numpy arrays.
model.fit(x_train, y_train, epochs=5, batch_size=32)
```

Evaluate your test loss and metrics in one line:

```python
loss_and_metrics = model.evaluate(x_test, y_test, batch_size=128)
```

Or generate predictions on new data:

```python
classes = model.predict(x_test, batch_size=128)
```

What you just saw is the most elementary way to use TF-Keras.

However, TF-Keras is also a highly-flexible framework suitable to iterate on state-of-the-art research ideas.
Keras follows the principle of **progressive disclosure of complexity**: it makes it easy to get started,
yet it makes it possible to handle arbitrarily advanced use cases,
only requiring incremental learning at each step.

In pretty much the same way that you were able to train & evaluate a simple neural network above in a few lines,
you can use TF-Keras to quickly develop new training procedures or exotic model architectures.
Here's a low-level training loop example, combining TF-Keras functionality with the TensorFlow `GradientTape`:

```python
import tensorflow as tf

# Prepare an optimizer.
optimizer = tf.keras.optimizers.Adam()
# Prepare a loss function.
loss_fn = tf.keras.losses.kl_divergence

# Iterate over the batches of a dataset.
for inputs, targets in dataset:
    # Open a GradientTape.
    with tf.GradientTape() as tape:
        # Forward pass.
        predictions = model(inputs)
        # Compute the loss value for this batch.
        loss_value = loss_fn(targets, predictions)

    # Get gradients of loss wrt the weights.
    gradients = tape.gradient(loss_value, model.trainable_weights)
    # Update the weights of the model.
    optimizer.apply_gradients(zip(gradients, model.trainable_weights))
```

For more in-depth tutorials about Keras, you can check out:

-   [Introduction to TF-Keras for engineers](https://keras.io/getting_started/intro_to_keras_for_engineers/)
-   [Introduction to TF-Keras for researchers](https://keras.io/getting_started/intro_to_keras_for_researchers/)
-   [Developer guides](https://keras.io/guides/)
-   [Other learning resources](https://keras.io/getting_started/learning_resources/)

---

## Installation

Keras comes packaged with TensorFlow 2 as `tensorflow.keras`.
To start using Keras, simply [install TensorFlow 2](https://www.tensorflow.org/install).
You can then import TF-Keras as follows:

```python
from tensorflow import keras
```

---

## Release and compatibility

Keras has **nightly releases** (`keras-nightly` on PyPI)
and **stable releases** (`keras` on PyPI).
The nightly TF-Keras releases are usually compatible with the corresponding version
of the `tf-nightly` releases
(e.g. `keras-nightly==2.7.0.dev2021100607` should be
used with `tf-nightly==2.7.0.dev2021100607`).
We don't maintain backward compatibility for nightly releases.
For stable releases, each Keras
version maps to a specific stable version of TensorFlow.

The table below shows the compatibility version mapping
between TensorFlow versions and TF-Keras versions.

All the release branches can be found on [GitHub](https://github.com/keras-team/tf-keras/releases).

All the release binaries can be found on [Pypi](https://pypi.org/project/tf_keras/#history).

---
## Support

You can ask questions and join the development discussion:

- In the [TensorFlow forum](https://discuss.tensorflow.org/).
- On the [Keras mailing list](https://groups.google.com/forum/#!forum/keras-users).

---

## Opening an issue

You can also post **bug reports and feature requests** (only)
in [GitHub issues](https://github.com/keras-team/tf-keras/issues).


---

## Opening a PR

We welcome contributions! Before opening a PR, please read
[our contributor guide](https://github.com/keras-team/tf-keras/blob/master/CONTRIBUTING.md),
and the [API design guideline](https://github.com/keras-team/governance/blob/master/keras_api_design_guidelines.md).
