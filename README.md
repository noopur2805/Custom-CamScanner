

Custom CamScanner
=====================

## Steps:

	1. Obtain page boundaries:
		For this, the page was simply cropped by a few pixela at the border.
		
	2. Detect text contours:
		Detect horizontal lines of text. First, identify their edges, then morphological operations like dilation 
		and erosion helped in connecting those adjacent text, thus providing the horizontal alignment of text. Each 
		remaining text contour is then approximated by its best-fitting line segment using PCA.
		
	3. Convert text into spans and assemble spans:
		Converting into span means to obtain how much page/text warp is onto how many further pixels on same line 
		of text.
		Basically, we generate candidate edges for every pair of text contours, and score them. The resulting 
		cost is infinite if the two contours overlap significantly along their lengths, if they are too far apart, 
		or if they diverge too much in angle. Otherwise, the score is a linear combination of distance and change 
		in angle. Once the connections are made, the contours can be easily grouped into spans.
		
	4. Page dewarp operation:
		Create naïve parameter estimate. I use PCA to estimate the mean orientation of all spans. The resulting
		principal components are used to analytically establish the initial guess of the x and y coordinates, 
		along with the pose of a flat, curvature-free page using "cv2.solvePnP". The reprojection of the keypoints 
		will be accomplished by sampling the cubic spline to obtain the z-offsets of the object points and calling 
		"cv2.projectPoints". to project into the image plane.
		This tuens the perspective of the page to top-down.


## Execution Command:

On command prompt:

`python process_image.py <image_file>`

