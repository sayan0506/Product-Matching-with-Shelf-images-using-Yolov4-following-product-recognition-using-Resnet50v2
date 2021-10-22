# Product-Matching
Matching givewn product in the shelf

## **Problem Statement**

The problem statement here is to detect product images from a shelf of goods in a super market. The shelf may contain 100 product categories. So, we are asked to execute the detection or product matching task in two ways

* case 1: If aproduct image is given, we need to locate the products inside the shelfimage
* case 2: If shelfimage is given to the system, the system should detect list of known products present in the shelf

## **Dataset**

The dataset contains 3153 shelf images, and 300 product images corresponding to 100 different categories containing 3 images for each of the individual caregory.

## **Approach**

1. For Object Detection we have used Yolov4-P5 trained on grocery-shelf images data previously, where the image size is 896
2. The next part is product matching, where we trained a classification model using transfer learning on Resnet50V2 trained on 'imagenet', where we added one softmax layer
3. For fine-tuning the model and due to very less training data, we freezed the resnet50v2, and trained only softmax conatining 209,400 trainable parameters only.
4. To handle lack of training data used data augmentations. For data augmentation we use image-augmentor module, where we utilized the following functionalities-

* Auto-contrast
* Auto-sharpness
* Auto-rotate(-10 to +10 deg)
* Auto-enhance color
* Auto equalize hist

1st apply all on ooriginal image, then applied all on rotated image, so we obtain 10 augmented samples, total samples after augmentation from 300 to 3600.

5. The train, validation split used is (valid+test) ratio 0.1665, so train contains 3400, i.e 34 images for each class, and valid and test are split with 50% split ratio. Thus, test and valid contain single image. 
6. Traained the model for 10 epochs, with LR= 1e-04, batch_size = 16, and got good accuracy on test set.
7. After the classifier is trained, we fetched the detection result for shelf images.
8. After detection is done, traversed through each bounding box, cropped those and pass to classififcation model, the classifier predicts the product id out of it.

## **Implementation**

The detailed implementation can be found in the colab notebook [here]().


## **Reference**


* [Product Matching in eCommerce using deep learning](https://medium.com/walmartglobaltech/product-matching-in-ecommerce-4f19b6aebaca)
* [Transfer Learning in Image Classification: how much training data do we really need?](https://towardsdatascience.com/transfer-learning-in-image-classification-how-much-training-data-do-we-really-need-7fb570abe774)
* [nn-grocery-shelves](https://github.com/empathy87/nn-grocery-shelves)
* [SKU-110K](https://github.com/eg4000/SKU110K_CVPR19)
* 
