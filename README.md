# sign_language_gesture_recognition

## How to navigate the project
-------------------------------

[Jupiter notebook file containing whole learning process](./asl_gesture_recognition.ipynb)

[Mediapipe Holistic landmark dataset for 17 most sampled words](./keys_train)

[Resized and subsampled to 30 frames dataset of images for 17 most sampled words](./img_train)

[Mediapipe Holistic landmark dataset for 95 most basic words](./keys_dataset)

[Resized and subsampled to 10 frames dataset of images for 95 most basic words](./selected_dataset)

Saved models:
- [Original](./best_model.h5)
- [After regularization - I](./best_reg_model.h5)
- [After regularization - II](./best_reg_model_2.h5)

## License

This project is licensed under the Apache License 2.0
You can read more about it [here](./LICENSE)

## Introduction

> “You know, no amount of smiling at a flight of stairs has ever made it turn into a ramp. Never. Smiling at a television screen isn’t going to make closed captions appear for people who are deaf. No amount of standing in the middle of a bookshop and radiating a positive attitude is going to turn all those books into Braille,” ~ [Stella Young](https://epicassist.org/the-biggest-barrier-for-people-with-disability/).

ASL sign in ASL:

![ASL sign in ASL](asl.gif)


Current world is far from perfect and just and many people are forced play it on a **hard mode**, not because of their physique, diversity, disabilities which they are having, but because of the barriers which society doesn't break down to make it more inclusing and accessible for all. Barriers which **can** be destroyed with the current technology.

One of such example is communication barrier for people with hearing impairments when doing errands, visiting offices, etc. when there is noone who knows Sign Language ([Source](https://www.cdc.gov/ncbddd/disabilityandhealth/disability-barriers.html)) . Of course it is always possible for them to write their massages but this style of communication is slower (can be compared to *half duplex* when there is only one side which can send information at the same time) than oral communication (*full duplex*, more nuanced, allowing speakers to exchange information live, simultanously).

![Explaination of half and full duplex](duplex.jpg)

Inspired by Microsoft Inlusive Design we propose a simple solution which could allow for people with hearing problem to communicate more freely with people who don’t know Sign Language.

- [Microsoft - Inclusive design](https://www.microsoft.com/design/inclusive/)
- [Microsoft - Gaming for everyone](https://news.microsoft.com/gamingforeveryone/)

Communication model:

![Communication diagram, two people, one sends sign language to AI model which gives as an output text, other sends voice to AI model which gives as an output text](diag.png)

Recognizing gestures from American Sign Language (Sign Language -> Text) as well as speech recognition (Speech -> text) 

Because there are already [libraries](https://pypi.org/project/SpeechRecognition/) and solutions which deal with speech recognition we will foremost focus on the first part of the communication.

## Dataset

[WLASL (World Level American Sign Language) Video](https://www.kaggle.com/datasets/risangbaskoro/wlasl-processed)
- largest opensource video dataset found by author
- 12 000 processed videos
- 2000 most common words
- "*Most existing sign language datasets are limited to a small number of words. Due to the limited vocabulary size, models learned from those datasets cannot be applied in practice. In this paper, we introduce a new large-scale Word-Level American Sign Language (WLASL) video dataset, containing more than 2000 words performed by over 100 signers.*" ~ [WLASL Scientfic Paper](https://arxiv.org/abs/1910.11006)
- "*66% at top-10 accuracy on 2,000 words/glosses*"  ~ [WLASL Scientfic Paper](https://arxiv.org/abs/1910.11006)
- Very difficult task, relatively new (from 2019)
- Current State-of-the-Art model achieves 58% TOP-1 accuracy 

### Distribution of number of samples 

![histogram distribution](./samples_dist.png)

Mode of number of samples is around 5-6 samples per word. It is very small number when training Neural Network. Because of that there is yet not any good model which achieved high accuracy on full WLASL dataset. 


![Top 1 accuracy on this dataset](top1.png)

## Example

### Original Image 
![original img](./img.png)
### Image with MediaPipe Holistic landmarks
![mediapipe img](./mediapipe.png)

### 4 frames subsampled from video for a word "book"
![4 frames subsampled from video for a word book](./4frames.png)

### How those frames look together
![Word book in ASL](./book.gif)

As we can see for basic signs even few frames are enough to preserve the meaning of the sign.


## Results

### Model after regularization - I

#### Accuracy
![Accuracy](./acc1.png)

#### Loss
![Loss](./loss1.png)

### Model after regularization - II (loaded from previous model)

#### Accuracy
![Accuracy](./acc.png)

#### Loss
![Loss](./loss.png)

## Current progress

- [x] Feature extraction using [MediaPipe Holistic](https://google.github.io/mediapipe/solutions/holistic.html) based on [source](https://www.youtube.com/watch?v=doDUihpj6ro)
- [x] Dataset preprocessing
    - [x] Image resize
    - [x] Subsample video to constant number of frames
- [x] Create dataset of 17 most sampled words from original dataset
- [x] Write Python generator to load dataset in batches
- [x] Train LSTM model on 17 most sampled words with promising accuracy
- [ ] Train CNN model on 17 most sampled words (no GPU power to do so)
- [x] Create dataset of 95 most basic words from original dataset
- [ ] Train LSTM model on 95 most basic words (currently model doesn't have enough samples to learn so many labels)
- [x] Create dataset of 19 letters from original dataset
- [ ] Train LSTM model on 19 letters (currently model doesn't have enough samples to learn so many labels)
- [x] Save best models using Keras Callbacks
- [ ] Gather additional datasets / Generate new dataset

## Credits
Project was **heavily** inspired by **Nicholas Renotte** video on youtube [Sign Language Detection using ACTION RECOGNITION with Python | LSTM Deep Learning Model](https://www.youtube.com/watch?v=doDUihpj6ro), here you can find his code [Github website](https://github.com/nicknochnack/ActionDetectionforSignLanguage).

Functions such as 
 - holistic_adnotation
 - mediapipe_detection
 - draw_landmarks
 - extract_keypoints
 
 were written by Nicholas.
 
Preprocessing as well as model training differs as Nicholas was working on videos which he captured, while author used WLASL dataset.

Author's functions:
 - video_preprocessing
 - video_normalization


[How to make and display gif in Python](https://stackoverflow.com/questions/61110188/how-to-display-a-gif-in-jupyter-notebook-using-google-colab) by [Ying](https://stackoverflow.com/users/6765415/ying)

Other parts might been utilized from libraries documentation thus not explicitly mentioned.

## Tech stack

- Python 3.10.9
- animatplot 0.4.2
- jupiter 1.0.0
- keras 2.11.0
- matplotlib 3.6.2
- mediapipe 0.9.0.1
- numpy 1.23.5
- opencv-contrib-python 4.6.0.66
- pandas 1.5.2
- pip 21.3.1
- scikit-learn 1.2
- tensorflow 2.11.0

