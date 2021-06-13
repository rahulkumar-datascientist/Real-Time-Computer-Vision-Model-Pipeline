# Real Time Computer Vision Pipeline for event detection and classification

The Project aims to implement a computer vision model pipeline and perform the selected query to identify
events from a given video stream. The Figure below shows the high-level block diagram architecture
of the pipeline. It consists of a video reader and a model cascade. The task is to
detect object events (car) with a specific attribute (car type).

<p align="center">
  <img src="Readme%20Image/Model_Cascade.PNG" width=500>
</p>


The Model Cascade is a 2 stage operation:
- **STAGE 1**: Deployed a SOTA Object Detector model (TinyYOLOv3) pre-trained on MSCOCO dataset. The model and weights are downloaded from : https://github.com/qqwweee/keras-yolo3   (or) https://pjreddie.com/darknet/yolo/
- **STAGE 2**: Using a pre-trained MobileNet model and performing Transfer Learning to create the model to detect the car type (SUV or Sedan) attribute. 

The Throughput of the pipeline is calculated on 2 type of queries:
1. Detecting the object. i.e., only the STAGE 1 of the pipeline is executed (skipping STAGE 2)
2. Detecting the object and passing it through a CNN model(Retrained MobileNet model) to determine the car type attribute. Here the CV pipeline exectues both the STAGE 1 and then STAGE 2.

## Below are the steps for creating the system

1. **Environment_setup.ipynb** - Here I download the DarkNet model and TinyYOLOv3 weights. The data is saved in "DarkNet model" folder.
2. **image_scrapping.ipynb** - Here I use Google and Bing Image crawler to download images of SUV and Sedan cars. I will use this set of images to retrain the MobileNet model.
3. **MobileNet Training.ipynb** - Here I retrain the MobileNet model using Transfer learning approach. Firstly i freezed the top layer and only trained the new Dense layer. Then I finetune the model by training all the parameters of the MobileNet model and saving it for using it during the pipeline execution.
4. **Optimised Computer Vision Pipeline v1.6.ipynb** - Used Producer – Consumer approach in designing the above pipeline where, the Video Reader is implemented in a 
different thread which produces the frame and places it in a queue. The model cascade is implemented in another 
thread which consumes the frame from the queue and perform either only object detection (Query-1) or runs the 
whole model cascade which gives the car’s attribute (Query - 2) depending on the choice selected by the user. The 
producer - consumer model is implemented using multi-threading to help the pipeline run more efficiently by running 
the threads in parallel and enabling the communication between them using a queue which transfers the frames from 
1 end to another.
5. The output of the pipeline (Query 1 and 2) is saved and merged in 1 final file for evaluation - **optimised result.xlsx**
6. The final video(Running the complete pipeline) is saved as **Output.mp4**
