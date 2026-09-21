## CSE5283-pose-estimation

## TASK 1:
identified_objects = [
    "BBQSauce",
    "Milk",
    "AlphabetSoup",
    "CreamCheese",
    "MacaroniAndChees",
    "Corn",
    "Butter",
    "OrangeJuice",
    "Pineapple",
    "Ketchup",

]

Object chosen is Macaroni and Cheese

<img width="389" height="227" alt="image" src="https://github.com/user-attachments/assets/cf24e8c8-afde-425f-9741-36b0d74f2fda" />

## TASK 2:
# A: 
axis-aligned bounding box (AABB) of every vertex in the mesh: for each of the three coordinate axes (X, Y, Z) independently I found the minimum and maximum vertex coordinate, and the difference (extents) gives the box's size along that axis, in the mesh's native units (millimeters, per the BOP/HOPE convention).

# B: 
clicked pixel coordinates:
  
  corner 0: (867.0, 472.0)
  
  corner 1: (962.0, 894.0)
  
  corner 2: (1310.0, 814.0)
  
  corner 3: (1167.0, 415.0)
  
  <img width="458" height="273" alt="image" src="https://github.com/user-attachments/assets/dbafb495-17c3-4b42-8bf6-5baf339636b4" />

My maually selected points in the image
# C: 

H =

 [[ 2.6908e+00 -2.9260e-01  8.6700e+02]
 
 [-3.6840e-01  1.7353e+00  4.7200e+02]
 
 [ 2.0000e-04 -9.0000e-04  1.0000e+00]]

lambda = 540.665267

R (Omega) =

 [[ 0.9745  0.2239 -0.0123]
 
 [-0.1887  0.8486  0.4943]
 
 [ 0.1211 -0.4794  0.8692]]

t (tau) =

 [-38.0876 -19.719  540.6653]

# D:

 ||M^T M - I||_F (raw M, before orthonormalization) = 0.035364

det(M) (raw M, before orthonormalization)           = 0.999664

||M - R||_F (after SVD correction)                  = 0.017682

 The small value of ‖M − R‖_F = 0.0177 indicates that my four clicked corner points were geometrically consistent and accurately localized, since a nearly-rigid raw estimate (needing only a tiny SVD correction to become a true rotation) is what you'd expect from clean, precise correspondences rather than noisy or mislabeled clicks.

# E:

corner 0: reprojected=(867.0,472.0)  clicked=(867.0,472.0)  error=0.00px

corner 1: reprojected=(962.3,887.7)  clicked=(962.0,894.0)  error=6.26px

corner 2: reprojected=(1314.1,808.4)  clicked=(1310.0,814.0)  error=6.97px

corner 3: reprojected=(1170.9,415.2)  clicked=(1167.0,415.0)  error=3.87px

mean reprojection error: 4.28 px

So the mesh lines up on top the object box in the image. This is directly the face I chose to click the 4 corners on.

<img width="461" height="282" alt="image" src="https://github.com/user-attachments/assets/e5ffbff9-0c6b-4282-89e3-dbe33b8b0091" />

Reprojected mesh on top of the selected points in the image
## TASK 3:

# A:

<img width="700" height="299" alt="image" src="https://github.com/user-attachments/assets/c13f1436-4a7a-416e-93a8-c41f186f38b3" />

Template render of mesh and depth map of mesh

# B:

Idx | Template Render (u, v) |     Scene Photo (u, v)

-------------------------------------------------------

  0 | ( 753.71,  262.90)       | ( 867.00,  472.00)   [Automated Corner]
  
  1 | ( 722.88,  812.14)       | ( 962.00,  894.00)   [Automated Corner]
  
  2 | (1186.30,  799.44)       | (1310.00,  814.00)   [Automated Corner]
  
  3 | (1155.64,  207.07)       | (1167.00,  415.00)   [Automated Corner]
  
  4 | (1210.96,  825.90)       | (1290.00,  850.00)   [Manual Feature]
  
  5 | ( 750.82,  840.14)       | ( 959.00,  919.00)   [Manual Feature]

<img width="721" height="216" alt="image" src="https://github.com/user-attachments/assets/1a6276aa-f0f4-45eb-b2ad-3b895a7777a4" />

Mathcing the selected points from the template in 3D to the real image with 6 points, 4 on one plane and the 5 + 6 on a second plane

# C:
--- 3 Unprojected Template Camera Points (X_cam) ---

Point 0: X_cam = [-61.34, -75.60, 403.75] (units: mm)

Point 1: X_cam = [-61.55, 73.81, 353.57] (units: mm)

Point 2: X_cam = [51.67, 64.79, 324.59] (units: mm)

Units Confirmation: Millimeters (inherited from mesh_t2 vertex definitions and depth rasterization).

# D:
Recovered R (PnP) =

 [[-0.0886 -0.9677  0.2362]
 
 [-0.1778  0.2487  0.9521]
 
 [-0.9801  0.0424 -0.1941]]

Recovered t (PnP) =

 [ -38.2484  -40.8493 -488.1723] (mm)

Mean Reprojection Error: 9.479 px

# E:
The mesh seems to fit very well towards the bottom of the object where I was able to select 2 additional points on a different plane than the rest of the points, giving that accurate depth and 3D matching. But is a little off towards the top right corner, almost making the mesh look like it is closer to the camera than the real box. This most likely happens because of the fact there is only one plane depctied in the points chosen towards this edge of the box object.

<img width="554" height="319" alt="image" src="https://github.com/user-attachments/assets/b50af14b-d9d2-497f-ac83-797635c12413" />

3D repojection of mesh onto the real image.

# F:
--- Comparison of Task 2 (Homography) vs Task 3 (PnP) ---

Rotation difference angle: 180.000 degrees (3.1416 rad)

Translation difference:    984.70 mm

t (Task 2 aligned): [ 40.95  29.24 490.83]

t (Task 3 PnP):     [ -38.25  -40.85 -488.17]

They do not agree. While the magnitudes of the translations are nearly identical ($\approx 493\text{ mm}$ from the camera), the two estimated poses are in direct physical contradiction due to a complete sign/orientation reversal. Both methods captured the correct geometric line of sight, but Task 3 converged to the geometrically flipped/reflected dual solution of the camera projection equations rather than the physically valid front-facing pose.
## TASK 4:
Now for the Mast3r matching the points do not match entirely on the object between the two images, here is a possible cause: While MASt3R extracted 29 candidate correspondences with valid depth on the template render, the recovered pose diverged dramatically from the ground truth Task 3 pose ($\Delta \theta = 165.17^\circ$, $\Delta t = 656.51\text{ mm}$).This failure is driven by the severe synthetic-to-real domain gap:Lack of Surface Texture: The CAD template uses flat-shaded polygon rasterization without packaging graphics or text, while the photograph is dominated by high-frequency brand typography and reflections. With no identifiable interior features on the template, MASt3R could only generate 32 raw matches, compared to the hundreds typical of textured scenes.Edge Misalignment & Planar Ambiguity: Without surface descriptors, the matcher latched onto the high-contrast silhouette boundary of the CAD render. Due to the two-fold symmetry of the rectangular box, the matches inverted the orientation, resulting in a near-$180^\circ$ rotation discrepancy ($165.17^\circ$).Degenerate RANSAC Consensus: Although 14 points were flagged as inliers with a low reprojection error ($3.21\text{ px}$), these points formed a spurious coplanar consensus on background/edge clutter, forcing PnP to converge to an impossible camera depth ($t_z \approx 163\text{ mm}$) and placing the object over half a meter away from its true 3D position.

# A: 
Raw reciprocal NN matches: 32

<img width="737" height="236" alt="image" src="https://github.com/user-attachments/assets/59bc6d42-3f87-4cd3-a993-c01dace987be" />

# B+C: 

Matches with valid template depth: 29 / 32

Inliers: 14 / 29

R (Task 4) =

 [[-0.4871  0.8717 -0.0525]
 
 [ 0.0968 -0.0058 -0.9953]
 
 [-0.8679 -0.4899 -0.0816]]

t (Task 4) = [  -0.84 -113.27  163.26] mm

Inlier reprojection error: mean 3.205 px, median 3.141 px

# --- Task 4 (MASt3R+RANSAC) vs Task 3 (hand-matched PnP) ---

Rotation diff: 165.169 deg | Translation diff: 656.51 mm

(Extra) vs Task 2: 92.637 deg, 359.66 mm

Points used -> Task 3: 6 | Task 4: 29 fed to RANSAC, 14 inliers

Scene matches inside projected silhouette: all 82.8% | inliers 92.9%

# D:
The template render is untextured, flat-shaded and on an empty background. That could bias MASt3R toward matching the box's silhouette and edges rather than its printed logo and text. In the photo those edges can also be matched to background clutter, such as table edges, shadows or other objects. This would give confident but wrong correspondences that sit systematically off the object. A second risk is that the render has no lighting or texture cues, so matches may cluster on high-contrast borders instead of being spread across the box face.
