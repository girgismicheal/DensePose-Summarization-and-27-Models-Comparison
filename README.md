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

