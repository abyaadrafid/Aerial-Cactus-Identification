# [Aerial Cactus Identification](https://www.kaggle.com/c/aerial-cactus-identification)

This was one my of very first competitions. While doing [Practical Deep Learning for Coders](https://course.fast.ai/), this competition provided a good source of practise. It was a binary classification probelm. The goal for this competition was to determine whether the given satellite image contained a columnur cactus. 

I used this dataset for two purposes :

1. To implement and test [ArcFace](https://arxiv.org/pdf/1801.07698.pdf) using pytorch.
2. To get placed into a high LeaderBoard position in the competition using FastAI.

## Approach

### EDA 
According to the dataset details, the images were taken from the air. The images are low-res, some of them rotated to arbitrary angles and some zoomed. From visual inspection, the cacti are somewhat easy to spot because of their unique texture and stick-like shape. The class imbalance is not severe, can be handled by data augmentation.
### Data split and Transforms
#### Split : 
As the class imbalance was not servere, the data could be split into train/valid set at random.
#### Transforms : 
Following Transforms were applied with 75% probability to augment the date, then the images were resized to 128*128. Test time augmentation was not applied.  

1. Horizontal Flip
2. Vertical Flip
3. Left and Right rotation upto 10°
4. Upto 110% zoom
### Models
I used DenseNet169 and Resnet101 for Leaderboard and ArcFace for research purposes.

### ArcFace
ArcFace was applied on the Resnet101 backbone. Implemented from scratch in pytorch. With embedding dimension = 2 and scale_factor (s) = 64, accuracy follows :
![image](https://github.com/abyaadrafid/Aerial-Cactus-Identification/blob/master/arcface.png)

#### DenseNet169
Densenet169 needs more time to converge because of its enormous size and paramters. 

| epoch 	| train_loss 	| valid_loss 	| error_rate 	| accuracy 	|  time 	|
|:-----:	|:----------:	|:----------:	|:----------:	|:--------:	|:-----:	|
|   0   	|  0.059754  	|  0.004154  	|  0.000000  	| 1.000000 	| 01:35 	|
|   1   	|  0.062731  	|  0.000837  	|  0.000000  	| 1.000000 	| 01:29 	|
|   2   	|  0.019187  	|  0.003954  	|  0.000000  	| 1.000000 	| 01:29 	|
|   3   	|  0.009922  	|  0.000457  	|  0.000000  	| 1.000000 	| 01:26 	|
|   4   	|  0.004491  	|  0.000055  	|  0.000000  	| 1.000000 	| 01:27 	|
#### Resnet101
Resnet101 needed less time to converge. 

| epoch 	| train_loss 	| valid_loss 	|  f_beta  	| error_rate 	| accuracy 	|  time 	|
|:-----:	|:----------:	|:----------:	|:--------:	|:----------:	|:--------:	|:-----:	|
|   0   	|  0.063169  	|  0.033260  	| 0.992063 	|  0.011429  	| 0.988571 	| 01:17 	|
|   1   	|  0.034835  	|  0.002770  	| 1.000000 	|  0.000000  	| 1.000000 	| 01:15 	|
|   2   	|  0.024171  	|  0.002123  	| 1.000000 	|  0.000000  	| 1.000000 	| 01:15 	|
|   3   	|  0.014281  	|  0.006416  	| 0.998415 	|  0.005714  	| 0.994286 	| 01:14 	|
|   4   	|  0.006923  	|  0.002465  	| 1.000000 	|  0.000000  	| 1.000000 	| 01:13 	|