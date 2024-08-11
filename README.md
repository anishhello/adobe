# adobe

## For the regularization part of the PS we have implemented a pipeline as follows:


*step 1- clustering of the images using DBSCAN to distinguish possibly different figures and then storing the polyline sets of these figures 

Now for each figure we carry out these steps

*step 2 - we try to fit each figure with the list of regular shapes we have . On a successful fit we extract the polyline information of the image and then pass it onto the symmetry analysis part

*step 3- In case the figure does not fit onto any given shapes then we assume that either the figure itself contains several segments which are ordered randomly . So we need a optimal method to decompose the whole figure data into further segments. 

*step 4-  For the optimal reordering of the segments we are using the concept of the well known problem of TRAVELLING SALESMAN PROBLEM . We have used the analogy of the distance as the difference of curvatures of the edges of segments to be reordered one after one another. Using this algorithm we reorder the segments in such a way that the further curvature-based segmentations yield least number of distinct segments and yield the optimal smooth segments.

*step 5- After this we repeat  step 2  for each new segment obtained by the curvature based segmentation of the optimal reordering extracted after step 4

## For the occlusion part we have adopted the pipeline as follows:

1. Bezier Curve Fitting (fit_bezier_curve Function):
   
Purpose: This function fits a cubic Bézier curve to a given set of points. A cubic Bézier curve is defined by four control points, and the function optimizes these control points to best fit the curve to the given data points.

Steps:
Cost Function: Defines a cost function that calculates the difference between the given points and the points on the Bézier curve for a set of control points.

Optimization: Uses the minimize function from scipy.optimize to adjust the control points such that the Bézier curve closely matches the given points.

Output: Returns the optimized control points of the Bézier curve.

3. Bezier Curve Generation (bezier_curve Function):
   
Purpose: Generates points on a Bézier curve given a set of control points and a parameter t that runs from 0 to 1.

Steps:

Cubic Bézier Formula: Uses the standard cubic Bézier formula to compute the position of the curve at each value of t.

Output: Returns the coordinates of the points on the curve.

5. Reflecting Points (reflect_points Function):
   
Purpose: Reflects a set of points across a given symmetry axis.

Steps:

Vector Calculation: Computes the vector from each point to the axis.

Projection: Projects the vector onto the axis direction to find the mirrored point.

Reflection: Calculates the reflected point by subtracting the vector from twice the projection.

Output: Returns the reflected points.

7. Completing the Border Using Bézier Curves (complete_border_bezier Function):
   
Purpose: This function completes the border of an object by connecting two segments with a smooth Bézier curve.

Steps:

Fitting Curves: Fits Bézier curves to both segments.

Connecting Curves: Creates a connecting curve (linear interpolation) between the end of the first segment and the start of the second.

Merging: Combines the Bézier curves and the connecting line to form a continuous completed border.

Output: Returns the x and y coordinates of the completed border.

9. Completing the Border Using Symmetry (complete_border_symmetry Function):
    
Purpose: Completes the border by reflecting a segment across a symmetry axis.

Steps:

Reflection: Reflects the second segment across the specified symmetry axis.

Fitting Curves: Fits Bézier curves to both the original segment and its reflected counterpart.

Connecting Curves: Creates a connecting curve between the end of the first segment and the start of the reflected segment.

Merging: Combines the Bézier curves and the connecting line to form the completed symmetric border.

Output: Returns the x and y coordinates of the completed border using symmetry.

11. Smooth Border Completion (smooth_border_completion Function):
    
Purpose: Completes the border by optimizing for smoothness, ensuring that the transition between segments is smooth.

Steps:

Objective Function: Defines an objective function that penalizes high curvature (sharp bends) in the completed curve.

Optimization: Uses the minimize function to adjust the control points of a Bézier curve to minimize the curvature.

Generating Curves: Generates points on the optimized Bézier curve.

Output: Returns the smooth Bézier curves.
