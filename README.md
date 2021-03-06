# Skeleton-Aware Networks for Deep Motion Retargeting
Reproduce the paper [Skeleton-Aware Networks for Deep Motion Retargeting](https://arxiv.org/abs/2005.05732) of [Kfir Aberman](https://kfiraberman.github.io), [Peizhuo Li](https://peizhuoli.github.io/).
Most of the pre-processing and post-processing codes which deal with motion capture files are borrowed from their github
in [Skeleton-Aware Networks for Deep Motion Retargeting](https://deepmotionediting.github.io/retargeting) 

### Requirements
Pytorch >= 1.3.1

### Demo
from left to right: input, target, output

![image](./images/base_ball.gif)

![image](./images/dancing_running_man.gif)


### Quick Start
First you need to download the test motion data we need from [google drive](https://docs.google.com/uc?export=download&id=1_849LvuT3WBEHktBT97P2oMBzeJz7-UP) or
[Baidu Disk](https://pan.baidu.com/s/1z1cQiqLUgjfxlWoajIPr0g), the pass code is (ye1q).

Place the Mixamo directory within ./datasets/

Then run
```bash
python inference.py
```

The output bvh files will be saved in the folder
```bash
./pretrained/result/
```

### Train
You need to download data from the Mixamo Datasets for training, here is a convenient 
[intro](https://forums.unrealengine.com/community/community-content-tools-and-tutorials/1376068-script-mixamo-download-script) 
about how to use a script to download fbx data from the Mixamo Datset.
After downloading, you need to customize the "data_path" variable in the script:
```bash
./datasets/fbx2bvh.py
```
Then run:
```bash
python preprocessing.py
```
to convert the fbx files to bvh file, as well as generating the train and validation files list.

At last, run
```bash
python train.py
```
It will train the network which could retarget the motion of "Aj" to "BigVegas" which are 2 different
characters from the "Mixamo" dataset.
Please note: The architectures of the neural networks are based on the topologies of the skeletons.So if you'd like train a model with
your customized skeletons. You might need to change some variables related with the topologies which
defined in the
```bash
./model/skeleton.py
``` 
also the names of joints defined in the
```bash
./datatsets/bvh_parser/py
```
An automatic way to generate the skeleton structure after skeleton pooling might be released in the future.

Learning curves showed below:

![Loss Curve](./images/loss_curve.png)

### Note
There are some changes compared with code released by the author:
1. I re-implement the architecture of the neural network. I use nn.Conv2d instead of
nn.Conv1d for the skeleton convolution, because I think 2D convolution is more in line with the description about skeleton
convolution according to the paper. However, it seems that 2D convolution are not as efficient as the original 1D convolution.

2. GAN part are deprecated(bad memories with it, training GAN needs lots of tricks). Learning to walk before you run.
So, technically my training process is in a "paired"(supervised) mode.

3. About the loss designing, I haven't add the end-effector loss into the training pipeline, multi-task losses training needs
tricks too.

3. IK optimization has not been implemented.

4. More data are needed for evaluating the generalization ability. 

### Visualization
You need to install Blender to visualize the result bvh files.
Just import the bvh file into Blender and press "White Space" Key.

## Citation
If you use this code for your research, please cite their fancy paper:
```
@article{aberman2020skeleton,
  author = {Aberman, Kfir and Li, Peizhuo and Sorkine-Hornung Olga and Lischinski, Dani and Cohen-Or, Daniel and Chen, Baoquan},
  title = {Skeleton-Aware Networks for Deep Motion Retargeting},
  journal = {ACM Transactions on Graphics (TOG)},
  volume = {39},
  number = {4},
  pages = {62},
  year = {2020},
  publisher = {ACM}
}
```
