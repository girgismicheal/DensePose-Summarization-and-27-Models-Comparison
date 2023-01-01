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

