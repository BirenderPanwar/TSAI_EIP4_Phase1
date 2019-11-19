
DNN: MNIST MODEL: Test Score: [0.0291597472808759, 0.9929]
----------------------------------------------------------

1. Convolution: 
Convolution network helps to extract features from the input data/image. It simulates the layered architecture of our brain as edge extractor, texture, pattern, component and finally an object extractor. 
It makes use of Kernels/Filters which when applied on the input data results in feature maps.


2. Filters/Kernels: 
Filters are the parameters which when convolved on the inputs data results in feature maps. 
The number of channels in the filter is the same as the number of input channels. These filters are learned and adjusted during back-propagation. 
Mostly we use odd kernel size due to its advantage of symmetry in nature to extract edges/gradients.


3. Epochs: 
It is the number of time network visit each sample of training data during back-propagation.
For example epochs of 10 mean, the network will see the entire training dataset 10 times to adjust the features of each convolution and dense layers.


4. 1x1 Convolution: 
1x1 Convolution is used to combine features of a large number of input channels to the lower number of output channels. 
This convolution helps to reduce the depth of the networks by retaining all the important features learned by the network in previous layers.
It is mostly placed after the max-pooling layer of each convolution block. 


5. 3x3 Convolution: 
3x3 Convolution is a powerful kernel size as we can create any higher kernel size just by combining various layers of 3X3.
This approach needs a lesser number of kernel parameters compared to when higher kernel size is used directly.
3X3 being an odd kernel size have the advantage of symmetric behavior which is useful for edge detection. 
it is widely used due to it's good support for hardware accelerators.

6. Feature Maps: 
Feature Maps is the collection of Feature Map which in turn is collections of similar features. 
Each Feature Map is a channel containing the same kind of information such as either specific color, all the horizontal edges, etc.


7. Activation Function: 
Convolution network does two important things, extracting the important features and ignoring the unwanted features.
Activation functions help to decide which features shall be filtered for the next layer and which one to be ignored.
it makes use of non-linear transformation on the output of the layer.


8. Receptive Field: 
Local Receptive field(LRF) is the size of the kernel of that layer. Global Receptive filed(GRF) is the total number of pixels the
layer has seen so far through all previous layers. The last layer must have GRF of at least the size of the object in the image for the network to learn about the object in the input image.
