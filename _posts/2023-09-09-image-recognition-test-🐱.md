---
permalink: /posts/2023/09/image_recognition_test
tags:
    - notebook
    - tensorflow
    - sample
---

```python
import tensorflow as tf
from tensorflow.keras import datasets, layers, models
import matplotlib.pyplot as plt
```

```python
#  LOAD AND SPLIT DATASET
(train_images, train_labels), (test_images, test_labels) = \
  datasets.cifar10.load_data()
# Normalize pixel values to be between 0 and 1
train_images, test_images = train_images / 255.0, test_images / 255.0
class_names = ['airplane', 'automobile', 'bird', 'cat', 'deer',
               'dog', 'frog', 'horse', 'ship', 'truck']
```

```python
# Let's look at a one image
IMG_INDEX = 7  # change this to look at other images
plt.imshow(train_images[IMG_INDEX] ,cmap=plt.cm.binary)
plt.xlabel(class_names[train_labels[IMG_INDEX][0]])
plt.show()
```


    
![png](/images/20230911-sample.png)
    



```python
model = models.Sequential()

## CONVOLUTIONAL BASE
# first layer receives 32,32,3 images. 32 filters of size 3x3 
model.add(layers.Conv2D(32, (3, 3), activation='relu',
                        input_shape=(32, 32, 3)))
# second layer pools 2x2 with a stide of 2x2 (not specified defaults to pool)
model.add(layers.MaxPooling2D((2, 2)))
# further layers use more filters due to the pooling shrinking the images 
# allowing more depth
model.add(layers.Conv2D(64, (3, 3), activation='relu'))
model.add(layers.MaxPooling2D((2, 2)))
model.add(layers.Conv2D(64, (3, 3), activation='relu'))

```


```python
# print("Num GPUs Available: ", len(tf.config.list_physical_devices('GPU')))
```


```python
model.summary()
```

    Model: "sequential"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     conv2d (Conv2D)             (None, 30, 30, 32)        896       
                                                                     
     max_pooling2d (MaxPooling2D  (None, 15, 15, 32)       0         
     )                                                               
                                                                     
     conv2d_1 (Conv2D)           (None, 13, 13, 64)        18496     
                                                                     
     max_pooling2d_1 (MaxPooling  (None, 6, 6, 64)         0         
     2D)                                                             
                                                                     
     conv2d_2 (Conv2D)           (None, 4, 4, 64)          36928     
                                                                     
    =================================================================
    Total params: 56,320
    Trainable params: 56,320
    Non-trainable params: 0
    _________________________________________________________________
    


```python
# flatten 4,4,64 to a single vector
model.add(layers.Flatten())
# feed it into a dense layer
model.add(layers.Dense(64, activation='relu'))
# do the 10 object classification
model.add(layers.Dense(10))
```


```python
model.summary()
```

    Model: "sequential"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     conv2d (Conv2D)             (None, 30, 30, 32)        896       
                                                                     
     max_pooling2d (MaxPooling2D  (None, 15, 15, 32)       0         
     )                                                               
                                                                     
     conv2d_1 (Conv2D)           (None, 13, 13, 64)        18496     
                                                                     
     max_pooling2d_1 (MaxPooling  (None, 6, 6, 64)         0         
     2D)                                                             
                                                                     
     conv2d_2 (Conv2D)           (None, 4, 4, 64)          36928     
                                                                     
     flatten (Flatten)           (None, 1024)              0         
                                                                     
     dense (Dense)               (None, 64)                65600     
                                                                     
     dense_1 (Dense)             (None, 10)                650       
                                                                     
    =================================================================
    Total params: 122,570
    Trainable params: 122,570
    Non-trainable params: 0
    _________________________________________________________________
    


```python
# Train!
model.compile(optimizer='adam',
              loss=tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True),
              metrics=['accuracy'])

history = model.fit(train_images, train_labels, epochs=10, 
                    validation_data=(test_images, test_labels))
```

    Epoch 1/10
    1563/1563 [==============================] - 18s 4ms/step - loss: 1.4885 - accuracy: 0.4566 - val_loss: 1.1859 - val_accuracy: 0.5804
    Epoch 2/10
    1563/1563 [==============================] - 5s 3ms/step - loss: 1.1033 - accuracy: 0.6087 - val_loss: 0.9931 - val_accuracy: 0.6475
    Epoch 3/10
    1563/1563 [==============================] - 5s 3ms/step - loss: 0.9554 - accuracy: 0.6653 - val_loss: 0.9515 - val_accuracy: 0.6644
    Epoch 4/10
    1563/1563 [==============================] - 5s 3ms/step - loss: 0.8526 - accuracy: 0.7022 - val_loss: 0.9089 - val_accuracy: 0.6777
    Epoch 5/10
    1563/1563 [==============================] - 5s 4ms/step - loss: 0.7833 - accuracy: 0.7264 - val_loss: 0.8903 - val_accuracy: 0.6917
    Epoch 6/10
    1563/1563 [==============================] - 9s 6ms/step - loss: 0.7173 - accuracy: 0.7474 - val_loss: 0.8368 - val_accuracy: 0.7094
    Epoch 7/10
    1563/1563 [==============================] - 9s 6ms/step - loss: 0.6726 - accuracy: 0.7612 - val_loss: 0.8823 - val_accuracy: 0.6956
    Epoch 8/10
    1563/1563 [==============================] - 11s 7ms/step - loss: 0.6274 - accuracy: 0.7796 - val_loss: 0.8806 - val_accuracy: 0.6979
    Epoch 9/10
    1563/1563 [==============================] - 11s 7ms/step - loss: 0.5806 - accuracy: 0.7960 - val_loss: 0.9070 - val_accuracy: 0.6970
    Epoch 10/10
    1563/1563 [==============================] - 10s 6ms/step - loss: 0.5424 - accuracy: 0.8090 - val_loss: 0.8613 - val_accuracy: 0.7181
    


```python
# Test against pre-labeled data
test_loss, test_acc = model.evaluate(test_images,  test_labels, verbose=2)
print(test_acc)
# around 71% accuracy
```

    313/313 - 1s - loss: 0.8613 - accuracy: 0.7181 - 517ms/epoch - 2ms/step
    0.7181000113487244
    


```python

```