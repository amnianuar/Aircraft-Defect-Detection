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

5. Training performed multiple iterations and 

