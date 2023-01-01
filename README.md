# DensePose-Summarization-and-27-Models-Comparison

## Paper Summary [1]
Dense pose estimation aims at mapping all human pixels of an RGB image to the 3D surface of the human body.

![](/Image/Screenshot_1.png)

- The authors introduced Densepose-COCO data which is collected images of all the people in the COCO dataset and created an efficient annotation pipeline that map 2D image to its 3D correspondences.
- The Densepose generated mapping is used as a ground truth for training the models.
- They measure the error using human annotation with respect to the ground truth to identify the hardest parts of the human body to annotate.

- The evaluation consists of 2 metrics:
  - Pointwise evaluation
  - Per-instance evaluation
![](/Image/Screenshot_2.png)

- They used the SMPL model that represent the human 3D model body where the mapped 
supervised points that point to where the image pixels are located on the human model.

- They divided the human body into 24 segments, and each segment was divided into 14 points.


### **Methodology:**

1. **Fully Connected DensePose Regression**

They used the regional proposal instead of a fully convolutional dense pose regression without the region proposals a single network needs to learn to perform multiple tasks which are detection, classification, regression, and segmentation, which are too many tasks for one network and lead to a performance degradation.

The FCN task are:
- Detection: detect the objects in the image.
- Classification: 24 parts + 1 part for background.
- Regression: where exactly the point in the model representation (UV coordinate).
- Segmentation: Human Part segments.


2. **Region-based Dense Pose Regression**

This approach is built upon the region proposal methodology which divides the image into regions and then passes each region through a ROIAlign to convert it to a fixed size feature vector, and each feature vector passed through two sequential part:
- The classification part: which classifies each pixel into one of 25 classes (24 human body parts + 1 for background).
- The regression part: after identifying the pixel class then we predict its UV coordinate on the human 3D model.

![](/Image/Screenshot_3.png)

3. **Multi-task cascaded architectures**

The cross-cascading architecture used the output of the RoIAlign module in the previous approach and fed it into the DensePose network as well as auxiliary networks for other tasks such as masks, and key points. Once first-stage predictions are obtained from all tasks, they are combined and then fed into a second-stage refinement unit of each branch.

This approach consists of 2 branches:
- GT masks and key-points which is taken from the mask R-CNN architecture.
- GT DensePose which is the same as we explained previously.

![](/Image/Screenshot_4.png)

4. **Distillation-based ground-truth interpolation using teacher network**
In this approach, they additionally used a teacher network to generate a supervision signal to be used as ground truth for points that didn't have ground truth, this dense supervision signal is used for region-based training.

![](/Image/Screenshot_5.png)
 
### **Results of the paper:**

**Human's 3D models Comparisons:**
The paper's 3D model has outperformed the previously known models as shown in the below graph.

![](/Image/Screenshot_6.png)


**Dataset's Comparisons:**

The paper compared its dataset with other datasets in the densepose field, to emphasize the quality of their dataset.

As most of the other datasets are semi-machine annotated, as per the shown graphs below when trying different experiments they found the quality of the model dropped when they added machine-annotated datasets, and the best performance is achieved by their dataset.

![](/Image/Screenshot_7.png)

**Approaches Comparisons**

By comparing the different approaches: 
- The FCN has the least performance.
- The region proposal approach the performance increased significantly.
- The distillation approach besides its usefulness in the interpolation, also has a great performance improvement.
- Finally, adding the keypoint branch helps to improve the overall pipeline performance.

![](/Image/Screenshot_8.png)


## OurDataset
We scrapped more than 5000 person images with clothes using "" tool to scrap datasets from different clothes websites such as Zara, Zalando, and Farfech.
Our dataset was used later for the model's performance evaluation on our problem and trained other models to be more accurate and fit the business requirements.

## Methodology
We scrapped a dataset of 5000 images from diverse websites, then we design our evaluation set and performance metric of each model for the dense pose we evaluate the model's part index by the human expert eye due to our problem limitations.
Then we tried different state-of-the-art models in this area and the best project per the writing time was "detectron 2" which was developed by Facebook, they trained 27 models and we tried all of them to evaluate their performance and suitability on our problem.
![](/Image/Screenshot_9.png)
The evaluation process takes around 2 days to be completed on our GPU as we selected 300 image only out of 5000 as the inference time of the models take around 3 to 5 seconds per image and we have (27 models*300 images) = 8100 images so it takes a little bit long time.

## Experiment
I have designed a dataset to cover a diverse dataset distribution as follows:
- 100 images from the HR-VITON dataset
- 60 Zalando (males)
- 20 Zara (males)
- 20 Farech (males)
- 20 Farech (females)

The above set of data just to validate the stability of the models over different distribution.

We tried 27 densepose models from detectron 2 project, and the models mainly divided into n families as follows:
- 
- 

## Result

https://drive.google.com/drive/folders/1b99Z1U0DFxgHCRWzdpKr7_ZUjMSr4feh?usp=sharing



# References:
[1] Güler, R. A., Neverova, N., &amp; Kokkinos, I. (2018, February 1). DensePose: Dense human pose estimation in the wild. arXiv.org. Retrieved December 31, 2022, from https://arxiv.org/abs/1802.00434v1 
