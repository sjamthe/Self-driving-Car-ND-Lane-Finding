# Self-driving-Car-ND-Lane-Finding

Here are the tricks & tips used in the code to detect and smooth the lines

<UL>
<LI> Mask only the bottom half of the image first.
<LI> If you grayscale the image sometimes yellow lines disappear if the road is too bright/reflective. 
To avoid that I use cv2.inRange and create a mask that only shows yellow colors. The add this yellow image back to original image
thus doubling the strength of yellow color. They apply greyscale, gaussian blur and canny transform to find edges.
<LI> The canny transform shows a lot of edges at the top so I apply another mask to remove a small 10 points strip at the top edge.
<LI> Get houghlines with rho = 1, theta = np.pi/180 , threshold = 40 , min_line_len = 100, max_line_gap = 160 
<LI> As houghlines returns a lot of lines that may not be lane lines, I have a lane_lines function to filter out all the junk. Here is the filter criteria
<OL>
<LI> We don't expect lane lines to be absolutely vertical as we are looking at the horizon. so remove lines where x1=x2
<LI> For the right lane we expect slope > 0.5, we also expect the topmost part of the right lane never cross 0.3*image width (limit lower slopes again - this may be redundant if we do the math right, also check that the top intercept of right lane is on the left half of image - another trial an error number which may vary based on camera angle!)
<LI> The bottom of the lane should be between 70% and 100% of the imagewidth - again dependent on the camera mount position, but fair assumption I guess.
<LI> For left lane the slope should be <-0.5 but the intercept and the bottom of the lane should be inside the image!.
<LI> The top of the lane should be no more than 70% of the image. This again may be similar to slope <-0.5 if we know image size 
</OL>
</UL>
