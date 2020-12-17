# DCGAN-for-recreating-CIFAR-10-images
Keras implementation of DCGAN

## Part 1: Implement the Discriminator of the DCGAN
The formula that is used to find the size of the kernel is given below:

$W/2 = ((W-K+2*P )/S)+1$

where, W = Size of the tensor, K = Size of the kernel, P =Padding size, S = Stride length.

We know that We have to get a tensor which is half of the size of the input. That is why the expression is equated to W/2. Substitution all the specifications given in the problem, 

$W/2 = ((W-5 +2*P)/2) + 1$

$=> W - 2 = W - 5 + 2*P$

$=> 2*P = 3$

$=> P = 3/2$

This means that a padding size of 1.5 will satisfy the condition to reeduce the tensor size by half. Since a fractional padding size does not make sense, a padding size of 2 can be applied to the input to get the desired size. 


We implement the discriminator for a given input shape as shown in the cell below. Zero padding of size 2 is given to the input tensor after which it is passed through a convolutional layer of kernel size 5 and stride equal to 2. The layer has an activation functon of leaky relu with an alpha parameter of 0.2. The output is now sent to a batch normalization layer after this. These layers are repeated again 3 more times till the desired size of 1x1x1 is obtained as output as given in the description. A dropout layer is added as a regularizer. Now, the ouput is sent to a fully connected layer with a sigmoid activation function to make a binary classification. 


The model is trained with an adam optimizer with a learning rate of 2e-3 and a beta value of 0.5. The model has a binary cross entropy as its loss to be minimized. 

![discriminator](https://drive.google.com/uc?export=view&id=1KrqS_AyjHIm7leVh58pjVRmnGx-le1vB)

## Part 2: 

![generator](https://drive.google.com/uc?export=view&id=18RwpQChvU1FLfnCEfMiJKsKklsOkDyTS)

The generaator was created with the specification provided above with an input latent dimension of 100. The gan model was also created which is the combination of the generator model followed by the discriminator model. 

## Part 3: Training the model
During every epoch of training, following things are happening:
* Randomly sample a batch size of real images and give each of them a "real" label
* Randomly generate a batch size of latent points and create fake images using the generator. Give each fake image a "fake" label.
* Use the data from 1 and 2 as training data to train only the discriminator
Randomly generate a batch size of latent points and create fake images using the generator. Give each fake image a "real" label.
* Use the data to train the combined network. 

The model is also getting saved during every epoch.

## Part 4: Experiment

### 1.   Comment on any differences between their performances. Without running additional experiments, do you think the performance of the classifier trained on fake images will improve if you use 500,000 fake images instead of 50,000?

Every time the c' clasifier is getting trained, it is getting trained on images produced by the GAN which generates image from random latent vectors. When they are created at random, some vectors may come out to be similar with others and as a result, a class may have a lot of images and some classes may h ave a lesser number of images. When the latent vectors are generated at random, it may lead to class imbalance and hence the classifier maybe able to predict one class better than the other. The classifier c' is able to make similar predictions to that of the classifier c but with a lesser confidence score than c almost everytime with mminor descripancies. This may again be due to the class imabalance. When 500,000 images are used to train the classifier c', again the class imabalance may get better or worse and depending on this, the classifier c' may perform better. But even though there might be class imbalances, there would now be enough samples for each class and the classifier might get better. But since it is random, and there is a very small chance where all the 500,000 images can be very similar, the classifier can also perform poor. It all depends on how close the images are going to be from each other. 

### 2.   You might wonder why anyone would use fake data to train classifier. As it turns out, this is a useful strategy to protect privacy of sensitive original data, especially in frederated learning when sensitive data need to be aggregated from many different sites. Inspecting a random set of fake images, do you think that they disclose any of the raw images from the original CIFAR10 dataset? 

The fake images do not disclose any of the raw images in the original CIFAR-10 dataset. They capture the features from the raw images wich inturn reflects the scene and object present but do not reproduce the exact same images.

