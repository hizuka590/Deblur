# ESTRNN & BSD
Efficient Spatio-Temporal Recurrent Neural Network for Video Deblurring (ECCV2020 Spotlight)
[Conference version (old BSD dataset)](https://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123510188.pdf) 

Real-world Video Deblurring: A Benchmark Dataset and An Efficient Spatio-Temporal Recurrent Neural Network
[Journal version (under review; new BSD dataset)](https://arxiv.org/abs/2106.16028)

by Zhihang Zhong, Ye Gao, Yinqiang Zheng, Bo Zheng, Imari Sato

This work presents an efficient RNN-based model and **the first real-world dataset for video deblurring** :-)

## Visual Results

### Results on REDS (Synthetic)
![image](https://github.com/zzh-tech/Images/blob/master/ESTRNN/reds.gif)


### Results on GOPRO (Synthetic)
![image](https://github.com/zzh-tech/Images/blob/master/ESTRNN/gopro.gif)


### Results on BSD (Real-world)
![image](https://github.com/zzh-tech/Images/blob/master/ESTRNN/bsd.gif)


## Beam-Splitter Deblurring Dataset (BSD)

We have collected a new real-world video deblurring dataset ([BSD](https://drive.google.com/file/d/19cel6QgofsWviRbA5IPMEv_hDbZ30vwH/view?usp=sharing)) with more scenes and better setups (center-aligned), using the proposed beam-splitter acquisition system:

![image](https://github.com/zzh-tech/Images/blob/master/ESTRNN/bsd_system.png)
![image](https://github.com/zzh-tech/Images/blob/master/ESTRNN/bsd_demo.gif)

The configurations of the new BSD dataset are as below:

<img src="https://github.com/zzh-tech/Images/blob/master/ESTRNN/bsd_config.png" alt="bsd_config" width="450"/>

Quantitative results on different setups of BSD:

<img src="https://github.com/zzh-tech/Images/blob/master/ESTRNN/results_on_bsd.png" alt="bsd_config" width="600"/>


## Quick Start

### Prerequisites

- Python 3.6
- PyTorch 1.6 with GPU
- opencv-python
- scikit-image
- lmdb
- thop
- tqdm
- tensorboard

### Downloading Datasets

Please download and unzip the dataset file for each benchmark.

- [**BSD**](https://drive.google.com/file/d/19cel6QgofsWviRbA5IPMEv_hDbZ30vwH/view?usp=sharing)
- [GOPRO](https://drive.google.com/file/d/1Tni2gZzI_Hd03Msc8Rrxl5JklznqO9AG/view?usp=sharing)
- [REDS](https://drive.google.com/file/d/1wMOtIqmnNfXqe0_-Xq0Xj6WMspCaEgRR/view?usp=sharing)

### Training

Specify *\<path\>* (e.g. "*./dataset/*") as where you put the dataset file.

Modify the corresponding dataset configurations in the command, or change the default values in "*./para/paramter.py*". 

Training command is as below:

```bash
python main.py --data_root <path> --dataset BSD --ds_config 2ms16ms
```

You can also tune the hyper-parameters such as batch size, learning rate, epoch number (P.S.: the actual batch size for ddp mode is num_gpus*batch_size):

```bash
python main.py --lr 1e-4 --batch_size 4 --num_gpus 2 --trainer_mode ddp
```

If you want to train on your own dataset, please refer to "*/data/how_to_make_dataset_file.ipynb*".

### Inference

Please download [checkpoints](https://drive.google.com/file/d/1n39u16UP5FUe04NDK-rpiBQtjUHibyRf/view?usp=sharing) of pretrained models for different setups and unzip them under the main directory.

#### Dataset (Test Set) Inference

Command to run a pre-trained model on BSD (3ms-24ms):

```bash
python main.py --test_only --test_checkpoint ./checkpoints/ESTRNN_C80B15_BSD_3ms24ms.tar --dataset BSD --ds_config 3ms24ms --video
```

#### Blurry Video Inference

Specify **"--src \<path\>"** as where you put the blurry video file (e.g., "--src ./blur.mp4") or video directory (e.g., "--src ./blur/", the image files under the directory should be indexed as "./blur/00000000.png", "./blur/00000001.png", ...).

Specify **"--dst \<path\>"** as where you store the results (e.g., "--dst ./results/").

Command to run a pre-trained model for a blurry video is as below:

```bash
python inference.py --src <path> --dst <path> --ckpt ./checkpoints/ESTRNN_C80B15_BSD_2ms16ms.tar
```

## Citing

If you use any part of our code, or ESTRNN and BSD are useful for your research, please consider citing:

```bibtex
@inproceedings{zhong2020efficient,
  title={Efficient spatio-temporal recurrent neural network for video deblurring},
  author={Zhong, Zhihang and Gao, Ye and Zheng, Yinqiang and Zheng, Bo},
  booktitle={European Conference on Computer Vision},
  pages={191--207},
  year={2020},
  organization={Springer}
}

@misc{zhong2021efficient,
      title={Efficient Spatio-Temporal Recurrent Neural Network for Video Deblurring}, 
      author={Zhihang Zhong and Ye Gao and Yinqiang Zheng and Bo Zheng and Imari Sato},
      year={2021},
      eprint={2106.16028},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}

```
