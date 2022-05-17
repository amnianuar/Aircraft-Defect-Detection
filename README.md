# Aircraft Defect Detection using Azure Custom Vision

This project explains how to use Microsoft Custom Vision AI to detect defects in the images by object detection and Microsoft PowerApps.

## Azure Portal
1. Create Cognitive Services and Custom Vision resources in Azure portal to be used. Custom Vision training and prediction resources created to train the images from dataset given and predict whether it has defects of scratch and dent. 

<img width="1461" alt="Screenshot 2022-05-17 at 11 33 29" src="https://user-images.githubusercontent.com/87117107/168723220-d2a9b3ea-2704-4143-9cc4-2ce7c24ccba7.png">

## Custom Vision Portal
1. Upload images to be trained and predict in https://www.customvision.ai/.
2. Create project with Object Detection and using Domains of General[A1]. General A1 chosen to train the images as it can detect the scratch unlike Domain General. General A1 usually performed for difficult use cases.

<img width="599" alt="create obj detect A1" src="https://user-images.githubusercontent.com/87117107/168724960-9e6c7ac3-c41f-4004-9853-16625dd699be.png">

3. Tag the images with label of scratch and dent identifying all of them. Tag labels must be minimum of 15 label to be able to train. It provide a nice UI to navigate with and easily label them. Training done without doing data pre-processing for the images as it is of importance to identify the defects in any kind of light settings. A quick test can be performed before publishing the model for prediction.
4. Even the smallest dent and scratch can be seen when performing Quick Test after training. 

<img width="1475" alt="smallest scratch label" src="https://user-images.githubusercontent.com/87117107/168724596-899fbf1c-1722-40b9-adad-2339de7154cb.png">


<img width="1223" alt="Screenshot 2022-05-17 at 11 46 54" src="https://user-images.githubusercontent.com/87117107/168724622-df91b30a-704f-4a6c-ae57-e496ca28d38d.png">

5. Training performed multiple iterations and iteration 8 chosen for predicting the unseen images. With 54 images trained, it have the ability to detect scratch and dent. Iteration 6 had 52 images trained and with just an increase of 2 images to iteration 8 of 54 images, it was able to improve the accuracy of detecting scratch defect from Recall of 16.7% to 41.2%. Significant improvement of precision, recall and mean average precision was seen.

<img width="1179" alt="Screenshot 2022-05-17 at 12 04 50" src="https://user-images.githubusercontent.com/87117107/168726474-f6bba26e-852b-45a0-9446-b4be89883c44.png">


<img width="1212" alt="Screenshot 2022-05-17 at 12 02 55" src="https://user-images.githubusercontent.com/87117107/168726275-d7695455-04c5-4b32-afcf-6872bcbb2a43.png">

6. For this project, 72 images dataset was provided. A set of 30, 52, 54 and 62 images were trained and overall good result was obtained with training of 54 images. The 62 images result gave inaccurate prediction of scratch and dent. This is due to the unbalanced tags of the dent and scratch for training since the scratch have been tagged lesser than scratch. Nevertheless, with small set of images trained, it can still predict the scratch and dent which would be hard to be performed without Custom Vision.
7. There is defect of irregular colours but due to insufficient example of images having the defect make it unable to be trained as the tag labels must at least be 15 to be trained using Custom Vision. Only defects of scratch and dent was trained and being predicted in this project.

## Custom Vision API and SDK with Python language
In the Custom Vision portal, training images is uploaded manually and automatic training can be done by using API. Python language is being used for the training API. This training images will automatically send to Custom Vision portal and iteration will start. The code can be obtained from `train-detector.py` from folder 

1. **Microsoft Visual Object Tagging Tool (VoTT)** is used to tag the dent and scratch on the images. It has the ability exporting to json file as this will be used to identify the bounding box of the coordinates of each tag. Since in the portal is done using the cursor and it has the capability of tagging the defect and trained from the labeled bounding box automatically but if done manually, annotation of the images must be done by ourselves to find the exact location of defect and scratch. 
2. The data of the training images is saved in Azure Blob Storage to be tagged on VoTT. It's very secure and protect the data privacy as SAS token and enable CORS should be done before connecting to the blob storage.

<img width="1271" alt="Screenshot 2022-05-16 at 15 03 20" src="https://user-images.githubusercontent.com/87117107/168729220-dda3a612-c506-489f-9555-b81e3d3ecf67.png">



