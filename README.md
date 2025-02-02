# Deep Neural Network for Malaria Infected Cell Recognition

## AIM
```
To develop a deep neural network for Malaria infected cell recognition and to analyze the performance.
```
## Problem Statement and Dataset
```
Malaria dataset of 27,558 cell images with an equal number of parasitized and uninfected cells. A level-set based algorithm was applied to detect and segment the red blood cells. The images were collected and annotated by medical professionals.Here we build a convolutional neural network model that is able to classify the cells.
```
## Neural Network Model

![image](https://github.com/22008008/malaria-cell-recognition/assets/118343520/488ed53f-91b8-4294-817d-ff396843cc29)


## DESIGN STEPS
```
STEP-1:
Import tensorflow and preprocessing libraries

STEP 2:
Download and load the dataset folder

STEP-3:
Split the training and testing folders.

STEP 4:
Perform image data generation methods.

STEP-6:
Build the convolutional neural network model

STEP-7:
Train the model with the training data

STEP-8:
Plot the performance plot

STEP-9:
Evaluate the model with the testing data using probability prediction(uninfected-> prob>0.5,parasitized-> <=0.5)

STEP-10:
Fit the model and predict the sample input.
```
## PROGRAM
```
Developed By: Sri Ranjani Priya.P
Register Number:212222220049
```
```
import os
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
from matplotlib.image import imread
from tensorflow.keras.preprocessing.image import ImageDataGenerator
from tensorflow import keras
from tensorflow.keras import layers
from tensorflow.keras import utils
from tensorflow.keras import models
from sklearn.metrics import classification_report,confusion_matrix
from google.colab import drive
import tensorflow as tf
drive.mount('/content/drive')
!tar --skip-old-files -xvf '/content/drive/MyDrive/Dataset/cell_images.tar.xz' -C '/content/drive/MyDrive/Dataset/'
my_data_dir = '/content/drive/MyDrive/Dataset/cell_images'
os.listdir(my_data_dir)
test_path = my_data_dir+'/test/'
train_path = my_data_dir+'/train/'
os.listdir(train_path)
len(os.listdir(train_path+'/uninfected/'))
len(os.listdir(train_path+'/parasitized/'))
os.listdir(train_path+'/parasitized')[122]
para_img= imread(train_path+
                 '/parasitized/'+
                 os.listdir(train_path+'/parasitized')[122])
plt.imshow(para_img)
dim1 = []
dim2 = []
for image_filename in os.listdir(test_path+'/uninfected'):
    img = imread(test_path+'/uninfected'+'/'+image_filename)
    d1,d2,colors = img.shape
    dim1.append(d1)
    dim2.append(d2)
sns.jointplot(x=dim1,y=dim2)
image_shape = (130,130,3)
help(ImageDataGenerator)
image_gen = ImageDataGenerator(rotation_range=20, # rotate the image 20 degrees
                               width_shift_range=0.10, # Shift the pic width by a max of 5%
                               height_shift_range=0.10, # Shift the pic height by a max of 5%
                               rescale=1/255, # Rescale the image by normalzing it.
                               shear_range=0.1, # Shear means cutting away part of the image (max 10%)
                               zoom_range=0.1, # Zoom in by 10% max
                               horizontal_flip=True, # Allo horizontal flipping
                               fill_mode='nearest' # Fill in missing pixels with the nearest filled value
image_gen.flow_from_directory(train_path)
image_gen.flow_from_directory(test_path)
model = models.Sequential()
model.add(layers.Conv2D(filters=32,kernel_size=(3,3),input_shape=image_shape,activation='relu'))
model.add(layers.MaxPooling2D(pool_size=(2,2)))

model.add(layers.Conv2D(filters=64,kernel_size=(3,3),input_shape=image_shape,activation='relu'))
model.add(layers.MaxPooling2D(pool_size=(2,2)))

model.add(layers.Conv2D(filters=32,kernel_size=(3,3),activation='relu'))
model.add(layers.MaxPooling2D(pool_size=(2,2)))

model.add(layers.Flatten())

model.add(layers.Dense(128))
model.add(layers.Activation('relu'))
model.add(layers.Dropout(0.5))

model.add(layers.Dense(1))
model.add(layers.Activation('sigmoid'))

model.compile(loss='binary_crossentropy',
              optimizer='adam',
              metrics=['accuracy'])
model.summary()
batch_size = 16

help(image_gen.flow_from_directory)
train_image_gen = image_gen.flow_from_directory(train_path,
                                               target_size=image_shape[:2],
                                                color_mode='rgb',
                                               batch_size=batch_size,
                                               class_mode='binary')

train_image_gen.batch_size
len(train_image_gen.classes)
train_image_gen.total_batches_seen
test_image_gen = image_gen.flow_from_directory(test_path,
                                               target_size=image_shape[:2],
                                               color_mode='rgb',
                                               batch_size=batch_size,
                                               class_mode='binary',shuffle=False)


train_image_gen.class_indices
results = model.fit(train_image_gen,epochs=20,
                              validation_data=test_image_gen
                             )
model.save('cell_model.h5')
losses = pd.DataFrame(model.history.history)
losses[['loss','val_loss']].plot()
model.metrics_names
model.evaluate(test_image_gen)
pred_probabilities = model.predict(test_image_gen)
test_image_gen.classes
predictions = pred_probabilities > 0.5
confusion_matrix(test_image_gen.classes,predictions)
plt.imshow(predictions)
print[predictions]
```    

## OUTPUT

### Training Loss, Validation Loss Vs Iteration Plot

![image](https://github.com/22008008/malaria-cell-recognition/assets/118343520/4b92644a-4dd6-4ace-8fb5-1b7f9e798255)


### Classification Report

![image](https://github.com/22008008/malaria-cell-recognition/assets/118343520/3986de03-02f1-4852-af57-9e95f00ba486)

### Confusion Matrix

array([[1219,   81],
       [  63, 1237]])

### New Sample Data Prediction

![image](https://github.com/22008008/malaria-cell-recognition/assets/118343520/7effe845-4e43-4ed2-b8fc-13f279e87606)


## RESULT
```
Successfully developed a convolutional deep neural network for Malaria Infected Cell Recognition.
```
