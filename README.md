# Self-driving-Car-ND-Lane-Finding

Here are the tricks & tips used in the code to detect and smooth the lines

<UL>
<LI> Mask only the bottom half of the image first.
<LI> If you grayscale the image sometimes yellow lines disappear if the road is too bright/reflective. 
To avoid that I use cv2.inRange and create a mask that only shows yellow colors. The add this yellow image back to original image
thus doubling the strength of yellow color. They apply greyscale, gaussian blur and canny transform to find edges.
<LI> The canny transform shows a lot of edges at the top so I apply another mask to remove a small 10 points strip at the top edge.
<LI> Get houghlines with rho = 1, theta = np.pi/180 , threshold = 40 , min_line_len = 100, max_line_gap = 160 
<LI> As houghlines returns a lot of lines that may not be lane lines, I have a lane_lines function to filter out all the junk.
</UL>
