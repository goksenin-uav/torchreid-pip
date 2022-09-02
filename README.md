<div align="center">
<h1>
  Torchreid-Pip: Packaged version of Torchreid 
</h1>
<h4>
    <img width="700" alt="teaser" src="https://raw.githubusercontent.com/goksenin-uav/torchreid-pip/main/doc/logo.png">
</h4>
</div>

This repo is a packaged version of the [Torchreid](https://github.com/KaiyangZhou/deep-person-reid) algorithm.
### Installation
```
pip install torchreid
```

### Overview
##### 1. Import ``torchreid``
```python
import torchreid
```
##### 2. Load data manager

```python 
datamanager = torchreid.data.ImageDataManager(
    root="reid-data",
    sources="market1501",
    targets="market1501",
    height=256,
    width=128,
    batch_size_train=32,
    batch_size_test=100,
    transforms=["random_flip", "random_crop"]
)
```
##### 3 Build model, optimizer and lr_scheduler

```python 
model = torchreid.models.build_model(
    name="resnet50",
    num_classes=datamanager.num_train_pids,
    loss="softmax",
    pretrained=True
)

model = model.cuda()

optimizer = torchreid.optim.build_optimizer(
    model,
    optim="adam",
    lr=0.0003
)

scheduler = torchreid.optim.build_lr_scheduler(
    optimizer,
    lr_scheduler="single_step",
    stepsize=20
)
```
##### 4. Build engine

```python
engine = torchreid.engine.ImageSoftmaxEngine(
    datamanager,
    model,
    optimizer=optimizer,
    scheduler=scheduler,
    label_smooth=True
)
```
##### 5. Run training and test

```python
engine.run(
    save_dir="log/resnet50",
    max_epoch=60,
    eval_freq=10,
    print_freq=10,
    test_only=False
)
```
Citation
---------
If you use this code or the models in your research, please give credit to the following papers:
```bibtex
@article{torchreid,
    title={Torchreid: A Library for Deep Learning Person Re-Identification in Pytorch},
    author={Zhou, Kaiyang and Xiang, Tao},
    journal={arXiv preprint arXiv:1910.10093},
    year={2019}
} 

@inproceedings{zhou2019osnet,
    title={Omni-Scale Feature Learning for Person Re-Identification},
    author={Zhou, Kaiyang and Yang, Yongxin and Cavallaro, Andrea and Xiang, Tao},
    booktitle={ICCV},
    year={2019}
}

@article{zhou2021osnet,
    title={Learning Generalisable Omni-Scale Representations for Person Re-Identification},
    author={Zhou, Kaiyang and Yang, Yongxin and Cavallaro, Andrea and Xiang, Tao},
    journal={TPAMI},
    year={2021}
}
```