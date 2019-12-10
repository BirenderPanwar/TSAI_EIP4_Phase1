
Assignment-4A: Image Annotation is completed through Web Portal
-----------------------------------------------------------------

Assignment-4B: CIFAR10 dataset. Model built using ResNet Architetcure
----------------------------------------------------------------------

1. Base ResNet code is taken from https://keras.io/examples/cifar10_resnet/ for model building
2. GradCAM code is integarted as provided by TSAI. This is used to get the heat map visualization for few test images.


Attempt-1: Test Score: 87.37 at Epochs=30
-----------------------------------------

1. ResNet20v1 architecture used as it is.

2. Image Augmentation Applied: Horizontal Flip, Cutout(size 16X16)

3. ReduceLROnPlataeu callback to adjust Learning Rate on Plataeu Region: patience=2


Attempt-2: Test Score: 88.23 at Epochs=41
-----------------------------------------
As ResNet20v1 architecture comes with 18 Conv layers so no attempt is made to change the number of conv layers.
However, few additional stategy is applied to modified the working of this architecture to acheive 88+ accuracy as below:

1. For down sampling, existing network was using 3X3 Kernel with Strides=2 that reduces the input dimension by half.
This is replaces by MaxPooling(pool_size(2,2)) followed by 1X1 Kernel

2. It is observed that network was not learning and this is observed by looking into training accuracy which was not improving. 
hence Number of Kernels is increased for each Stages to help learn some complex features: 
Features maps sizes:
stage 0: 32x32, 16  ==> Changed to 32X32, 32
stage 1: 16x16, 32  ==> Changed to 16X16, 64
stage 2:  8x8,  64  ==> Changed to 8X8, 96
 

Attempt-3: Test Score: 88.42 at Epochs=29
-----------------------------------------
In attempt-2, after certain epoch the validation accuracy was not improving even though training accuracy was reporting high.
so dropout is added at the start of each stage in residual path to prevent overfit.
Retained all other augumentation strategy and Network size as in Attempt-1 and Attempt-2 design


Overall Summary on How 88+ accuracy is acheived?
------------------------------------------------

1. ResNet20v1 is used for model building

2. Image Augumentation is added. Horizontal flip and Cutout are used

3. How cutout function is implemented?
Cutout function is implemented and passed as preprocessing function in ImageDataGenerator
As input image size is 32X32. Padding of 16 is added hence making size to 64X64.
(x,y) cordinates which represent the left top cordinates are selected at random between (0~48, 0~48). 
and Cutout size is kept as 16X16. Cutout region is filled with mean value of each channel.
and finally center 32X32 is used as output. This making original image with cutout region of various sizes at random.

4. ReduceLROnPlateau callback is used to change the Learning rate when model is stuck in Plateau region.
patience=2 is used which help to change the LR when loss didn't improved for consicutive 2 Epochs. This helped in faster convergence.

5. As ResNet20v1 architecture comes with 18 Conv layers so no attempt is made to change the number of conv layers.
However, few additional stategy is applied to modified the working of this architecture to acheive 88+ accuracy as below:

5.1 For down sampling, existing network was using 3X3 Kernel with Strides=2 that reduces the input dimension by half.
This is replaces by MaxPooling(pool_size(2,2)) followed by 1X1 Kernel

5.2. It is observed that network was not learning and this is observed by looking into training accuracy which was not improving. 
hence Number of Kernels is increased for each Stages to help learn some complex features: 
Stage-0: Number of kernels: 32
Stage-1: Number of kernels: 64  
Stage-2: Number of kernels: 96  

5.3. After certain epoch the validation accuracy was not improving even though training accuracy was reporting high.
so dropout is added at the start of each stage in residual path to prevent overfit

6. Best model with highest validation accuracy of 88.42 reported for Epoch number 29


Model Epoch History(Attempt-3)
------------------------------

Epoch 1/50
391/391 [==============================] - 76s 196ms/step - loss: 1.9766 - acc: 0.3888 - val_loss: 1.8454 - val_acc: 0.4470

Epoch 00001: val_acc improved from -inf to 0.44700, saving model to /content/saved_models7/cifar10_ResNet20v1_model_best.h5
Epoch 2/50
391/391 [==============================] - 71s 181ms/step - loss: 1.4401 - acc: 0.5667 - val_loss: 1.6029 - val_acc: 0.5183

Epoch 00002: val_acc improved from 0.44700 to 0.51830, saving model to /content/saved_models7/cifar10_ResNet20v1_model_best.h5
Epoch 3/50
391/391 [==============================] - 70s 180ms/step - loss: 1.3097 - acc: 0.6214 - val_loss: 2.5952 - val_acc: 0.4437

Epoch 00003: val_acc did not improve from 0.51830
Epoch 4/50
391/391 [==============================] - 70s 180ms/step - loss: 1.2090 - acc: 0.6645 - val_loss: 1.6160 - val_acc: 0.5429

Epoch 00004: val_acc improved from 0.51830 to 0.54290, saving model to /content/saved_models7/cifar10_ResNet20v1_model_best.h5
Epoch 5/50
391/391 [==============================] - 70s 179ms/step - loss: 0.9552 - acc: 0.7404 - val_loss: 0.9633 - val_acc: 0.7252

Epoch 00005: val_acc improved from 0.54290 to 0.72520, saving model to /content/saved_models7/cifar10_ResNet20v1_model_best.h5
Epoch 6/50
391/391 [==============================] - 70s 180ms/step - loss: 0.8720 - acc: 0.7629 - val_loss: 0.8917 - val_acc: 0.7618

Epoch 00006: val_acc improved from 0.72520 to 0.76180, saving model to /content/saved_models7/cifar10_ResNet20v1_model_best.h5
Epoch 7/50
391/391 [==============================] - 70s 179ms/step - loss: 0.8328 - acc: 0.7762 - val_loss: 0.9060 - val_acc: 0.7560

Epoch 00007: val_acc did not improve from 0.76180
Epoch 8/50
391/391 [==============================] - 70s 180ms/step - loss: 0.8000 - acc: 0.7859 - val_loss: 0.8778 - val_acc: 0.7611

Epoch 00008: val_acc did not improve from 0.76180
Epoch 9/50
391/391 [==============================] - 70s 179ms/step - loss: 0.7762 - acc: 0.7957 - val_loss: 0.7819 - val_acc: 0.7874

Epoch 00009: val_acc improved from 0.76180 to 0.78740, saving model to /content/saved_models7/cifar10_ResNet20v1_model_best.h5
Epoch 10/50
391/391 [==============================] - 70s 179ms/step - loss: 0.7549 - acc: 0.8018 - val_loss: 0.8676 - val_acc: 0.7663

Epoch 00010: val_acc did not improve from 0.78740
Epoch 11/50
391/391 [==============================] - 70s 180ms/step - loss: 0.7443 - acc: 0.8048 - val_loss: 0.9821 - val_acc: 0.7419

Epoch 00011: val_acc did not improve from 0.78740
Epoch 12/50
391/391 [==============================] - 70s 179ms/step - loss: 0.6272 - acc: 0.8433 - val_loss: 0.6961 - val_acc: 0.8232

Epoch 00012: val_acc improved from 0.78740 to 0.82320, saving model to /content/saved_models7/cifar10_ResNet20v1_model_best.h5
Epoch 13/50
391/391 [==============================] - 70s 179ms/step - loss: 0.5814 - acc: 0.8542 - val_loss: 0.6177 - val_acc: 0.8482

Epoch 00013: val_acc improved from 0.82320 to 0.84820, saving model to /content/saved_models7/cifar10_ResNet20v1_model_best.h5
Epoch 14/50
391/391 [==============================] - 70s 179ms/step - loss: 0.5592 - acc: 0.8600 - val_loss: 0.6229 - val_acc: 0.8377

Epoch 00014: val_acc did not improve from 0.84820
Epoch 15/50
391/391 [==============================] - 70s 179ms/step - loss: 0.5387 - acc: 0.8648 - val_loss: 0.5714 - val_acc: 0.8567

Epoch 00015: val_acc improved from 0.84820 to 0.85670, saving model to /content/saved_models7/cifar10_ResNet20v1_model_best.h5
Epoch 16/50
391/391 [==============================] - 70s 178ms/step - loss: 0.5233 - acc: 0.8691 - val_loss: 0.5919 - val_acc: 0.8529

Epoch 00016: val_acc did not improve from 0.85670
Epoch 17/50
391/391 [==============================] - 70s 178ms/step - loss: 0.5130 - acc: 0.8725 - val_loss: 0.6137 - val_acc: 0.8466

Epoch 00017: val_acc did not improve from 0.85670
Epoch 18/50
391/391 [==============================] - 69s 177ms/step - loss: 0.4540 - acc: 0.8924 - val_loss: 0.5223 - val_acc: 0.8717

Epoch 00018: val_acc improved from 0.85670 to 0.87170, saving model to /content/saved_models7/cifar10_ResNet20v1_model_best.h5
Epoch 19/50
391/391 [==============================] - 70s 178ms/step - loss: 0.4365 - acc: 0.8977 - val_loss: 0.5298 - val_acc: 0.8688

Epoch 00019: val_acc did not improve from 0.87170
Epoch 20/50
391/391 [==============================] - 70s 179ms/step - loss: 0.4241 - acc: 0.9005 - val_loss: 0.5119 - val_acc: 0.8762

Epoch 00020: val_acc improved from 0.87170 to 0.87620, saving model to /content/saved_models7/cifar10_ResNet20v1_model_best.h5
Epoch 21/50
391/391 [==============================] - 69s 178ms/step - loss: 0.4152 - acc: 0.9029 - val_loss: 0.5209 - val_acc: 0.8733

Epoch 00021: val_acc did not improve from 0.87620
Epoch 22/50
391/391 [==============================] - 69s 178ms/step - loss: 0.4077 - acc: 0.9058 - val_loss: 0.5109 - val_acc: 0.8776

Epoch 00022: val_acc improved from 0.87620 to 0.87760, saving model to /content/saved_models7/cifar10_ResNet20v1_model_best.h5
Epoch 23/50
391/391 [==============================] - 69s 178ms/step - loss: 0.3956 - acc: 0.9095 - val_loss: 0.5266 - val_acc: 0.8703

Epoch 00023: val_acc did not improve from 0.87760
Epoch 24/50
391/391 [==============================] - 69s 177ms/step - loss: 0.3892 - acc: 0.9100 - val_loss: 0.5362 - val_acc: 0.8692

Epoch 00024: val_acc did not improve from 0.87760
Epoch 25/50
391/391 [==============================] - 70s 178ms/step - loss: 0.3629 - acc: 0.9203 - val_loss: 0.4969 - val_acc: 0.8819

Epoch 00025: val_acc improved from 0.87760 to 0.88190, saving model to /content/saved_models7/cifar10_ResNet20v1_model_best.h5
Epoch 26/50
391/391 [==============================] - 70s 179ms/step - loss: 0.3616 - acc: 0.9189 - val_loss: 0.5007 - val_acc: 0.8821

Epoch 00026: val_acc improved from 0.88190 to 0.88210, saving model to /content/saved_models7/cifar10_ResNet20v1_model_best.h5
Epoch 27/50
391/391 [==============================] - 70s 178ms/step - loss: 0.3538 - acc: 0.9215 - val_loss: 0.4991 - val_acc: 0.8830

Epoch 00027: val_acc improved from 0.88210 to 0.88300, saving model to /content/saved_models7/cifar10_ResNet20v1_model_best.h5
Epoch 28/50
391/391 [==============================] - 70s 178ms/step - loss: 0.3458 - acc: 0.9258 - val_loss: 0.4973 - val_acc: 0.8830

Epoch 00028: val_acc did not improve from 0.88300
Epoch 29/50
391/391 [==============================] - 70s 178ms/step - loss: 0.3404 - acc: 0.9264 - val_loss: 0.4963 - val_acc: 0.8842

Epoch 00029: val_acc improved from 0.88300 to 0.88420, saving model to /content/saved_models7/cifar10_ResNet20v1_model_best.h5
Epoch 30/50
391/391 [==============================] - 70s 178ms/step - loss: 0.3393 - acc: 0.9272 - val_loss: 0.5015 - val_acc: 0.8827

Epoch 00030: val_acc did not improve from 0.88420
Epoch 31/50
391/391 [==============================] - 70s 178ms/step - loss: 0.3392 - acc: 0.9258 - val_loss: 0.4967 - val_acc: 0.8842

Epoch 00031: val_acc did not improve from 0.88420
Epoch 32/50
391/391 [==============================] - 70s 178ms/step - loss: 0.3334 - acc: 0.9293 - val_loss: 0.4982 - val_acc: 0.8830

Epoch 00032: val_acc did not improve from 0.88420
Epoch 33/50
391/391 [==============================] - 70s 178ms/step - loss: 0.3356 - acc: 0.9273 - val_loss: 0.4988 - val_acc: 0.8826

Epoch 00033: val_acc did not improve from 0.88420
Epoch 34/50
391/391 [==============================] - 70s 178ms/step - loss: 0.3375 - acc: 0.9285 - val_loss: 0.4981 - val_acc: 0.8828

Epoch 00034: val_acc did not improve from 0.88420
Epoch 35/50
391/391 [==============================] - 70s 179ms/step - loss: 0.3315 - acc: 0.9294 - val_loss: 0.4983 - val_acc: 0.8829

Epoch 00035: val_acc did not improve from 0.88420
Epoch 36/50
391/391 [==============================] - 70s 178ms/step - loss: 0.3351 - acc: 0.9290 - val_loss: 0.4981 - val_acc: 0.8834

Epoch 00036: val_acc did not improve from 0.88420
Epoch 37/50
391/391 [==============================] - 70s 179ms/step - loss: 0.3341 - acc: 0.9276 - val_loss: 0.4984 - val_acc: 0.8833

Epoch 00037: val_acc did not improve from 0.88420
Epoch 38/50
391/391 [==============================] - 70s 178ms/step - loss: 0.3349 - acc: 0.9288 - val_loss: 0.4983 - val_acc: 0.8838

Epoch 00038: val_acc did not improve from 0.88420
Epoch 39/50
391/391 [==============================] - 70s 178ms/step - loss: 0.3332 - acc: 0.9298 - val_loss: 0.4983 - val_acc: 0.8832

Epoch 00039: val_acc did not improve from 0.88420
Epoch 40/50
391/391 [==============================] - 70s 178ms/step - loss: 0.3321 - acc: 0.9304 - val_loss: 0.4984 - val_acc: 0.8833

Epoch 00040: val_acc did not improve from 0.88420
Epoch 41/50
391/391 [==============================] - 69s 177ms/step - loss: 0.3318 - acc: 0.9284 - val_loss: 0.4983 - val_acc: 0.8832

Epoch 00041: val_acc did not improve from 0.88420
Epoch 42/50
391/391 [==============================] - 70s 178ms/step - loss: 0.3294 - acc: 0.9312 - val_loss: 0.4981 - val_acc: 0.8834

Epoch 00042: val_acc did not improve from 0.88420
Epoch 43/50
391/391 [==============================] - 69s 178ms/step - loss: 0.3331 - acc: 0.9292 - val_loss: 0.4985 - val_acc: 0.8833

Epoch 00043: val_acc did not improve from 0.88420
Epoch 44/50
391/391 [==============================] - 70s 178ms/step - loss: 0.3315 - acc: 0.9289 - val_loss: 0.4989 - val_acc: 0.8828

Epoch 00044: val_acc did not improve from 0.88420
Epoch 45/50
391/391 [==============================] - 69s 177ms/step - loss: 0.3326 - acc: 0.9293 - val_loss: 0.4986 - val_acc: 0.8832

Epoch 00045: val_acc did not improve from 0.88420
Epoch 46/50
391/391 [==============================] - 69s 177ms/step - loss: 0.3343 - acc: 0.9287 - val_loss: 0.4986 - val_acc: 0.8831

Epoch 00046: val_acc did not improve from 0.88420
Epoch 47/50
391/391 [==============================] - 69s 177ms/step - loss: 0.3358 - acc: 0.9290 - val_loss: 0.4984 - val_acc: 0.8834

Epoch 00047: val_acc did not improve from 0.88420
Epoch 48/50
391/391 [==============================] - 70s 178ms/step - loss: 0.3322 - acc: 0.9291 - val_loss: 0.4989 - val_acc: 0.8837

Epoch 00048: val_acc did not improve from 0.88420
Epoch 49/50
391/391 [==============================] - 69s 177ms/step - loss: 0.3344 - acc: 0.9288 - val_loss: 0.4985 - val_acc: 0.8831

Epoch 00049: val_acc did not improve from 0.88420
Epoch 50/50
391/391 [==============================] - 69s 177ms/step - loss: 0.3314 - acc: 0.9296 - val_loss: 0.4983 - val_acc: 0.8831

Epoch 00050: val_acc did not improve from 0.88420


