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
<LI> For left lane the slope should be &lt; -0.5 but the intercept and the bottom of the lane should be inside the image!.
<LI> The top of the left lane should be no more than 70% of the image. This again may be similar to slope  &lt; -0.5 if we know image size.
</OL>
<LI> For all the houghlines we selected above we extend them to the top of the image and bottom of the image. We store these two x values along with how high the line is and how close to the car the line is.
<LI> All these lines are stored (seperately for left and right lanes) for last n images (n = 10). We don't draw lanes based on only one image but last 'n' images. This way we make sure that images that have missing lanes also have lane marking.
<LI> For all the lines found for each lane in last n images we find median and std for the bottom and top intercept for each lane.
<LI><B>Ignore lines that are too far from median</B> If the std/median is > 0.15 and the line's bottom intercept is further than std from median we ignore that line. 
<LI>For all the lines that were selected we do a weighted average for the top and bottom intercept. The weights are how tall the line is and how close to the bottom it is. <b>This finally gives us the lanes </b>
<LI> Sometimes some strong lines on the road (or the edge of divider) may look like a lane. So we do a final check. We know car cannot jump abruptly so we compare the new lane with previous lane. If the left bottom of the lane has jumped for 100 points or more we choose the prev lane and ignore the new lane!

</UL>
