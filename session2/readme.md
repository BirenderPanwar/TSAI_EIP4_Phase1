
DNN: MNIST MODEL: Test Score: [0.017225117182139364, 0.9954]
------------------------------------------------------------

Total params: 12,328
------------

Following Strategy is used to optimize the model performance:
-------------------------------------------------------------

1. Batch Normalization and Dropout layer is removed from the last convolution layer. 
   As this Conv layer is closest to the output layer, it does not make sense to drop features learnt so far at this layer.
   This helped to bring training and validation accuracy closer.
   
2. To reduced the network parameters, the number of kernels is reduced in first convolution block to 8 and 16.
   this helped to reduced the number of network parameters and still acheiving validation accuracy above 94%
   
3. Similarlly number of kernels is set to 8 for 1x1 layer of first Conv block. this further helped to reduce the NW parameters.
   1X1 layer is used to combine the existing channels and create fewer number of complex channels and helped in depth reduction.
   Doing so helped to reduced number of parameters needed for next layer.

4. Learning Rate scheduler is applied such that learning rate(LR) reduces after each epochs. For initial epochs learning rate will be high 
   and as the epochs progresses the LR reduces with decreasing margin. 
   This approach helps to reach sweet spot of the learning curve.

5. Dropout and BatchNormalization are used for each convolution layer except for the last layer to avoid overfitting of the model.

6. Final Model is build after experimenting with many iterations as changes were made one at a time.
   For every model build, kernals was visualized to understand what features are extracted and 
   how accuracy is impacted to help taking decision for next step.

7. Model is built using 2 convolution block. First block with 2 layer followed by 1X1 and Maxpool layer. 
   Second conv block having 4 convolution layers with 16 kernels each. 
   Each conv layers except last one is using activation fxn as ReLU for non-linear transformation.


Model Epoch History
-------------------

Train on 60000 samples, validate on 10000 samples
Epoch 1/20

Epoch 00001: LearningRateScheduler setting learning rate to 0.003.
60000/60000 [==============================] - 12s 206us/step - loss: 0.2319 - acc: 0.9266 - val_loss: 0.0595 - val_acc: 0.9803
Epoch 2/20

Epoch 00002: LearningRateScheduler setting learning rate to 0.0022744503.
60000/60000 [==============================] - 9s 146us/step - loss: 0.0705 - acc: 0.9778 - val_loss: 0.0429 - val_acc: 0.9858
Epoch 3/20

Epoch 00003: LearningRateScheduler setting learning rate to 0.0018315018.
60000/60000 [==============================] - 9s 147us/step - loss: 0.0521 - acc: 0.9833 - val_loss: 0.0330 - val_acc: 0.9888
Epoch 4/20

Epoch 00004: LearningRateScheduler setting learning rate to 0.0015329586.
60000/60000 [==============================] - 9s 148us/step - loss: 0.0448 - acc: 0.9857 - val_loss: 0.0291 - val_acc: 0.9900
Epoch 5/20

Epoch 00005: LearningRateScheduler setting learning rate to 0.0013181019.
60000/60000 [==============================] - 9s 153us/step - loss: 0.0389 - acc: 0.9878 - val_loss: 0.0250 - val_acc: 0.9911
Epoch 6/20

Epoch 00006: LearningRateScheduler setting learning rate to 0.0011560694.
60000/60000 [==============================] - 9s 153us/step - loss: 0.0365 - acc: 0.9882 - val_loss: 0.0272 - val_acc: 0.9907
Epoch 7/20

Epoch 00007: LearningRateScheduler setting learning rate to 0.0010295127.
60000/60000 [==============================] - 9s 157us/step - loss: 0.0338 - acc: 0.9895 - val_loss: 0.0232 - val_acc: 0.9925
Epoch 8/20

Epoch 00008: LearningRateScheduler setting learning rate to 0.0009279307.
60000/60000 [==============================] - 10s 158us/step - loss: 0.0309 - acc: 0.9903 - val_loss: 0.0212 - val_acc: 0.9928
Epoch 9/20

Epoch 00009: LearningRateScheduler setting learning rate to 0.0008445946.
60000/60000 [==============================] - 9s 157us/step - loss: 0.0269 - acc: 0.9913 - val_loss: 0.0191 - val_acc: 0.9942
Epoch 10/20

Epoch 00010: LearningRateScheduler setting learning rate to 0.0007749935.
60000/60000 [==============================] - 9s 158us/step - loss: 0.0275 - acc: 0.9914 - val_loss: 0.0233 - val_acc: 0.9924
Epoch 11/20

Epoch 00011: LearningRateScheduler setting learning rate to 0.0007159905.
60000/60000 [==============================] - 9s 157us/step - loss: 0.0254 - acc: 0.9919 - val_loss: 0.0190 - val_acc: 0.9942
Epoch 12/20

Epoch 00012: LearningRateScheduler setting learning rate to 0.000665336.
60000/60000 [==============================] - 9s 156us/step - loss: 0.0249 - acc: 0.9918 - val_loss: 0.0179 - val_acc: 0.9939
Epoch 13/20

Epoch 00013: LearningRateScheduler setting learning rate to 0.0006213753.
60000/60000 [==============================] - 9s 158us/step - loss: 0.0246 - acc: 0.9920 - val_loss: 0.0171 - val_acc: 0.9938
Epoch 14/20

Epoch 00014: LearningRateScheduler setting learning rate to 0.0005828638.
60000/60000 [==============================] - 9s 157us/step - loss: 0.0229 - acc: 0.9926 - val_loss: 0.0174 - val_acc: 0.9953
Epoch 15/20

Epoch 00015: LearningRateScheduler setting learning rate to 0.0005488474.
60000/60000 [==============================] - 9s 158us/step - loss: 0.0220 - acc: 0.9928 - val_loss: 0.0171 - val_acc: 0.9949
Epoch 16/20

Epoch 00016: LearningRateScheduler setting learning rate to 0.0005185825.
60000/60000 [==============================] - 9s 157us/step - loss: 0.0208 - acc: 0.9929 - val_loss: 0.0160 - val_acc: 0.9952
Epoch 17/20

Epoch 00017: LearningRateScheduler setting learning rate to 0.000491481.
60000/60000 [==============================] - 10s 161us/step - loss: 0.0202 - acc: 0.9933 - val_loss: 0.0185 - val_acc: 0.9944
Epoch 18/20

Epoch 00018: LearningRateScheduler setting learning rate to 0.0004670715.
60000/60000 [==============================] - 9s 157us/step - loss: 0.0198 - acc: 0.9937 - val_loss: 0.0168 - val_acc: 0.9945
Epoch 19/20

Epoch 00019: LearningRateScheduler setting learning rate to 0.0004449718.
60000/60000 [==============================] - 10s 159us/step - loss: 0.0190 - acc: 0.9934 - val_loss: 0.0161 - val_acc: 0.9952
Epoch 20/20

Epoch 00020: LearningRateScheduler setting learning rate to 0.000424869.
60000/60000 [==============================] - 9s 158us/step - loss: 0.0199 - acc: 0.9932 - val_loss: 0.0172 - val_acc: 0.9954

