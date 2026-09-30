---
dataset_info:
  features:
  - name: image_id
    dtype: string
  - name: image
    dtype: image
  - name: mean_score
    dtype: float32
  - name: total_votes
    dtype: int32
  - name: rating_counts
    sequence: int32
  splits:
  - name: train
    num_bytes: 30423653087.844166
    num_examples: 229957
  - name: test
    num_bytes: 3318287806.0878363
    num_examples: 25551
  download_size: 33811841760
  dataset_size: 33741940893.932003
configs:
- config_name: default
  data_files:
  - split: train
    path: data/train-*
  - split: test
    path: data/test-*
---

# AVA-Huggingface

This repository contains a Hugging Face dataset built from the [AVA (Aesthetic Visual Analysis)](https://github.com/imfing/ava_downloader) dataset. The dataset includes images along with their aesthetic scores, total votes, and rating distributions. The data is prepared by filtering out images with fewer than 50 votes and stratifying them based on the computed mean aesthetic score.

## Dataset Overview

- **Image ID**: Unique identifier for each image.
- **Image**: The actual image loaded from disk.
- **Mean Score**: The average aesthetic score computed from the rating counts.
- **Total Votes**: The total number of votes for the image.
- **Rating Counts**: The distribution of ratings (scores 1 through 10).

## Preprocessing Steps

1. **Parsing**: The `AVA.txt` file is parsed to extract the rating counts for each image.
2. **Filtering**: Images with fewer than 50 total votes are excluded.
3. **Stratification**: The filtered images are stratified into 10 bins based on their mean aesthetic score.
4. **Dataset Creation**: The data is then converted into a Hugging Face dataset with custom features for direct use with Hugging Face’s tools.

## Train/Test Split Note

**Important:** The train and test splits provided in this repository are generated using a simple range-based selection:

```python
dataset_full_dict = DatasetDict({
    "train": hf_dataset_full.select(range(int(0.9 * len(hf_dataset_full)))),
    "test": hf_dataset_full.select(range(int(0.1 * len(hf_dataset_full)), len(hf_dataset_full)))
})
```

This split is **not** the same as any official train/test split from the original AVA dataset. It is only meant to facilitate experiments and should not be considered as a validated split for rigorous evaluations.

## How to Use

You can load the dataset directly using the Hugging Face `datasets` library:

```python
from datasets import load_dataset

dataset = load_dataset("trojblue/AVA-Huggingface")
print(dataset)
```

(a stratified subset for model testing is also available here): 

- [trojblue/AVA-aesthetics-10pct-min50-10bins · Datasets at Hugging Face](https://huggingface.co/datasets/trojblue/AVA-aesthetics-10pct-min50-10bins)



## Citation

If you use this dataset in your research, please consider citing the original work:

```bibtex
@inproceedings{murray2012ava,
  title={AVA: A Large-Scale Database for Aesthetic Visual Analysis},
  author={Murray, N and Marchesotti, L and Perronnin, F},
  booktitle={Proceedings of the European Conference on Computer Vision (ECCV)},
  pages={3--18},
  year={2012}
}
```

## License

Please refer to the license of the original AVA dataset and ensure that you adhere to its terms when using this subset.
