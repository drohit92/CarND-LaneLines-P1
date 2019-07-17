# **Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/poi.png   

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps: 

    1. Convert image from RGB to gray. Used helper function grayscale
    2. Applied some gaussian smoothing. Used helper function gaussian_blur
    3. Run canny edge detection on this smoothened image. Used helper function canny
    4. I noticed a pattern of how lines showed up right about the center of the image. So added some logic to crop the region of interest      
    5. Draw hough lines on this roi image. Used helper function hough_lines which internally used draw_lines

In order to draw the left and right lanes, I added some logic to draw_lines() in order to:
    
    For each image:
    1. Find all poosible hough lines using  cv2.HoughLinesP 
    2. Then find slopes of all these possible hough lines.
    3. Ignore some of the lines which were either too veritical or too horizontal and look at them only if they were in a comfortable range.
    4. While looping through all these hough lines that match our criteria, I was collecting these points in separate arrays left_line_points and right_line_points
    5. For both the left and right points, I then use cv2.fitLine to get the equation of the left and right params
    6. Then using more coordinate geometry I found the point of intersection (POI).
    7. With the aim to draw lines and extrapolate them to point where they almost meet, I decided to find the POI and extrapolate my lines only a little below the POI.
    
![alt text][image1]

### 2. Identify potential shortcomings with your current pipeline


Potential shortcomings:
1. At the very basic, the params are not well tuned. In a few frames you will some lines will be much horizontal or vertical.
2. Currently the video does not seem very smooth. I am thinking that in a production env, frames over time are taken into consideration. As an example, lines in frame at (t+1)sec cannot be very very far or different from lines in frame at tsec. Since I process frames independently I currently do that guarantee that.
3. Seem to be doing a very bad job at curves.

### 3. Suggest possible improvements to your pipeline

Possible improvements:
1. Experiment more and have tighter param values
2. Taken multiple frames into account and try to smoothen it over time. Not sure how though :)