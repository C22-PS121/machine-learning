# Machine Learning Path Repository

This repository used for audio preprocessing and creating model for DANTION. We use android-based audio detection. At the end of the model, we will transform the model to tflite

## Workflow

Data Collecting ➡️ Audio Preprocessing ➡️ Creating Model ➡️ Convert to TF-Lite ➡️ Deploy to Android

## Data Collecting

We gather our data from 70 peoples saying:
- Begal
- Maling
- Rampok
- Pencuri
- Tabrakan
- Kecelakaan
- Kebakaran

After that, we augment our data by overlaying using Traffic Ambience and Rain Ambience. You can access our augmented dataset here: [Indonesian Words Audio Dataset](https://www.kaggle.com/datasets/ahmadulfi/indonesian-words-audio-dataset)


This audio data will be used to detect the event. We also retrieve random audio set from [UrbanSound8K](https://www.kaggle.com/datasets/chrisfilo/urbansound8k?select=fold10).

## Creating Model
We tried some hyperparameters in creating the model. These are some of the result.
 
*__________ 45 Epochs Attempt*

![Bad Acc Graph](https://github.com/C22-PS121/machine-learning/blob/main/saved-graph/Bad%20Acc%20Graph.png) ![Good Acc Graph](https://github.com/C22-PS121/machine-learning/blob/main/saved-graph/Bad%20Loss%20Graph.png)

Notice that the line is indicating overfitting. So that we adjust the hyperparameters and reduce the epochs.

*__________ 35 Epochs Attempt*

![35 Epoch Accuracy Graph](https://github.com/C22-PS121/machine-learning/blob/main/saved-graph/35epochs%20Acc%20Graph.png) ![35 Epoch Loss Graph](https://github.com/C22-PS121/machine-learning/blob/main/saved-graph/35epochs%20Loss%20Graph.png)
      
If we take look closely, the loss and accuracy is still not as smooth as we expected. The graphs starting to split up as the epoch getting bigger. So, we try to re-adjust the hyperparameters at the model and reduce the epochs.

*__________ 20 Epochs Attempt*

![Better Accuracy Graph](https://github.com/C22-PS121/machine-learning/blob/main/saved-graph/Better%20Acc%20Graph.png) ![Better Loss Graph](https://github.com/C22-PS121/machine-learning/blob/main/saved-graph/Better%20Loss%20Graph.png)

After many attemp adjust the hyperparameters and adjusting the epochs size, finally we got the best result we can outcome.

```
_________________________________________________________________
 Layer (type)                Output Shape              Param #   
=================================================================
 conv2d_63 (Conv2D)          (None, 124, 129, 32)      320       
                                                                 
 max_pooling2d_62 (MaxPoolin  (None, 62, 65, 32)       0         
 g2D)                                                            
                                                                 
 conv2d_64 (Conv2D)          (None, 62, 65, 64)        18496     
                                                                 
 max_pooling2d_63 (MaxPoolin  (None, 31, 33, 64)       0         
 g2D)                                                            
                                                                 
 conv2d_65 (Conv2D)          (None, 31, 33, 128)       73856     
                                                                 
 max_pooling2d_64 (MaxPoolin  (None, 16, 17, 128)      0         
 g2D)                                                            
                                                                 
 dropout_61 (Dropout)        (None, 16, 17, 128)       0         
                                                                 
 conv2d_66 (Conv2D)          (None, 16, 17, 256)       295168    
                                                                 
 max_pooling2d_65 (MaxPoolin  (None, 8, 9, 256)        0         
 g2D)                                                            
                                                                 
 dropout_62 (Dropout)        (None, 8, 9, 256)         0         
                                                                 
 flatten_16 (Flatten)        (None, 18432)             0         
                                                                 
 dropout_63 (Dropout)        (None, 18432)             0         
                                                                 
 dense_32 (Dense)            (None, 64)                1179712   
                                                                 
 dropout_64 (Dropout)        (None, 64)                0         
                                                                 
 dense_33 (Dense)            (None, 8)                 520       
                                                                 
=================================================================
Total params: 1,568,072
Trainable params: 1,568,072
Non-trainable params: 0
_________________________________________________________________
```

## References
Tensorflow: [Simple Audio Recognition](https://www.tensorflow.org/tutorials/audio/simple_audio)