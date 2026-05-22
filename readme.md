# Multi-view 3D Reconstruction

This notebook was developed as part of a computer vision course project and later prepared as a standalone demonstration of the reconstruction pipeline.

This notebook reconstructs scene structure from three views of the same scene. It estimates sparse 3D points and camera geometry from matched image features, then computes stereo disparity maps from a rectified image pair.

The reconstruction is relative rather than metric. The images do not include a measured camera calibration, so the recovered scale and geometry are approximate.

## Notebook

The main work is in [`mv_stereo_reconstruction.ipynb`](mv_stereo_reconstruction.ipynb). The pipeline is:

1. Detect and match SIFT features between views.
2. Estimate epipolar geometry and reject outliers with RANSAC.
3. Recover a relative pose for the first image pair and triangulate sparse 3D points.
4. Estimate the third camera matrix by resection from 2D to 3D correspondences.
5. Rectify the first image pair and compute SSD disparity maps.
6. Smooth the disparity maps with ROF total-variation denoising.

The notebook includes direct implementations of the normalized eight-point method, triangulation, camera resection, SSD disparity search, and ROF smoothing. OpenCV is used for SIFT and selected geometry utilities.

The notebook also shows intermediate checks such as inlier counts, third-view reprojection error, disparity statistics, and the effect of SSD patch size. A longer stage-by-stage walkthrough is in [`docs/mv_stereo_reconstruction_2_pipeline.md`](docs/mv_stereo_reconstruction_2_pipeline.md).

## Run

The notebook uses Python, NumPy, OpenCV, Matplotlib, and Jupyter. For a quick local setup:

```bash
python -m venv .venv
python -m pip install numpy opencv-python matplotlib jupyter
jupyter notebook
```

Open `mv_stereo_reconstruction.ipynb` from the project root and run it from top to bottom. The input views are included in `figures/`.

## Limitations

- The 3D reconstruction has arbitrary scale because the calibration is approximate.
- Uncalibrated rectification can introduce image warping.
- SSD stereo matching is sensitive to occlusions, low-texture areas, repeated patterns, and brightness changes.
- A larger reconstruction pipeline would normally use calibrated cameras, more views, and bundle adjustment.

## References

The implementation follows concepts from Jan Erik Solem, *Programming Computer Vision with Python*, especially the sections on epipolar geometry, triangulation, stereo reconstruction, and ROF denoising.

The sample views in `figures/` come from the Oxford multi-view image data used for the example.

## License

The project code and original documentation are released under the MIT License. The sample images are attributed above and should be checked against their source terms before reuse elsewhere.
