# FIVR-200K
<img src="https://raw.githubusercontent.com/MKLab-ITI/FIVR-200K/master/banner.png" width="100%">

An annotated dataset of YouTube videos designed as a benchmark for Fine-grained Incident Video Retrieval. The dataset comprises 225,960 videos associated with 4,687 Wikipedia events and 100 selected video queries. 

Project Website: [[link](http://ndd.iti.gr/fivr/)]

Paper: [[publisher](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=8668422)] [[arXiv](https://arxiv.org/abs/1809.04094)] [[pdf](https://arxiv.org/pdf/1809.04094.pdf)]

## Installation

* Clone this repo:
```bash
git clone https://github.com/MKLab-ITI/FIVR-200K
cd FIVR-200K
```
* You can install all the dependencies by
```bash
pip install -r requirements.txt
```
or
```bash
conda install --file requirements.txt
```
* Install [youtube-dl](https://ytdl-org.github.io/youtube-dl/index.html) (make sure it is up-to-date)

## Dataset format

* The files that contains the dataset can be found in [dataset](https://github.com/MKLab-ITI/FIVR-200K/tree/master/dataset) folder

* The video annotations are in file [annotation.json](https://github.com/MKLab-ITI/FIVR-200K/blob/master/dataset/annotation.json) that has the following format:
```bash
{
  "5MBA_7vDhII": {
    "ND": [
      "_0uCw0B2AgM",
      ...],
    "DS": [
      "hc0XIE1aY0U",
      ...],
    "CS": [
      "ydEqiuDiuyc",
      ...],    
    "IS": [
      "d_ZNjE7B4Wo",
      ...]
  },
  ....
}
```

* The events crawled from Wikipedia's [Current Event](https://en.wikipedia.org/wiki/Portal:Current_events) page are in file [events.json](https://github.com/MKLab-ITI/FIVR-200K/blob/master/dataset/events.json) that has the following format:
```bash
[
  {
    "headline": "iraqi insurgency", 
    "topic": "armed conflict and attack", 
    "date": "2013-01-22", 
    "text": [
      "car bombings in baghdad kill at least 17 people and injure dozens of others."
    ], 
    "href": [
      "http://www.bbc.co.uk/news/world-middle-east-21141242", 
      "https://www.reuters.com/article/2013/01/22/us-iraq-violence-idUSBRE90L0BQ20130122"
    ], 
    "youtube": [
      "ZpjqUq-EnbQ", 
      ...
    ]
  },
  ...
]
```

* The Youtube IDs of the videos in the dataset are in file [youtube_ids.txt](https://github.com/MKLab-ITI/FIVR-200K/blob/master/dataset/youtube_ids.txt)

* The global features of the benchmarked approaches in the paper can be found [here](http://ndd.iti.gr/features/)
    * The ordering is according to the [youtube_ids.txt](https://github.com/MKLab-ITI/FIVR-200K/blob/master/dataset/youtube_ids.txt)

## Download Videos

* Run the following command to download videos:
```bash
python download_dataset.py --video_dir VIDEO_DIR [--dataset_ids DATASET_FILE] [--cores NUMBER_OF_CODES] [--resolution RESOLUTION]
```

* An example to run the download script:
```bash
python download_dataset.py --video_dir ./videos --dataset_ids dataset/youtube_ids.txt --cores 4 --resolution 360
```

* Videos will be saved in the following directory structure ```VIDEO_DIR/YT_ID.mp4```

* The videos that are no longer available are stored in a text file with name ```missing_videos.txt```

## Evaluation

1. Generation of the result file 
    * A file that contains a dictionary with keys the YT ids of the query videos and values another dictionary with keys the YT ids of the dataset videos and values their similarity to the query.

    * Results can be stored in a JSON file with the following format:
    ```bash
    {
      "wrC_Uqk3juY": {
        "KQh6RCW_nAo": 0.716,
        "0q82oQa3upE": 0.300,
          ...},
      "k_NT43aJ_Jw": {
        "-KuR8y1gjJQ": 1.0,
        "Xb19O5Iur44": 0.417,
          ...},
      ....
    }
    ```

   * An implementation for the generation of the JSON file can be found [here](https://github.com/MKLab-ITI/FIVR-200K/blob/1c0c093f29eea6c71f8f154408b2a371b74cb427/calculate_similarities.py#L79)

2. Evaluation of the results
    * Run the following command to run the evaluation:
    ```bash
    python evaluation.py --result_file RESULT_FILE --relevant_labels RELEVANT_LABELS
    ```

    * An example to run the evaluation script:
    ```bash
    python evaluation.py --result_file ./results/lbow_vgg.json --relevant_labels ND,DS
    ```
    
3. Evaluation on the three retrieval task
    * Provide different values to the `relevant_labels` argument to evaluate your results for the three retrieval task
    ```bash
    DSVR: ND,DS
    CSVR: ND,DS,CS
    ISVR: ND,DS,CS,IS
    ```

* Reported results
    * To re-run the experiments of the paper run the following command
    ```bash
    bash evaluate_run.sh APPROACH_NAME FEATURES_NAME
    ```

    * An example to run the evaluation script:
    ```bash
    bash evaluate_run.sh BOW VGG
    ```
    
    * The results will probably not be the same as the reported to the paper, because we are constantly fixing mislabeled videos that were missed during the annotation process.
    
    * See in the the [Updates](https://github.com/MKLab-ITI/FIVR-200K#updates) section when was the last update of the dataset's annotation

## Updates

In case that you find a mislabeled video please submit it to the following form [here](https://docs.google.com/forms/d/e/1FAIpQLSdHWcPWB4YnAzap7VVJ4wUN_AIf2DIK3H97EjW6VjyHpM4ZSA/viewform?usp=pp_url)

* **Update 29/5**: fix labels for 373 videos

## Citation
If you use this code for your research, please cite our paper.
```
@article{kordopatis2019fivr,
  title={FIVR: Fine-grained Incident Video Retrieval},
  author={Kordopatis-Zilos, Giorgos and Papadopoulos, Symeon and Patras, Ioannis and Kompatsiaris, Ioannis},
  journal={IEEE Transactions on Multimedia},
  year={2019}
}
```

## Related Projects
**[NDVR-DML](https://github.com/MKLab-ITI/ndvr-dml)** **[Intermediate-CNN-Features](https://github.com/MKLab-ITI/intermediate-cnn-features)**

## License
This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details

## Contact for further details about the project

Giorgos Kordopatis-Zilos (georgekordopatis@iti.gr) <br>
Symeon Papadopoulos (papadop@iti.gr)