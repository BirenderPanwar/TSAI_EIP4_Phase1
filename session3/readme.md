
Assignment-3: CIFAR10
---------------------

Base Model Test Score: 0.8217, Total params: 1,172,410
------------------------------------------------------

Best Model Test Score: 0.8314, Total params: 59,643
---------------------------------------------------

Following Strategy is used to optimize the model performance:
-------------------------------------------------------------

1. Network is designed using Seperable Convolution layer which perform a depthwise spatial convolution(i.e on each input channel seperately) 
   followed by a pointwise convolution. This helped to reduced the number of network parameters and still acheiving validation accuracy above 83%

2. Batch Normalization is used for each Separable Convolution layer layer to avoid overfitting of the model. 
         
3. Droupout is used at the end of each Convolution block to avoid overfitting and better generalization

4. 1X1 Conv2D is used to combine the features at the eand of Conv block-1 and Conv Block-2. 
   This also helped to reduce the number of channels so that number of learnable parameter for subsequent layers is less.
   
4. Learning Rate scheduler is applied such that learning rate(LR) reduces after each epochs. 
   For initial epochs learning rate will be high and as the epochs progresses the LR reduces with decreasing margin. 
   This approach helps to reach sweet spot of the learning curve.

6. Best model with highest validation accuracy is reported for Epoch number 48


Model Summary with Output dimensions and Global Receptive Field for each layer
------------------------------------------------------------------------------
# Define the model
model1 = Sequential(name = "model_CIFAR10")

# Convolution Block-1
model1.add(SeparableConv2D(32, (3, 3), padding='same', input_shape=(32, 32, 3), use_bias = False, name="blk1_ly1_conv")) #OUT: 32X32X32, GRF: 3X3
model1.add(Activation('relu'))
model1.add(BatchNormalization(name="blk1_ly1_batchnorm"))
model1.add(SeparableConv2D(64, (3, 3), use_bias = False, name="blk1_ly2_conv")) #OUT: 30X30X64, GRF: 5X5
model1.add(Activation('relu'))
model1.add(BatchNormalization(name="blk1_ly2_batchnorm"))

model1.add(MaxPooling2D(pool_size=(2, 2), name="blk1_maxpool")) #OUT: 15X15X64, GRF: 6X6
model1.add(Convolution2D(32, (1, 1), activation='relu', use_bias = False, name="blk1_combine")) #OUT: 15X15X32, GRF: 6X6
model1.add(Dropout(0.20, name="blk1_ly2_dropout"))

# Convolution Block-2
model1.add(SeparableConv2D(64, (3, 3), padding='same', use_bias = False, name="blk2_ly1_conv")) #OUT: 15X15X64, GRF: 10X10
model1.add(Activation('relu'))
model1.add(BatchNormalization(name="blk2_ly1_batchnorm"))
model1.add(SeparableConv2D(96, (3, 3), use_bias = False, name="blk2_ly2_conv")) #OUT: 13X13X96, GRF: 14X14
model1.add(Activation('relu'))
model1.add(BatchNormalization(name="blk2_ly2_batchnorm"))
model1.add(SeparableConv2D(128, (3, 3), use_bias = False, name="blk2_ly3_conv")) #OUT: 11X11X128, GRF: 18X18
model1.add(Activation('relu'))
model1.add(BatchNormalization(name="blk2_ly3_batchnorm"))

model1.add(MaxPooling2D(pool_size=(2, 2), name="blk2_maxpool")) #OUT: 5X5X128, GRF: 20X20
model1.add(Convolution2D(64, (1, 1), activation='relu', use_bias = False, name="blk2_combine")) #OUT: 5X5X64, GRF: 20X20
model1.add(Dropout(0.20, name="blk2_ly3_dropout"))

# Convolution Block-3
model1.add(SeparableConv2D(96, (3, 3), padding='same', use_bias = False, name="blk3_ly1_conv")) #OUT: 5X5X96, GRF: 28X28
model1.add(Activation('relu'))
model1.add(BatchNormalization(name="blk3_ly1_batchnorm"))
model1.add(SeparableConv2D(128, (3, 3), use_bias = False, name="blk3_ly2_conv")) #OUT: 3X3X128, GRF: 36X36
model1.add(Activation('relu'))
model1.add(BatchNormalization(name="blk3_ly2_batchnorm"))
model1.add(Dropout(0.20, name="blk3_ly2_dropout"))

# GAP: just to get num_classes channels as we have num_classes of classes at output
model1.add(SeparableConv2D(num_classes, (3, 3), use_bias = False, name="blk4_ly1_conv")) #OUT: 1X1X10, GRF: 44X44

model1.add(Flatten(name='flatten'))
model1.add(Activation('softmax', name="output"))


Model Epoch History
-------------------

Epoch 1/50

Epoch 00001: LearningRateScheduler setting learning rate to 0.01.
390/390 [==============================] - 17s 44ms/step - loss: 1.4296 - acc: 0.4794 - val_loss: 2.8050 - val_acc: 0.4509
Epoch 2/50

Epoch 00002: LearningRateScheduler setting learning rate to 0.0075815011.
390/390 [==============================] - 15s 38ms/step - loss: 1.0399 - acc: 0.6292 - val_loss: 1.8623 - val_acc: 0.4838
Epoch 3/50

Epoch 00003: LearningRateScheduler setting learning rate to 0.0061050061.
390/390 [==============================] - 15s 38ms/step - loss: 0.8911 - acc: 0.6860 - val_loss: 0.9151 - val_acc: 0.6913
Epoch 4/50

Epoch 00004: LearningRateScheduler setting learning rate to 0.005109862.
390/390 [==============================] - 15s 37ms/step - loss: 0.7888 - acc: 0.7212 - val_loss: 0.7936 - val_acc: 0.7320
Epoch 5/50

Epoch 00005: LearningRateScheduler setting learning rate to 0.0043936731.
390/390 [==============================] - 15s 38ms/step - loss: 0.7224 - acc: 0.7470 - val_loss: 0.7813 - val_acc: 0.7342
Epoch 6/50

Epoch 00006: LearningRateScheduler setting learning rate to 0.0038535645.
390/390 [==============================] - 15s 38ms/step - loss: 0.6710 - acc: 0.7649 - val_loss: 0.6851 - val_acc: 0.7629
Epoch 7/50

Epoch 00007: LearningRateScheduler setting learning rate to 0.003431709.
390/390 [==============================] - 15s 38ms/step - loss: 0.6275 - acc: 0.7798 - val_loss: 0.6702 - val_acc: 0.7675
Epoch 8/50

Epoch 00008: LearningRateScheduler setting learning rate to 0.0030931024.
390/390 [==============================] - 15s 38ms/step - loss: 0.5920 - acc: 0.7928 - val_loss: 0.6342 - val_acc: 0.7837
Epoch 9/50

Epoch 00009: LearningRateScheduler setting learning rate to 0.0028153153.
390/390 [==============================] - 15s 38ms/step - loss: 0.5660 - acc: 0.8008 - val_loss: 0.6366 - val_acc: 0.7872
Epoch 10/50

Epoch 00010: LearningRateScheduler setting learning rate to 0.0025833118.
390/390 [==============================] - 15s 38ms/step - loss: 0.5391 - acc: 0.8108 - val_loss: 0.6280 - val_acc: 0.7905
Epoch 11/50

Epoch 00011: LearningRateScheduler setting learning rate to 0.0023866348.
390/390 [==============================] - 15s 38ms/step - loss: 0.5171 - acc: 0.8195 - val_loss: 0.6790 - val_acc: 0.7785
Epoch 12/50

Epoch 00012: LearningRateScheduler setting learning rate to 0.0022177866.
390/390 [==============================] - 15s 38ms/step - loss: 0.4977 - acc: 0.8267 - val_loss: 0.7008 - val_acc: 0.7706
Epoch 13/50

Epoch 00013: LearningRateScheduler setting learning rate to 0.002071251.
390/390 [==============================] - 15s 38ms/step - loss: 0.4824 - acc: 0.8312 - val_loss: 0.5993 - val_acc: 0.8042
Epoch 14/50

Epoch 00014: LearningRateScheduler setting learning rate to 0.0019428793.
390/390 [==============================] - 15s 37ms/step - loss: 0.4718 - acc: 0.8358 - val_loss: 0.5770 - val_acc: 0.8095
Epoch 15/50

Epoch 00015: LearningRateScheduler setting learning rate to 0.0018294914.
390/390 [==============================] - 15s 38ms/step - loss: 0.4542 - acc: 0.8399 - val_loss: 0.6420 - val_acc: 0.7865
Epoch 16/50

Epoch 00016: LearningRateScheduler setting learning rate to 0.0017286085.
390/390 [==============================] - 15s 37ms/step - loss: 0.4424 - acc: 0.8455 - val_loss: 0.5650 - val_acc: 0.8112
Epoch 17/50

Epoch 00017: LearningRateScheduler setting learning rate to 0.00163827.
390/390 [==============================] - 15s 38ms/step - loss: 0.4291 - acc: 0.8484 - val_loss: 0.5691 - val_acc: 0.8120
Epoch 18/50

Epoch 00018: LearningRateScheduler setting learning rate to 0.0015569049.
390/390 [==============================] - 15s 37ms/step - loss: 0.4219 - acc: 0.8519 - val_loss: 0.5601 - val_acc: 0.8155
Epoch 19/50

Epoch 00019: LearningRateScheduler setting learning rate to 0.0014832394.
390/390 [==============================] - 15s 37ms/step - loss: 0.4082 - acc: 0.8562 - val_loss: 0.5514 - val_acc: 0.8164
Epoch 20/50

Epoch 00020: LearningRateScheduler setting learning rate to 0.00141623.
390/390 [==============================] - 15s 38ms/step - loss: 0.4023 - acc: 0.8593 - val_loss: 0.5557 - val_acc: 0.8176
Epoch 21/50

Epoch 00021: LearningRateScheduler setting learning rate to 0.0013550136.
390/390 [==============================] - 15s 38ms/step - loss: 0.3907 - acc: 0.8611 - val_loss: 0.5566 - val_acc: 0.8186
Epoch 22/50

Epoch 00022: LearningRateScheduler setting learning rate to 0.00129887.
390/390 [==============================] - 15s 37ms/step - loss: 0.3871 - acc: 0.8627 - val_loss: 0.5615 - val_acc: 0.8159
Epoch 23/50

Epoch 00023: LearningRateScheduler setting learning rate to 0.0012471938.
390/390 [==============================] - 15s 38ms/step - loss: 0.3765 - acc: 0.8670 - val_loss: 0.5700 - val_acc: 0.8162
Epoch 24/50

Epoch 00024: LearningRateScheduler setting learning rate to 0.0011994722.
390/390 [==============================] - 15s 38ms/step - loss: 0.3710 - acc: 0.8683 - val_loss: 0.5597 - val_acc: 0.8153
Epoch 25/50

Epoch 00025: LearningRateScheduler setting learning rate to 0.001155268.
390/390 [==============================] - 15s 38ms/step - loss: 0.3636 - acc: 0.8724 - val_loss: 0.5685 - val_acc: 0.8192
Epoch 26/50

Epoch 00026: LearningRateScheduler setting learning rate to 0.0011142061.
390/390 [==============================] - 15s 38ms/step - loss: 0.3584 - acc: 0.8741 - val_loss: 0.5630 - val_acc: 0.8171
Epoch 27/50

Epoch 00027: LearningRateScheduler setting learning rate to 0.001075963.
390/390 [==============================] - 15s 37ms/step - loss: 0.3547 - acc: 0.8744 - val_loss: 0.5626 - val_acc: 0.8200
Epoch 28/50

Epoch 00028: LearningRateScheduler setting learning rate to 0.001040258.
390/390 [==============================] - 15s 38ms/step - loss: 0.3519 - acc: 0.8728 - val_loss: 0.5512 - val_acc: 0.8222
Epoch 29/50

Epoch 00029: LearningRateScheduler setting learning rate to 0.0010068466.
390/390 [==============================] - 15s 38ms/step - loss: 0.3390 - acc: 0.8777 - val_loss: 0.5675 - val_acc: 0.8202
Epoch 30/50

Epoch 00030: LearningRateScheduler setting learning rate to 0.0009755146.
390/390 [==============================] - 15s 38ms/step - loss: 0.3404 - acc: 0.8781 - val_loss: 0.5539 - val_acc: 0.8222
Epoch 31/50

Epoch 00031: LearningRateScheduler setting learning rate to 0.0009460738.
390/390 [==============================] - 15s 37ms/step - loss: 0.3343 - acc: 0.8790 - val_loss: 0.5591 - val_acc: 0.8236
Epoch 32/50

Epoch 00032: LearningRateScheduler setting learning rate to 0.000918358.
390/390 [==============================] - 15s 38ms/step - loss: 0.3300 - acc: 0.8820 - val_loss: 0.5593 - val_acc: 0.8246
Epoch 33/50

Epoch 00033: LearningRateScheduler setting learning rate to 0.0008922198.
390/390 [==============================] - 15s 38ms/step - loss: 0.3239 - acc: 0.8834 - val_loss: 0.5600 - val_acc: 0.8273
Epoch 34/50

Epoch 00034: LearningRateScheduler setting learning rate to 0.0008675284.
390/390 [==============================] - 15s 37ms/step - loss: 0.3176 - acc: 0.8872 - val_loss: 0.5691 - val_acc: 0.8224
Epoch 35/50

Epoch 00035: LearningRateScheduler setting learning rate to 0.0008441668.
390/390 [==============================] - 15s 38ms/step - loss: 0.3238 - acc: 0.8832 - val_loss: 0.5573 - val_acc: 0.8270
Epoch 36/50

Epoch 00036: LearningRateScheduler setting learning rate to 0.0008220304.
390/390 [==============================] - 15s 37ms/step - loss: 0.3113 - acc: 0.8895 - val_loss: 0.5643 - val_acc: 0.8286
Epoch 37/50

Epoch 00037: LearningRateScheduler setting learning rate to 0.0008010253.
390/390 [==============================] - 15s 37ms/step - loss: 0.3087 - acc: 0.8890 - val_loss: 0.5625 - val_acc: 0.8280
Epoch 38/50

Epoch 00038: LearningRateScheduler setting learning rate to 0.0007810669.
390/390 [==============================] - 14s 37ms/step - loss: 0.3128 - acc: 0.8872 - val_loss: 0.5568 - val_acc: 0.8268
Epoch 39/50

Epoch 00039: LearningRateScheduler setting learning rate to 0.000762079.
390/390 [==============================] - 15s 38ms/step - loss: 0.3055 - acc: 0.8899 - val_loss: 0.5596 - val_acc: 0.8255
Epoch 40/50

Epoch 00040: LearningRateScheduler setting learning rate to 0.0007439923.
390/390 [==============================] - 15s 37ms/step - loss: 0.3025 - acc: 0.8916 - val_loss: 0.5634 - val_acc: 0.8291
Epoch 41/50

Epoch 00041: LearningRateScheduler setting learning rate to 0.0007267442.
390/390 [==============================] - 15s 37ms/step - loss: 0.3016 - acc: 0.8909 - val_loss: 0.5679 - val_acc: 0.8264
Epoch 42/50

Epoch 00042: LearningRateScheduler setting learning rate to 0.0007102777.
390/390 [==============================] - 15s 37ms/step - loss: 0.2915 - acc: 0.8951 - val_loss: 0.5855 - val_acc: 0.8271
Epoch 43/50

Epoch 00043: LearningRateScheduler setting learning rate to 0.0006945409.
390/390 [==============================] - 15s 37ms/step - loss: 0.2946 - acc: 0.8953 - val_loss: 0.5660 - val_acc: 0.8265
Epoch 44/50

Epoch 00044: LearningRateScheduler setting learning rate to 0.0006794863.
390/390 [==============================] - 15s 37ms/step - loss: 0.2938 - acc: 0.8927 - val_loss: 0.5763 - val_acc: 0.8256
Epoch 45/50

Epoch 00045: LearningRateScheduler setting learning rate to 0.0006650705.
390/390 [==============================] - 15s 38ms/step - loss: 0.2884 - acc: 0.8958 - val_loss: 0.5729 - val_acc: 0.8251
Epoch 46/50

Epoch 00046: LearningRateScheduler setting learning rate to 0.0006512537.
390/390 [==============================] - 15s 38ms/step - loss: 0.2842 - acc: 0.8974 - val_loss: 0.5884 - val_acc: 0.8256
Epoch 47/50

Epoch 00047: LearningRateScheduler setting learning rate to 0.0006379992.
390/390 [==============================] - 15s 37ms/step - loss: 0.2833 - acc: 0.8971 - val_loss: 0.5746 - val_acc: 0.8269
Epoch 48/50

Epoch 00048: LearningRateScheduler setting learning rate to 0.0006252736.
390/390 [==============================] - 15s 37ms/step - loss: 0.2795 - acc: 0.8990 - val_loss: 0.5685 - val_acc: 0.8314
Epoch 49/50

Epoch 00049: LearningRateScheduler setting learning rate to 0.0006130456.
390/390 [==============================] - 15s 37ms/step - loss: 0.2816 - acc: 0.8984 - val_loss: 0.5693 - val_acc: 0.8290
Epoch 50/50

Epoch 00050: LearningRateScheduler setting learning rate to 0.0006012868.
390/390 [==============================] - 15s 38ms/step - loss: 0.2791 - acc: 0.9002 - val_loss: 0.5742 - val_acc: 0.8287
Model took 737.01 seconds to train

