# CVPR-NTIRE26 Video Saliency Prediction Challenge 2026

[![Page](https://img.shields.io/badge/Challenge-Page-blue)](https://www.codabench.org/competitions/12842/)
[![Challenges](https://img.shields.io/badge/Challenges-NTIRE%202026-orange)](https://cvlai.net/ntire/2026/)

<img width="2637" height="1358" alt="NTIRE_LOGO_WIDE" src="https://github.com/user-attachments/assets/3be490bb-79fb-4a7a-b904-cda78a8de8fb" />

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

1) Check that your predictions match the structure and names of the [baseline CenterPrior submission](https://drive.google.com/file/d/1rPgMdb4L79OD2vvpDQyqWZIDox78rmxG/view)
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
