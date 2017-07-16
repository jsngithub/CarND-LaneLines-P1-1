# **Finding Lane Lines on the Road** 

---

### Description of the processing pipeline

The following steps are executed in the pipeline:

1. Convert the image to grayscale
2. Blur the image using the Gaussian blur
3. Perform Canny edge detection
4. Mask out the edges that are not in the region of interest.  Because the camera is mounted at at fixed position in the car the lane lines always tend to show up at a fixed trapezoid area in the bottom half of the image.  So the region of interest was defined to be a trapezoid and the size was determined from the sample images.  The following shows that unmasked v. masked images:
![Unmasked](/test_images_output/1_edges_solidWhiteCurve.jpg)
![Masked](/test_images_output/2_masked_solidWhiteCurve.jpg)

5. The masked image is broken into a left and right half then run through the Hough transform to find the line segments.
6. (part of "hough_lines") Once the line segments are found, lines that are "too horizontal" to be a lane line are filtered out (based on the slope).
7. (part of "hough_lines") Once the bad lines are filtered out, polyfit is used to determine the best fit based on the endpoints of the line segments.  This process outputs the slope(m) and intercept(b) of the line.
8. (part of "hough_lines") using the m, b and how tall the line should be (based on the region of interest trapezoid), the two red lane lines are drawn. 
9. The red lane lines are overlayed on top of the original image using the weighted_img function.


### Potential shortcomings in the pipeline

One potential shortcoming would be what would happen when the camera position changes.  The algorithms in this project are likely overfit to the test images and videos.  For example, the algorithm assumes that the left and the right lanes are on the left and right side of the image (repectively) and that they do not cross over to the other side of the image.  However, it is likely that the camera position is permanently fixed for a given car and is unlikely to change.

Another shortcoming is that the algorithm tries to do a line-fit no matter how many line segments are detected.  In certain instances it may fit and detect a full lane based on a single minimum line segment specified in the Hough algorithm.  This would often result in poor lane detection where the slope and location of the lane may be way off.


### Possible improvements

A possible improvement would be to use the color information for the lines.  Lane lines are either yellow or white, so using this information in the detection process will improve the accuracy.  For example, in the challenge video some black marks were falsely detected as a part of the lane line.

Another possible improvement would be to smooth out the changes in the line position from frame to frame by filtering.

Third possible improvement would be to through out outlier line segments in the linefit algorithm.
