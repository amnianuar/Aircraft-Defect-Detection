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

6. For this project, 72 images dataset was provided. 62 images was used for training and 10 are set aside for testing. A set of 30, 52, 54 and 62 images were trained and overall good result was obtained with training of 54 images. The 62 images result gave inaccurate prediction of scratch and dent. This is due to the unbalanced tags of the dent and scratch for training since the scratch have been tagged lesser than scratch. Nevertheless, with small set of images trained, it can still predict the scratch and dent which would be hard to be performed without Custom Vision.
7. There is defect of irregular colours but due to insufficient example of images having the defect make it unable to be trained as the tag labels must at least be 15 to be trained using Custom Vision. Only defects of scratch and dent was trained and being predicted in this project.

## Custom Vision API and SDK with Python language
In the Custom Vision portal, training images is uploaded manually and automatic training can be done by using API. Python language is being used for the training API. This training images will automatically send to Custom Vision portal and iteration will start. The code can be obtained from `train-detector.py'.

1. **Microsoft Visual Object Tagging Tool (VoTT)** is used to tag the dent and scratch on the images. It has the ability exporting to json file as this will be used to identify the bounding box of the coordinates of each tag. Since in the portal is done using the cursor and it has the capability of tagging the defect and trained from the labeled bounding box automatically but if done manually, annotation of the images must be done by ourselves to find the exact location of defect and scratch. 

3. The data of the training images is saved in Azure Blob Storage to be tagged on VoTT. It's very secure and protect the data privacy as SAS token and enable CORS should be done before connecting to the blob storage. After finished tagging, export to json file was done named `aircraft-defects-export.json` to Azure Blob storage.

<img width="1271" alt="Screenshot 2022-05-16 at 15 03 20" src="https://user-images.githubusercontent.com/87117107/168729220-dda3a612-c506-489f-9555-b81e3d3ecf67.png">

3. The exported json file of bounding coordinates was not normalized hence was calculated manually and put into `tagged-images.json`. All the images will automatically exchange data using traning API to Custom Vision portal.

4.  When bounding box is not correctly tagged at the correct location of the defect, it can be adjusted in the Custom Vision portal on the Training Images section. Additional defect detected can be tagged as well.

<img width="1323" alt="JSON file normalized not so accurate" src="https://user-images.githubusercontent.com/87117107/168733128-6933a972-8c6c-4957-9d02-cd0131a18f7f.png">


<img width="1481" alt="Screenshot 2022-05-15 at 03 50 19" src="https://user-images.githubusercontent.com/87117107/168734544-347dfc62-5f46-4ef6-a2a4-fab6a1adeab8.png">

5. After finished training images using API, the new iteration is evaluated to identify which iterations have the highest accuracy for predicting the defects and correctly tagging them. After this, the iteration of training the model is published for prediction to be done for the unseen images.
6. The 10 images from dataset for testing that has been set aside will be predicted its image of having defects or not
7. A client terminal app is used to predict the images by using Python SDK and to plot the tagged exact region of dent and scratches. The code can be obtained from `test-detector.py`. 
8. The result showed the exact location of dent and scratch which could not be visible by the human eyes. This shows that Custom Vision have made it possible to detect the defects in real time and the model was well trained even with 54 images. The accuracy of the predicion is even up to above 90% for many defects in the image. 
9. Defects of higher probability than 50% are displayed to avoid false positives.

![output-defect-2699](https://user-images.githubusercontent.com/87117107/168736455-aea81bfa-273d-4196-9b67-898d85a06af1.jpg)

![output-defect-2637](https://user-images.githubusercontent.com/87117107/168736566-62f7e1b4-0b71-4ef9-807d-f23626612fac.jpg)
![output-defect-2638](https://user-images.githubusercontent.com/87117107/168736573-5005b233-67b0-4eca-9edf-bc08e2d55c52.jpg)
![output-defect-2640](https://user-images.githubusercontent.com/87117107/168736577-4dd88f76-4df7-49e9-ace7-56dd8822b532.jpg)
![output-defect-2697](https://user-images.githubusercontent.com/87117107/168736580-93871c22-f65f-4d09-96e1-beb899f50af4.jpg)
![output-defect-2698](https://user-images.githubusercontent.com/87117107/168736582-c5928398-0b79-47ce-a56b-1962d821d550.jpg)
![output-defect-2701](https://user-images.githubusercontent.com/87117107/168736584-95ddecdd-471d-41f6-a72f-23a54bbe71d9.jpg)
![output-defect-2703](https://user-images.githubusercontent.com/87117107/168736587-efdbe4b8-1341-4d88-93b5-4061f944af30.jpg)
![output-defect-2705](https://user-images.githubusercontent.com/87117107/168736589-bd26a0dc-37a9-49d6-8186-3993c22c9c8d.jpg)
![output-defect-2627](https://user-images.githubusercontent.com/87117107/168736824-89b40c5d-9e03-4ec4-bfb4-87bd95bf0026.jpg)

10. Since all images dataset provided have defects, image of a clean metal surface from the internet were used to test the prediction. This is due to some of the images of aircraft parts seem to be of metal or steel surfaces. It could be seen the first output have no defect at all and the second output have detect scratches. Even at this point, the model could detect scratch. The training of the model could be better by having more training images in different lighting conditions and different angles to produce better defect detection later.

![output-defect-TestCleanSurface-2](https://user-images.githubusercontent.com/87117107/168737678-6884a614-0412-4f64-aa2f-b1b24a3458d1.jpg)
![output-defect-TestCleanSurface](https://user-images.githubusercontent.com/87117107/168737718-c5a5810d-a085-45a8-bc32-e89e92fed30d.jpg)

11. The output defects exact coordinates was generated from the terminal client app using the prediction python SDK. 
<img width="948" alt="Screenshot 2022-05-15 at 16 58 52" src="https://user-images.githubusercontent.com/87117107/168740277-8bf56d92-4761-46fc-a7cd-d8dd2f0482d5.png">


## Microsoft Power Apps 

Since the client terminal app does not have frontend UI to navigate with, another way can be done to build apps by using Power Apps. Power Apps will be connected to the API prediction resources from Custom Vision services and will be deployed to cloud. For this project, client mobile app is built by connecting to the API and deployed to cloud.

1. Power Apps provide connection to the Custom Vision services by using data to add connectors and set the connection using the prediction key and URL which can be obtained from Custom Vision portal. 

<img width="785" alt="Screenshot 2022-05-16 at 00 08 55" src="https://user-images.githubusercontent.com/87117107/168741407-3cf8a26b-fd95-4a93-a732-3964ae1689dc.png">
<img width="338" alt="Screenshot 2022-05-16 at 00 10 38" src="https://user-images.githubusercontent.com/87117107/168741440-83e87639-9993-4c3e-8df0-6bf1187a4704.png">

2. The design of the apps are as follow:
<img width="495" alt="Screenshot 2022-05-17 at 14 18 49" src="https://user-images.githubusercontent.com/87117107/168742281-22a89b8d-a125-4abd-9100-678c49bd967b.png">

<img width="497" alt="Screenshot 2022-05-17 at 14 19 00" src="https://user-images.githubusercontent.com/87117107/168742308-a20748c3-1cec-4bde-a599-730f62b27cb4.png">

3. The **button Detect Defects** is the most important where it will connect to the prediction resources to detect whether there is defect when the image is uploaded. The following **code** should be specify in the button:

```javascript  
ClearCollect(gallerycol,CustomVision.DetectImageV2("Your Custom Vision AI project ID","Your Iteration",UploadedImage1).predictions)
```
<img width="1323" alt="Screenshot 2022-05-17 at 14 30 52" src="https://user-images.githubusercontent.com/87117107/168744109-75f57cfc-b3d7-488f-b327-350044b1b398.png">







