Assignment-5B: Person Attribute Model
-------------------------------------

Open souce used
---------------

1. Base ResNet code is taken from https://keras.io/examples/cifar10_resnet/ for model building. 
   This resnet code is further customize to build the model for this assignment.
   
2. CyclicLR: https://github.com/bckenstler/CLR

Attempt-1: VGG16 
----------------

File: PersonAttrubutes_Attempt1_VGG16.ipynb

1. VGG16 arcitecture

2. Network is trained form scratch and initial weight is set as None

3. Image augmentation: Cutout (size (32,32)

4. CyclicLR: Exponential Cycle Policy is used.

5. Batch size=32

6. Seperate build tower is created for age and image_quality class with higher neurons 
   as for these class prediction accuracies are reporting less compare to other classes

7. model is first trained for 60 epochs and then 20 epochs. it was executed one after the 
   another to ensure there is no data leak

8. Validation Results:

Total Validation Loss:  7.674099153087985
Validation Accuracies: 

{
'age_output_acc': 0.3956653225806452,

'bag_output_acc': 0.5559475806451613,

'emotion_output_acc': 0.7001008064516129,

'footwear_output_acc': 0.6008064516129032,

'gender_output_acc': 0.6123991935483871,

'image_quality_output_acc': 0.5488911290322581,

'pose_output_acc': 0.6204637096774194,

'weight_output_acc': 0.6466733870967742

}
 

Attempt-2: ResNet32v1
---------------------

File: PersonAttrubutes_Attempt2_ResNet32v1.ipynb

1. ResNet20v1 architecture is used.

1.1 Three stage network is created with 5 Blocks and each Blocks with Stacks of 2 x (3 x 3) Conv2D-ReLU.
Hence total of stages(3) * blocks(5) * convlayer(2) + 2 = 32 layer.

1.2 Number of Kernels is increased for each Stages to help learn some complex features: 
Features maps sizes:
stage 0: 224x224, 16
stage 1: 112X112, 32
stage 2: 56X56, 48

1.3 For down sampling, existing network was using 3X3 Kernel with Strides=2 that reduces the input dimension by half.
This is replaces by MaxPooling(pool_size(2,2)) followed by 1X1 Kernel

1.4 Batch Normalization is not used as with Batch normalization layer, network not learning at all.

2. Image Augmentation Applied: Cutout(size 32X32). custom code is written to handle black portion of the image on left and right side.
   mean of the image is taken for the center and left and right portion of 67 pixels is not used for mean calculation.

3. CyclicLR is implemented. Exponential Cycle policy 

4. Batch_size=64, epoch=96, MIN_LR = 1e-7, MAX_LR = 1e-3

5. Validation Results

Best model at Epoch=10

Total Validation Loss:  7.6698704534961335

Validation Accuracies: 

{

	'age_output_acc': 0.3845766129032258,
	
	'bag_output_acc': 0.5650201612903226,
	
	'emotion_output_acc': 0.7147177419354839,
	
	'footwear_output_acc': 0.623991935483871,
	
	'gender_output_acc': 0.6532258064516129,
	
	'image_quality_output_acc': 0.5539314516129032,
	
	'pose_output_acc': 0.6431451612903226,
	
	'weight_output_acc': 0.6335685483870968
	
}

Attempt-3: ResNet20v1 with Dropout layer
-----------------------------------------

File: PersonAttrubutes_Attempt3_ResNet20v1.ipynb

1. Attempt is made to try with light model. here architetcure is same as above only difference is that each stage with 3 block. 
   Hence total of stages(3) * blocks(3) * convlayer(2) + 2 = 20 layer.

2. As previous model is overfitting after epoch=10, so Dropout layer is added at the end of each stage to reduce over fitting

3. Validation Results

Best model at Epoch=10

Total Validation Loss:  7.429036955679616

Validation Accuracy Score:

{

	'age_output_acc': 0.3709677419354839,
	
	'bag_output_acc': 0.5645161290322581,
	
	'emotion_output_acc': 0.7106854838709677,
	
	'footwear_output_acc': 0.6466733870967742,
	
	'gender_output_acc': 0.7011088709677419,
	
	'image_quality_output_acc': 0.5554435483870968,
	
	'pose_output_acc': 0.7036290322580645,
	
	'weight_output_acc': 0.6320564516129032
	
 }



