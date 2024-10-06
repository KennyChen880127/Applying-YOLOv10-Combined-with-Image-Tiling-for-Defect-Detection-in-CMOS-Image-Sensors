<div align="center">
<h1>
<b>
Applying YOLOv10 Combined with Image Tiling for Defect Detection in CMOS Image Sensors
</b>
</h1>
</div>

To identify surface defects on CMOS image sensors using industrial cameras, this study addresses the challenges posed by high-resolution industrial camera images. To reduce the loss of image features caused by resizing, we utilized the state-of-the-art object detection algorithm, YOLOv10. We compared the commonly used image scaling method (which conserves training and inference resources) with our proposed method of combining YOLOv10 with image tiling. Both methods were trained and deployed on Jetson Orin NX 8GB for inference comparison. 
This improvement significantly enhanced the training results, with precision, recall, and mAP@0.5 increasing from 0.79, 0.68, and 0.75 to 0.87, 0.82, and 0.88, respectively. The inference results also showed an increase in mAP@0.5 from 0.75 to 0.88, demonstrating that the algorithm can accurately detect small-scale objects in high-resolution images.

## YOLOv10
[YOLOv10](https://github.com/THU-MIG/yolov10), built on the [Ultralytics Python package](https://pypi.org/project/ultralytics/) by researchers at [Tsinghua University](https://www.tsinghua.edu.cn/en/), introduces a new approach to real-time object detection, addressing both the post-processing and model architecture deficiencies found in previous YOLO versions. By eliminating non-maximum suppression (NMS) and optimizing various model components, YOLOv10 achieves state-of-the-art performance with significantly reduced computational overhead. Extensive experiments demonstrate its superior accuracy-latency trade-offs across multiple model scales.

## Image Tiling
Image Tiling is a technique used to improve the detection of small objects in high-resolution images by dividing the original image into smaller tiles. This approach allows for the detection of fine details that might otherwise be missed when processing the entire image at once, particularly when the image is resized to fit the input size of the detection model. Each tile is processed independently through the object detection model, and the results are then merged to provide comprehensive detection over the entire image. This method not only preserves the resolution of critical features but also improves the accuracy of detecting small objects while maintaining computational efficiency. Image Tiling is especially useful when dealing with large images, such as those captured by industrial cameras, where small defects or objects need to be identified with precision.

### Features
* When creating the dataset, a randomly annotated object is selected, and the image is cropped outward from its center to form a 640×640 pixel image, or manually adjusted to other sizes as needed. If other annotated objects are not within the cropped image, one of the remaining annotated objects will be used as the center, and the process is repeated. If the extended area exceeds the original image size, the extra space is filled with black pixels, as shown in the illustration.
* When dividing the input image into multiple tiles, formulas (1) and (2) can be used to determine the number of horizontal and vertical tiles.Where W and H represent the width and height of the input image, respectively, and step is the distance moved during each tiling step. This step distance is calculated as the tile size minus the overlap area. The overall inference process is shown in Figure 6.

        tiles_x = W/step......(1)
        tiles_y = H/step......(2)
  

### Results
| Yellow dots indicate correction values. | Purple dots indicate correction values |
| ------------- | ------------- |
| ![image](https://github.com/KennyChen880127/YOLOv8-Improved-Pose-Estimation/blob/main/example_1.jpg) | ![image](https://github.com/KennyChen880127/YOLOv8-Improved-Pose-Estimation/blob/main/example_2.jpg) |
