# Automated-Objects-Removal-Inpainter

Automated object remover Inpainter is a project that combines Semantic segmentation and EdgeConnect architectures with minor changes in order to remove specified objects from photos. For Semantic Segmentation, the code from pytorch has been adapted, whereas for EdgeConnect, the code has been adapted from [https://github.com/knazeri/edge-connect](https://github.com/knazeri/edge-connect).

This project is capable of removing objects from list of 20 different ones.It can be used as photo editing tool as well as for Data augmentation.

Python 3.8.18 and pytorch 1.5.1 have been used in this project.

## How does it work?

    
<img src="https://user-images.githubusercontent.com/31131069/89242695-2b9f5e80-d5d0-11ea-8c72-c865cc72616b.png" width="30%"></img> 

Semantic segmenator model of deeplabv3/fcn resnet 101 has been combined with EdgeConnect. A pre-trained segmentation network has been used for object segmentation (generating a mask around detected object), and its output is fed to a EdgeConnect network along with input image with portion of mask removed. EdgeConnect uses two stage adversarial architecture where first stage is edge generator followed by image completion network. EdgeConnect paper can be found [here](https://arxiv.org/abs/1901.00212) and code in this [repo](https://github.com/knazeri/edge-connect)

## Prerequisite
* python 3
* pytorch 1.0.1 <
* NVIDIA GPU + CUDA cuDNN (optional)

## Installation
* clone this repo 
```
git clone https://github.com/sujaykhandekar/Automated-objects-removal-inpainter.git
cd Automated-objects-removal-inpainter
```
or alternately download zip file.
* install pytorch with this command
```
conda install pytorch==1.5.1 torchvision==0.6.1 -c pytorch
```
* install other python requirements using this command
```
pip install -r requirements.txt
```
* Install one of the three pretrained Edgeconnect model and copy them in ./checkpoints directory  
[Plcaes2](https://drive.google.com/drive/folders/1qjieeThyse_iwJ0gZoJr-FERkgD5sm4y?usp=sharing) (option 1)
[CelebA](https://drive.google.com/drive/folders/1nkLOhzWL-w2euo0U6amhz7HVzqNC5rqb) (option 2)
[Paris-street-view](https://drive.google.com/drive/folders/1cGwDaZqDcqYU7kDuEbMXa9TP3uDJRBR1) (option 3)

## Prediction/Test

A modified version of <em> Automated-Objects-Removal-Inpainter </em> is implemented in the <em> avatarRedaction.ipynb </em> notebook which takes in a video input of mp4 format and applies the redaction / inpainting model to each frame.
The output is a video with human avatars human removed from the footage.

After all packages have been installed to the desired environment, move over to the  <em> avatarRedaction </em> notebook and run the code cells. 

For quick prediction you can run this command. If you don't have cuda/gpu please run the second command.
```
python test.py --input ./examples/my_small_data --output ./checkpoints/resultsfinal --remove 3 15
```
It will take sample images in the ./examples/my_small_data  directory and will create and produce result in directory ./checkpoints/resultsfinal. You can replace these input /output directories with your desired ones.
numbers after --remove specifies objects to be removed in the images. ABove command will remove 3(bird) and 15(people) from the images. Check segmentation-classes.txt for all removal options along with it's number.

Output images will all be 256x256. It takes around 10 minutes for 1000 images on NVIDIA GeForce GTX 1650

for better quality but slower runtime you can use  this command
```
python test.py --input ./examples/my_small_data --output ./checkpoints/resultsfinal --remove 3 15 --cpu yes
```
It will run the segmentation model on cpu. It will be 5 times slower than on gpu (default)
For other options including different segmentation model and EdgeConnect parameters to change please make corresponding modifications in .checkpoints/config.yml file

## training
For training your own segmentation model you can refer to this [repo](https://github.com/CSAILVision/semantic-segmentation-pytorch) and replace .src/segmentor_fcn.py with your model.

For training Edgeconnect model plaese refer to orignal [EdgeConnect repo](https://github.com/knazeri/edge-connect)  after training you can copy your model weights in .checkpoints/ 

## some results
<img src="https://user-images.githubusercontent.com/31131069/89246127-6a391700-d5d8-11ea-85a3-20d65ab3a571.png" width="23%"></img> <img src="https://user-images.githubusercontent.com/31131069/89245762-8b4d3800-d5d7-11ea-89f6-16c21142b2bd.png" width="23%"></img>
<video width="426" src="examples\source\shopping2.mp4" controls title="Title"></video> <video width="426" src="examples\output\output-shopping2.mp4 " controls title="Title">

## License
Licensed under a [Creative Commons Attribution-NonCommercial 4.0 International.](https://creativecommons.org/licenses/by-nc/4.0/)

Except where otherwise noted, this content is published under a [CC BY-NC](https://github.com/knazeri/edge-connect) license, which means that you can copy, remix, transform and build upon the content as long as you do not use the material for commercial purposes and give appropriate credit and provide a link to the license.

## Citation
```
@inproceedings{nazeri2019edgeconnect,
  title={EdgeConnect: Generative Image Inpainting with Adversarial Edge Learning},
  author={Nazeri, Kamyar and Ng, Eric and Joseph, Tony and Qureshi, Faisal and Ebrahimi, Mehran},
  journal={arXiv preprint},
  year={2019},
}

@InProceedings{Nazeri_2019_ICCV,
  title = {EdgeConnect: Structure Guided Image Inpainting using Edge Prediction},
  author = {Nazeri, Kamyar and Ng, Eric and Joseph, Tony and Qureshi, Faisal and Ebrahimi, Mehran},
  booktitle = {The IEEE International Conference on Computer Vision (ICCV) Workshops},
  month = {Oct},
  year = {2019}
}
```
