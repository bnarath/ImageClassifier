# ImageClassifier
Train and deploy an image classifier with the TensorFlow framework within the Amazon Sagemaker ecosystem


# Seting up Amazon Sagemaker
- Create AWS Account at [AWS Management console](https://aws.amazon.com/console/)
- Search for Amazon Sagemaker in the console
- Create a notebook instance
- Wait until the notebook status says "InService"
- Open a notebook as "conda_tensorflow_p36"

# Download annotated images from [oxford vgg dataset](http://www.robots.ox.ac.uk/~vgg/data/pets/)
They have 37 category pet dataset with roughly 200 images for each class. 
The images have a large variations in scale, pose and lighting.
All images have an associated ground truth annotation of breed, head ROI, and pixel level trimap segmentation.
