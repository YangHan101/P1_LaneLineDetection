# **Finding Lane Lines on the Road** 

**Objective of the Project**

The goal of this project is as following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline includes 6 steps. First of all, I applied canny edge detection on each of the RGB channels. And combined the detected edges in each channel. In this way, I believe yellow and white line can be equally detected. 

After that an area filter was applied on the canny edge image to isolate the region that is most relevant to this problem.

The edge pixels in the region of interest (ROI) are used as input for the Hough line detection. The parameters are set, so that the lanelines are constantly.

Each of the detected lines was extrapolated to identify the crossing points on the boundaries of the ROI. The detected line will only be considered if both ends after extrapolation stay within the range of the ROI. By doing this, the horizontal edges will be generally excluded in the the next step. With this filter and carefully chosen ROI, the remaining line detection results are related to the lane lines.

After the extrapolation, each of the detected line can be parametrized with the crossing coordinates on the top and bottom of the ROI. Using these parameters, we can further classify the lines into two groups, left and right lane line, using kmeans from sklearn package. Using the inverse of the distance of the detected line from the cluster center, the parameters are then averaged for left and right cluster. In some cases a few lines from the road edge and shades would also be included in one or both of the clusters. However they will most likely be further away from the cluster centers, therefore causing less impact on the detection result.

Because I used the endpoints of extrapolated line segments to parametrize the detected lines, I didn't use the draw_line() function. Instead, I called cv2.line directly to draw lane line on the image. 

### 2. Identify potential shortcomings with your current pipeline
In the challenge video, there are a few frames of black tire marks on top of gray cement road. This caused some problem with the detection since the edges of tire marks are passing through the filters I have in place. 
Another possible problem is the assumption that laneline are straight. That would not be true in some of the cases. 
Local road will have writings on the gound like "stop ahead", "slow" or turning arrows. That could get through the filters as well.

### 3. Suggest possible improvements to your pipeline
A possible improvement is to detect the lane lines in two or more sections and then we can combine the detection results in different sections to generate a "curved" lane line.
