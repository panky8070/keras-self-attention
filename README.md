# Keras Global Self-Attention

[![Travis](https://travis-ci.org/CyberZHG/keras-self-attention.svg)](https://travis-ci.org/CyberZHG/keras-self-attention)
[![Coverage](https://coveralls.io/repos/github/CyberZHG/keras-self-attention/badge.svg?branch=master)](https://coveralls.io/github/CyberZHG/keras-self-attention)


Attention mechanism for processing sequence data that considers the context for each timestamp.

* ![](https://user-images.githubusercontent.com/853842/44248592-1fbd0500-a21e-11e8-9fe0-52a1e4a48329.gif)
* ![](https://user-images.githubusercontent.com/853842/44248591-1e8bd800-a21e-11e8-9ca8-9198c2725108.gif)
* ![](https://user-images.githubusercontent.com/853842/44248590-1df34180-a21e-11e8-8ff1-268217f466ba.gif)
* ![](https://user-images.githubusercontent.com/853842/44248589-1d5aab00-a21e-11e8-9daa-f14578fb95d7.gif)

## Install

```bash
pip install keras-self-attention
```

## Usage

### Basic

By default, the attention layer uses additive attention and considers the whole context while calculating the relevance. The following code creates an attention layer that follows the equations in the first section (`attention_activation` is the activation function of `e_{t, t'}`):

```python
import keras
from keras_self_attention import Attention


model = keras.models.Sequential()
model.add(keras.layers.Embedding(input_dim=10000,
                                 output_dim=300,
                                 mask_zero=True))
model.add(keras.layers.Bidirectional(keras.layers.LSTM(units=128,
                                                       return_sequences=True)))
model.add(Attention(attention_activation='sigmoid'))
model.add(keras.layers.Dense(units=5))
model.compile(
    optimizer='adam',
    loss='categorical_crossentropy',
    metrics=['categorical_accuracy'],
)
model.summary()
```

### Local Attention

The global context may be too broad for one piece of data. The parameter `attention_width` controls the width of the local context:

```python
from keras_self_attention import Attention

Attention(
    attention_width=15,
    attention_activation='sigmoid',
    name='Attention',
)
```

### Multiplicative Attention

You can use multiplicative attention by setting `attention_type`:

![](https://user-images.githubusercontent.com/853842/44248897-c229b800-a21f-11e8-9b05-62ca384ed220.gif)

```python
from keras_self_attention import Attention

Attention(
    attention_width=15,
    attention_type=Attention.ATTENTION_TYPE_MUL,
    kernel_regularizer=keras.regularizers.l2(1e-6),
    use_attention_bias=False,
    name='Attention',
)
```
