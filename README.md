Download link :https://programming.engineering/product/from-3d-reconstruction-to-recognition-homework-2/


# From-3D-Reconstruction-to-Recognition-Homework-2
From 3D Reconstruction to Recognition Homework #2
Fundamental Matrix Estimation From Point Correspondences (30 points)

 .

Figure 1: Example illustration, with epipolar lines shown in both images (Images courtesy Forsyth & Ponce)

This problem is concerned with the estimation of the fundamental matrix from point correspon-dences. In this problem, you will implement both the linear least-squares version of the eight-point algorithm and its normalized version to estimate the fundamental matrices. You will implement the methods in p1.py and complete the following:

Implement the linear least-squares eight point algorithm in lls eight point alg() and re-port the returned fundamental matrix. Remember to enforce the rank-two constraint for the fundamental matrix via singular value decomposition. Brie y describe your implementation in your written report.

[20 points]


Implement the normalized eight point algorithm in normalized eight point alg() and re-port the returned fundamental matrix. Remember to enforce the rank-two constraint for the fundamental matrix via singular value decomposition. Brie y describe your implementation in your written report.

[5 points]

After implementing methods to determine the Fundamental matrix, we can now determine epipolar lines. Speci cally to determine the accuracy of our Fundamental matrix, we will compute the average distance between a point and its corresponding epipolar line in compute distance to epipolar lines(). Brie y describe your implementation in your writ-ten report.

[5 points]

Matching Homographies for Image Recti cation (20 points)

Building o of the previous problem, this problem seeks to rectify a pair of images given a few matching points. The main task in image recti cation is generating two homographies H1; H2 that transform the images in a way that the epipolar lines are parallel to the horizontal axis of the image. A detailed description of the standard homography generation for image recti cation can be found in Course Notes 3. You will implement the methods in p2.py as follows:

The rst step in rectifying an image is to determine the epipoles. Complete the function compute epipole(). Include the epipoles you calculated in your written report.

[2 points]

Tip: If you want to check your answer, recall that the epipole is where all the epipolar lines intersect.

Finally, we can determine the homographies H1 and H2. We rst compute H2 as the ho-mography that takes the second epipole e2 to a point at in nity (f; 0; 0). The matching homography H1 is computed by solving a least-squares problem. Complete the function compute matching homographies(). In your written report include a description of your implementation, the homographies computed for the sample image pair, and your recti ed images plotted using plot epipolar lines on images().

[18 points]

The Factorization Method (15 points)

In this question, you will explore the factorization method, initially presented by Tomasi and Kanade, for solving the a ne structure from motion problem. You will implement the methods in p2.py and complete the following:

Implement the factorization method as described in lecture and in the course notes. Complete the function factorization method(). Brie y describe your implementation in your written report.

[5 points]

Run the provided code that plots the resulting 3D points. Compare your result to the ground truth provided. The results should look identical, except for a scaling and rotation. Explain why this occurs.

[3 points]


Report the 4 singular values from the SVD decomposition. Why are there 4 non-zero singular values? How many non-zero singular values would you expect to get in the idealized version of the method, and why?

[2 points]

The next part of the code will now only load a subset of the original correspondences. Compare your new results to the ground truth, and explain why they no longer appear similar.

[3 points]

Report the new singular values, and compare them to the singular values that you found previously. Explain any major changes.

[2 points]

Triangulation in Structure From Motion (35 points)



Figure 2: The set of images used in this structure from motion reconstruction.

Structure from motion is inspired by our ability to learn about the 3D structure in the surround-ing environment by moving through it. Given a sequence of images, we are able to simultaneously estimate both the 3D structure and the path the camera took. In this problem, you will implement signi cant parts of a structure from motion framework, estimating both R and T of the cameras, as well as generating the locations of points in 3D space. Recall that in the previous problem we triangulated points assuming a ne transformations. However, in the actual structure from motion problem, we assume projective transformations. By doing this problem, you will learn how to solve this type of triangulation. In Course Notes 4, we go into further detail about this process. You will implement the methods in p4.py and complete the following:

Given correspondences between pairs of images, we compute the respective Fundamental and Essential matrices. Given the Essential matrix, we must now compute the R and T between the two cameras. However, recall that there are four possible R; T pairings. In this part, we seek to nd these four possible pairings, which we will later be able to decide between. In the course notes, we explain in detail the following process:

1. To compute R: Given the singular value decomposition E = UDV T , we can rewrite E = MQ where M = UZUT and Q = UW V T or UW T V T , where

Z =

2

1

0

03

and W =

21

0

0

3

4

0

1

0

5

0

1

0

5

0

0

0

40

0

1

Note that this factorization of E only guarantees that Q is orthogonal. To nd a rotation, we simply compute R = (det Q)Q.

2. To compute T : Given that E = U V T , T is simply either u3 or u3, where u3 is the third column vector of U.


Implement this in the function estimate initial RT(). We provide the correct R; T , which should be contained in your computed four pairs of R; T . Include the four pairs of R; T in your written report.

[5 points]

In order to distinguish the correct R; T pair, we must rst know how to nd the 3D point given matching correspondences in di erent images. The course notes explain in detail how to compute a linear estimate (DLT) of this 3D point:

For each image i, we have pi = MiP , where P is the 3D point, pi is the homogenous image coordinate of that point, and Mi is the projective camera matrix.

Formulate matrix

2

p1;1m13>

m11>

3

p1;2m13>

m12>

6

...

7

A =

6

7

6

7

6

7

6

pn;1mn3>

mn1>

7

pn;2mn3>

mn2>

4

5

where pi;1 and pi;2 are the xy coordinates in image i and mki> is the k-th row of Mi.

3. The 3D point can be solved for by using the singular value decomposition.

Implement the linear estimate of this 3D point in linear estimate 3d point(). Like before, we print out a way to verify that your code is working. Include the output generated by this part of the code in your written report.

[5 points]

However, we can do better than linear estimates, but usually this falls under some iterative nonlinear optimization. To do this kind of optimization, we need some residual. A simple one is the reprojection error of the correspondences, which is computed as follows:

For each image i, given camera matrix Mi, the 3D point P , we compute y = MiP , and nd the image coordinates

p0 = 1 y1

i y3 y2

Given the ground truth image coordinates pi, the reprojection error ei for image i is

ei = pi0 pi

The Jacobian is written as follows:

2

@e1

@e1

@e1

3

J =

@P1

@P2

@P3

6

...

...

...

7

6

7

6

7

6

@em @em

@em

7

4

5

@P1

@P2

@P3

Recall that each ei is a vector of length two, so the whole Jacobian is a 2K 3 matrix, where K is the number of cameras. Fill in the methods reprojection error() and jacobian(), which computes the reprojection error and Jacobian for a 3D point and its list of images. Like before, we print out a way to verify that your code is working. Include the output generated by this part of the code in your written report.

[5 points]


Implement the Gauss-Newton algorithm, which nds an approximation to the 3D point that minimizes this reprojection error. Recall that this algorithm needs a good initialization, which we have from our linear estimate in part (b). Also recall that the Gauss-Newton algorithm

is not guaranteed to converge, so, in this implementation, you should update the estimate of

^

the point P for 10 iterations (for this problem, you do not have to worry about convergence criteria for early termination):

^ ^

(J

T

J)

1

J

T

e

P = P

where J and e are the Jacobian and error computed from the previous part. Implement the

Gauss-Newton algorithm to nd an improved estimate of the 3D point in the

nonlinear estimate 3d point() function. Like before, we print out a way to verify that

your code is working. Include the output generated by this part of the code in your written

report.

[5 points]

Now nally, go back and distinguish the correct R; T pair from part (a) by implementing the method estimate RT from E(). You will do so by:

First, compute the location of the 3D point of each pair of correspondences given each R; T pair

Given each R; T you will have to nd the 3D point’s location in that R; T frame. The cor-rect R; T pair is the one for which the most 3D points have positive depth (z-coordinate) with respect to both camera frames. When testing depth for the second camera, we must transform our computed point (which is the frame of the rst camera) to the frame of the second camera.

Submit the output generated by this part of the code.

[5 points]

Congratulations! You have implemented a signi cant portion of a structure from motion pipeline. Your code is able to compute the rotation and translations between di erent cam-eras, which provides the motion of the camera. Additionally, you have implemented a robust method to triangulate 3D points, which enable us to reconstruct the structure of the scene. In order to run the full structure from motion pipeline, please change the variable run pipeline at the top of the main function to True. Submit the nal plot of the reconstructed statue. Hopefully, you can see a point cloud that looks like the frontal part of the statue in the above sequence of images.

Note: Since the class is using Python, the structure from motion framework we use is not the most e cient implementation. It will be common that generating the nal plot may take a few minutes to complete. Furthermore, Matplotlib was not built to be e cient for 3D rendering. Although it’s nice to wiggle the point cloud to see the 3D structure, you may nd that the GUI is laggy. If we used better options that incorporate OpenGL (see Glumpy), the visualization would be more responsive. However, for the sake of the class, we will only use the numpy-related libraries.

[10 points]
