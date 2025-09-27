# ARGP : Adaptive and Recoverable 3D Gaussian Splatting Pruning for Efficient Real-Time Scene Reconstruction

This repository contains the official authors implementation associated with the paper "Adaptive and Recoverable 3D Gaussian Splatting Pruning for Efficient Real-Time Scene Reconstruction". 

![Teaser image](assets/ARGP.png)

Abstract: *3D Gaussian Splatting (3DGS) has emerged as an efficient explicit representation for real-time novel view synthesis. However, repeated densification during training often leads to uncontrolled Gaussian growth, causing heavy memory overhead and slow convergence. This redundancy stems from two limitations: (1) fixed opacity thresholds that fail to adapt to evolving distributions; (2) irreversible pruning strategies that risk eliminating essential Gaussians. 
To address this, we propose *Adaptive and Recoverable Gaussian Pruning (ARGP)*, a training-integrated framework that suppresses redundant growth while preserving reconstruction quality. During densification, we employ *Adaptive Opacity Pruning (AOP)*, which removes a fixed proportion of low-opacity Gaussians based on quantiles, effectively suppressing redundancy. In the fine-tuning stage, we introduce *Iterative Recovery Pruning (IRP)*, which selectively reinstates critical Gaussians using a gradient-informed recovery score, thus preventing over-pruning and preserving reconstruction quality. 
Extensive experiments on Mip-NeRF 360, Tanks & Temples, and Deep Blending validate the effectiveness our proposed ARGP. For instance, ARGP achieves an 85.4% reduction in Gaussian counts and 1.60× training speedup, while maintaining competitive reconstruction quality on Tanks & Temples.*
 




## Dataset
The used datasets, MipNeRF360 is hosted by the paper authors [here](https://jonbarron.info/mipnerf360/). Tank & Temple and DeepBlending are obtained from [here](https://github.com/graphdeco-inria/gaussian-splatting/).


## Run
Follow the setup instructions for the original [3D-GS](https://github.com/graphdeco-inria/gaussian-splatting/) codebase. Our code changes are made in (1) the differential renderer submodule and (2) the Python files in this repo.

### Environment
```shell
git clone https://github.com/Sinyo-Liu/ARGP --recursive
cd ARGP

conda create -n argp_env python=3.8
conda activate argp_env

# install pytorch
pip install torch==2.1.0 torchvision==0.16.0 torchaudio==2.1.0 --index-url https://download.pytorch.org/whl/cu121

# install dependencies
pip install -r requirements.txt

# install submodules
cd submodules/diff-gaussian-rasterization
python setup.py install

cd submodules/simple-knn
python setup.py install

```


### Train
```shell
python train.py -s /path/to/dataset --eval
```

### Render & Evaluation
```shell
python render.py -m /path/to/output 
python metrics.py -m /path/to/output
```

## Example Results
We provide example ckpts of re-produced 3DGS and ARGP for several scenes which can be downloaded from [Google Drive](https://drive.google.com/drive/folders/1lGSYpIE1t80-RQMNWEe7iNc0BmSoZYp6?usp=sharing). If you want to evaluate our pre-trained models, you will have to download the corresponding source data sets and indicate their location to `render.py` with an additional `--source_path/-s` flag.

```
python render.py -m /path/to/pre-trained_model -s /path/to/dataset
python metrics.py -m /path/to/pre-trained_model
```