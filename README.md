# OCT-tools
Segmentation and analysis of retinal layers from individual OCT b-scans, with an emphasis on computing choroid thickness. Developed by Sara Patterson (sarap44_at_uw.edu) in the [Neitz lab][neitzlab] at University of Washington. This is quick project that is unlikely to be maintained or further improved, however, contributions are welcome.

### Usage
The code works best when all the images are in a single folder with numbers as filenames (e.g., `1.png`, `2.png`, etc). The analysis and data will be saved with the prefix `im1_`, `im2_`, etc.

1. If you intend to compare images of the same eye, the first step is to align them. The `alignImages.m` function will calculate the rotation necessary to align two images and save the rotation value. Translations along the X-axis can be computed after segmentation.
2. Crop the images. Note: when the version of the image returned by the `octImage` property of the `OCT` class is rotated, then cropped so it's best to crop the image after rotating. MATLAB's built-in `imcrop` function is useful here.
3. Input the image to `ChoroidIdentification` to segment the layers.
4. Segment the RPE-Choroid and ILM boundaries.
5. Assign a Choroid Vertex (the peak of the choroid-sclera boundary).
6. Detect Edges (uses Matlab's builtin Canny edge detection) - This will return too many edges, but will be sensitive enough to pick out most the choroid-sclera boundary.
7. Remove the extra edges by the exclude edges button which allows drawing regions of interest around edges to exclude.
8. Fit the choroid-sclera boundary with a parabola.
9. Steps 7 and 8 are usually an iterative process. Once an initial choroid fit is present, Exclude Outliers will remove any edges X pixels away from the fit. X is an integer provided to the edit box right of the Exclude Outliers button.
10. Provide a name for the exported data (works best if this includes the image ID number like `im1`, `im2`, etc). This saves .txt files of the extracted boundaries, remaining edges and fit parameters. The `OCT` class properties are populated by searching for these files.
11. To compare two images, use `compareChoroids.m`. This function plots the two choroids and optionally computes an x-axis shift value to align the two foveal pits. The alignment is just a simple registration of maximum values and could be improved.



### Dependencies
This code was written in Matlab 2018 and requires the Bioinformatics, Computer Vision and Signal Processing toolboxes. All external dependencies are provided, except the free [GUI Layout Toolbox][guilayout], which can be installed from Matlab's Add-Ons window.

### References
The initial ILM and RPE segmentation uses a simplified implementation of an algorithm introduced in:
Chiu, S.J., Li, X.T., Nicholas, P., Toth, C.A., Izatt, J.A., Farsiu, S. (2010) Automatic segmentation of seven retinal layers in SDOCT images congruent with expert manual segmentation. *Optics Express*, 18(18), 19413-19428

[guilayout]: <https://www.mathworks.com/matlabcentral/fileexchange/47982-gui-layout-toolbox>
[neitzlab]: <http://www.neitzvision.com/>