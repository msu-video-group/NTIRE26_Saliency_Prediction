# CVPR-NTIRE Video Saliency Prediction Challenge 2026

[![Page](https://img.shields.io/badge/Challenge-Page-blue)](https://www.codabench.org/competitions/12842/)
[![Challenges](https://img.shields.io/badge/Challenges-NTIRE%202026-orange)](https://cvlai.net/ntire/2026/)

<img width="2637" height="1358" alt="NTIRE_LOGO_WIDE" src="https://github.com/user-attachments/assets/3be490bb-79fb-4a7a-b904-cda78a8de8fb" />


## Dataset
[![Dataset](https://img.shields.io/badge/Dataset-Google%20Drive-brightgreen)](https://drive.google.com/drive/folders/1zpNtlrWKowW5m0SOUymjZDPXUMmbn9jy?usp=sharing)
[![Dataset HuggingFace](https://img.shields.io/badge/Dataset-HuggingFace-yellow)](https://huggingface.co/datasets/ANDRYHA/VideoSaliencyChallenge)

We provide a novel audio-visual mouse saliency dataset with the following key-features:
* Diverse content: movie, sports, live, vertical videos, etc.;
* Large scale: **2000** videos with mean **18s** duration;
* High resolution: all streams are **FullHD**;
* **Audio** track saved and played to observers;
* Mouse fixations from **>5000** observers (**>70** per video);
* License: **CC-BY**;

File structure:
1) `Videos.zip` — 2000 (1200 Train + 800 Test) .mp4 video (kindly reminder: many videos contain an audio stream and users watched the video with the sound turned ON!) 

2) `TrainTestSplit.json` — in this JSON we provide Train/Public Test/Private Test split of all videos 

3) `SaliencyTrain.zip/SaliencyTest.zip` — almost losslessly (crf 0, 10bit, min-max normalized) compressed continuous saliency maps videos for Train/Test subset 

4) `FixationsTrain.zip/FixationsTest.zip` — contains the following files for Train/Test subset: 

* `.../video_name/fixations.json` — per-frame fixations coordinates, from which saliency maps were obtained, this JSON will be used for metrics calculation

* `.../video_name/fixations_maps/` — binary fixation maps in '.png' format (since some fixations could share the same pixel, this is a lossy representation and is NOT used either in calculating metrics or generating Gaussians, however, we provide them for visualization and frames count checks)

5) `VideoInfo.json` — meta information about each video (e.g. license)

6) `SampleSubmission.zip` — example submission for the challenge, obtained from fitted Center Prior Gaussian over mean training saliency maps.

## Evaluation

### Environment setup

```
conda create -n saliency python=3.8.16
conda activate saliency
pip install numpy==1.24.2 opencv-python==4.7.0.72 tqdm==4.65.0
conda install ffmpeg=4.4.2 -c conda-forge
```
### Run evaluation
Archives with videos were accepted from challenge participants as submissions and scored using the same pipeline as in `bench.py`.

Usage example:

1) Check that your predictions match the structure and names of the [baseline SampleSubmission](https://drive.google.com/file/d/1-luBWGvicN1JHZjq0h0imUVvBh5q1-4y/view?usp=sharing)
2) Install `pip install -r requirments.txt`, `conda install ffmpeg`
3) Download and extract `SaliencyTest.zip`,  `FixationsTest.zip`, and `TrainTestSplit.json` files from the dataset page
4) Run `python bench.py` with flags:
* `--model_video_predictions ./SampleSubmission-CenterPrior` — folder with predicted saliency videos
* `--model_extracted_frames ./SampleSubmission-CenterPrior-Frames` — folder to store prediction frames (should not exist at launch time), requires ~170 GB of free space
* `--gt_video_predictions ./SaliencyTest/Test` — folder from dataset page with gt saliency videos
* `--gt_extracted_frames ./SaliencyTest-Frames` — folder to store ground-truth frames (should not exist at launch time), requires ~170 GB of free space
* `--gt_fixations_path ./FixationsTest/Test` — folder from dataset page with gt saliency fixations
* `--split_json ./TrainTestSplit.json` — JSON from dataset page with names splitting
* `--results_json ./results.json` — path to the output results json
* `--mode public_test` — public_test/private_test subsets
5) The result you get will be available following `results.json` path
