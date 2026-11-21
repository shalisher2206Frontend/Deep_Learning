# Pothole Detection

Author: Sharipov Alisher

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/shalisher2206Frontend/YOUR_REPOSITORY/blob/main/Pothole_Detection.ipynb)

Current notebook covers Parts 1–3. Final test evaluation is still pending.

## 1. Dataset

The goal is to detect potholes and draw bounding boxes around them.

- Dataset: [Roboflow Pothole v1](https://universe.roboflow.com/brad-dwyer/pothole-voxrl/dataset/1).
- Creator: Atikur Rahman Chitholian.
- License: [ODbL v1.0](https://opendatacommons.org/licenses/odbl/1-0/). Retain attribution and applicable license notices.
- One class: `0 = pothole`.
- Annotation format: YOLO TXT, one row per pothole:

```text
class_id x_center y_center width height
```

Coordinates are normalized between 0 and 1. Images without potholes have no annotation rows.

We chose this dataset because it is small, already labeled and contains one class.

The original split contains 465 training, 133 validation and 67 test images.

Final counts after cleaning:

| Split | Images | Pothole boxes |
|---|---:|---:|
| Training | TODO | TODO |
| Validation | TODO | TODO |
| Test | TODO | TODO |

### Data checks

The notebook checks label formats and coordinates, searches for duplicate candidates across splits, and displays five images with ground-truth boxes.

One invalid box was found in the test label for:

`img-415_jpg.rf.2285ec32b9bdc584221e193dc135e3fd`

We decided to exclude that image and its matching label file.

The initial similarity check reported 44 possible duplicate pairs. These are candidates requiring visual review. Confirmed duplicates, near-duplicates and frames from the same source must remain in one split.

Review decisions and any split changes: TODO.

If creating a new split, record the grouping method and random seed.

## 2. Run in Google Colab

1. Download the dataset as a YOLOv8 ZIP.
2. Open the notebook using the Colab button.
3. Select Runtime → Change runtime type → GPU.
4. Run the setup cells and upload the dataset ZIP.
5. After loading the dataset, run this exclusion cell:

```python
name = "img-415_jpg.rf.2285ec32b9bdc584221e193dc135e3fd"
(root / "test/images" / f"{name}.jpg").unlink(missing_ok=True)
(root / "test/labels" / f"{name}.txt").unlink(missing_ok=True)
```

6. Rerun the label checks and resolve confirmed data issues.
7. Run Parts 2–3 in the same Colab session.
8. Download the executed notebook and result ZIPs.

The notebook installs the libraries, creates the dataset configuration and downloads the initial pretrained weights automatically.

## 3. Baseline and controlled experiment

- Framework: Ultralytics 8.3.228.
- Image library: Pillow 11.0.0.
- Model: YOLOv8n.
- Initial pretrained weights: `yolov8n.pt`.
- Epochs: 30.
- Batch size: 8.
- Optimizer: SGD.
- Initial learning rate: 0.01.
- Random seed: 42.
- Early stopping: disabled.

Actual training times and runtime versions are saved in the run information files. Each run saves its configuration, per-epoch metrics, learning curves and best validation checkpoint.

### Hypothesis

Increasing training image size from 416 to 640 may improve detection of small potholes, but training may take longer.

Only training image size changes. Both runs use the same prepared split and start from the same pretrained weights.

Both selected checkpoints are evaluated on validation images at size 640, confidence floor 0.001 and NMS IoU 0.70.

Copy the actual values from `comparison.csv`:

| Run | Training size | Precision | Recall | mAP@0.50 | Training minutes |
|---|---:|---:|---:|---:|---:|
| Baseline | 416 | TODO | TODO | TODO | TODO |
| Experiment | 640 | TODO | TODO | TODO | TODO |

Explanation: TODO — describe the metric changes and whether they were worth the additional training time.

## 4. Final test evaluation

This part is pending.

Choose the final run using validation results, then evaluate its best checkpoint on the held-out test set.

- Selected run and reason: TODO.
- Test precision: TODO.
- Test recall: TODO.
- Test mAP@0.50: TODO.
- Evaluation image size: TODO.
- Confidence threshold: TODO.
- NMS IoU threshold: TODO.

Precision measures how many reported detections are correct. Recall measures how many annotated potholes are found. mAP@0.50 summarizes detection quality across confidence thresholds, matching boxes at IoU of at least 0.50.

Add at least five test prediction examples showing boxes, class names and confidence scores.

Explain three failure cases, their likely causes and practical next steps. If fewer than three failures occur, state this and discuss challenging examples instead.

## 5. Saved results and weights

- Baseline outputs: TODO — link to `baseline_results.zip`.
- Experiment outputs: TODO — link to `experiment_results.zip`.
- Final trained checkpoint: TODO — working download link to the selected `weights/best.pt`.

The archives contain configurations, metrics, learning curves and trained checkpoints. The experiment archive also contains the comparison table and dataset configuration.

Keep small result tables, curves, configurations and prediction examples visible in the repository or executed notebook.

## 6. Limitations and credits

The dataset is small and contains one class. The similarity check can flag different images as possible duplicates and cannot identify every related source frame automatically.

Remaining issues and observed model limitations: TODO.

Credits: Atikur Rahman Chitholian, Roboflow, Ultralytics and ChatGPT assistance with code and explanations.
