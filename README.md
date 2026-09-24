SFM-Based Fault Detection on THEBE
This project fine-tunes a Seismic Foundation Model (SFM) for 2D fault segmentation on the THEBE dataset. It compares a direct Pure SFM baseline with the proposed boundary-aware refinement model.
Required Downloads
- SFM code: https://github.com/shenghanlin/seismicfoundationmodel
- Pretrained SFM checkpoint: https://drive.google.com/drive/folders/1RObf_J-37VCEALgOikMWODBlhz73IHIU?usp=sharing
The GitHub repository provides the SFM implementation and models_Segmentation.py. The Google Drive folder provides the pretrained checkpoint used by the notebooks, such as SFM-Base.pth.
Main Files
File	Purpose
thebe_sfm_only_baseline_10k_100ep.ipynb	Pure SFM baseline without augmentation or added refinement modules
thebe_sfm_10k_bdconv_centerline_gate_100ep.ipynb	Proposed model with local refinement, Sobel guidance, BDConv, and centerline gating
thebe_sfm_reproduces.ipynb	Earlier development experiment; not the official baseline
models_Segmentation.py	Original SFM model definition
SFM-Base.pth	Pretrained SFM checkpoint
patch_cache/	Saved 96 × 96 training and validation patches
THEBE_Fault_Detection_Results_Report.docx	Experimental results and visualizations


How to Use
1. Prepare the files
Download the SFM repository and checkpoint, then place the required files in accessible folders. The existing patch cache should contain:
train_10k_valid_seismic_images.npy
train_10k_valid_seismic_masks.npy
val_2k_valid_seismic_images.npy
val_2k_valid_seismic_masks.npy
Do not rebuild the cache when reproducing the reported results.
2. Update the paths
Edit the configuration cell at the beginning of the notebook:
THEBE_ROOT = r"D:\path\to\thebe"
SFM_ROOT = r"D:\path\to\seismicfoundationmodel"
SFM_CHECKPOINT = r"D:\path\to\SFM-Base.pth"
SFM_MODEL_FILE = r"D:\path\to\models_Segmentation.py"
CACHE_DIR = r"D:\path\to\patch_cache"
3. Run the baseline
Open and run:
thebe_sfm_only_baseline_10k_100ep.ipynb
This directly fine-tunes the pretrained SFM encoder with the original MLA decoder architecture. It does not use Sobel guidance, BDConv, local refinement, or centerline gating.
4. Run the proposed model
Open and run:
thebe_sfm_10k_bdconv_centerline_gate_100ep.ipynb
The notebook loads the same cache and adds the proposed boundary-aware refinement components. A new run directory is created automatically so previous results are not overwritten.
5. Evaluate
- The best checkpoint is selected using validation ODS F1.
- Full crosslines are reconstructed from overlapping 96 × 96 patches with stride 48.
- The threshold is selected using 200 validation crosslines.
- The same threshold is applied to 141 held-out test crosslines without retuning.
Main Results
Model	PR-AUC	Precision	Recall	F1	IoU
Pure SFM baseline	0.529	0.476	0.583	0.524	0.355
Proposed model	0.557	0.492	0.630	0.552	0.382


The proposed model improves test IoU from 0.355 to 0.382 and F1 from 0.524 to 0.552.
Notes
- Use the same cached patches for a fair baseline–proposed comparison.
- Do not select checkpoints or thresholds using the test set.
- Use a different output directory for every experiment.
- The current experiments use 2D THEBE crossline sections.

