# Light CNN, in pytorch
A [pytorch](http://pytorch.org/) implementation of [A Light CNN for Deep Face Representation with Noisy Labels](https://arxiv.org/abs/1511.02683) from the paper by Xiang Wu, Ran He, Zhenan Sun and Tieniu Tan.  The official and original Caffe code can be found [here](https://github.com/AlfredXiangWu/face_verification_experiment).  

### Table of Contents
- <a href='#installation'>Installation</a>
- <a href='#datasets'>Datasets</a>
- <a href='#training'>Train</a>
- <a href='#evaluation'>Evaluate</a>
- <a href='#performance'>Performance</a>
- <a href='#citation'>Citation</a>
- <a href='#references'>References</a>

## Installation
- Install [pytorch](http://pytorch.org/) following the website.
- Clone this repository.
	- Note: We currently only run it on Python 2.7.	

## Datasets
- Download face dataset such as  CASIA-WebFace, VGG-Face and MS-Celeb-1M.
	- The MS-Celeb-1M clean list is uploaded: [Baidu Yun](http://pan.baidu.com/s/1gfxB0iB), [Google Drive](https://drive.google.com/file/d/0ByNaVHFekDPRbFg1YTNiMUxNYXc/view?usp=sharing).
- All face images are converted to gray-scale images and normalized to **144x144** according to landmarks. 
- According to the five facial points, we not only rotate two eye points horizontally but also set the distance between the midpoint of eyes and the midpoint of mouth(ec_mc_y), and the y axis of midpoint of eyes(ec_y) .
- The aligned LFW images are uploaded on [Baidu Yun](https://pan.baidu.com/s/1eR6vHFO).
  
  Dataset     | size    |  ec_mc_y  | ec_y  
  :----| :-----: | :----:    | :----: 
  Training set | 144x144 |     48    | 48    
  Testing set  | 128x128 |     48    | 40 

## Training 
- To train Light CNN using the train script simply specify the parameters listed in ```train.py``` as a flag or manually change them.
```Shell
python train.py --root_path=/path/to/your/datasets/ \
		--train_list=/path/to/your/train/list.txt \
		--val_list=/path/to/your/val/list.txt \
		--save_path=/path/to/your/save/path/ \
		--num_classes=n
```
	
- Tips:
	- The lists of train and val datasets are followed by the format of caffe. The details of data loader is shown in ```load_imglist.py```. Or you can use ```torchvision.datasets.ImageFolder``` to load your datasets.
	- The ```num_classes``` denotes the number of identities in your training dataset.
	- We enlarge the learning rate for the parameters of fc2 which may lead better performance. If the training is collapsed on your own datasets, you can decrese it. 
	- We modify the implementation of SGD with momentum since the official pytorch implementation is different from Sutskever et. al. The details are shown in [here](http://pytorch.org/docs/master/optim.html#torch.optim.SGD).
	- When training by pytorch, you can set a larger learning rate than caffe and it is faster converaged by pytorch than caffe for Light CNN. 
	
## Evaluation

- To evaluate a trained network:
```
python extract_features.py --resume=/path/to/your/model \
			   --root_path=/path/to/your/datasets/ \
			   --img_list=/path/to/your/list.txt \
			   --save_path=/path/to/your/save/path/ \
			   --num_classes=n
```
- You can use ```vlfeat``` or ```sklearn``` to evaluate the features on ROC and obtain ```EER``` and ```TPR@FPR``` for your testing datasets. 

## Performance

## Citation
If you use our models, please cite the following paper:

	@article{wulight,
	  title={A Light CNN for Deep Face Representation with Noisy Labels},
	  author={Wu, Xiang and He, Ran and Sun, Zhenan and Tan, Tieniu}
	  journal={arXiv preprint arXiv:1511.02683},
	  year={2015}
	}
	
## References
- [Original Light CNN implements (caffe)](https://github.com/AlfredXiangWu/face_verification_experiment).
- [Pytorch](https://github.com/pytorch/pytorch). 


