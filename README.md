# Progressive Joint Low-light Enhancement and Noise Removal for Raw Images



If you find it useful in your research, please consider citing the following paper:

> ```latex
> @ARTICLE{9730802,
>     author={Lu, Yucheng and Jung, Seung-Won},
>     journal={IEEE Transactions on Image Processing}, 
>     title={Progressive Joint Low-Light Enhancement and Noise Removal for Raw Images}, 
>     year={2022},
>     volume={31},
>     number={},
>     pages={2390-2404},
>     doi={10.1109/TIP.2022.3155948}}
> ```

------



#### Introduction

The main contributions of this work are as follows:

- We present a framework that performs joint enhancement and denoising of low-light images. The proposed framework results in images with higher quality compared to current state-of-the art methods both qualitatively and quantitatively.

- We design a two-branch structure that estimates enhancement parameters in low-resolution via bilateral learning and applies joint enhancement and denoising in full-resolution progressively. This design enables the former to have a large receptive field insensitive to noise while preserving high-resolution features for the latter.

- We propose a strategy that uses several existing datasets developed for enhancement and denoising, respectively. For the coefficient estimation branch, a carefully designed cost function combined with zero-reference loss and high-level perceptual color loss enables weakly supervised network learning. For the joint operation branch, a more accurate noise model estimated using only a few dark image samples is employed in data synthesis.

  

#### System Requirements

- Jupyter Notebook Environment

- pyTorch (tested on v1.6)

- torchvision, torchnet, and Visdom
- rawPy (check [this](https://pypi.org/project/rawpy/) for installation instruction)

- py3exiv2 (check [this](https://stackoverflow.com/questions/41075975/impossible-to-install-py3exiv2-with-pip) for installation instruction)
- colour and colour_demosaicing



#### Evaluation

1. Clone this repository to your local machine, make sure that the pre-trained weights are in the folder "saves".

2. Run the test by loading *"run-demo.ipynb"* in Jupyter Notebook, you may need to specify a new GPU ID if needed:

   ```python
   os.environ["CUDA_VISIBLE_DEVICES"] = '0'
   ```

   A sample image has been included in this repository (in the folder *"samples"*). The processed image can be found in the folder *"results"*.

   You can also test your own raw files (in Adobe DNG format), it is recommended to use the same configuration specified in Section IV when capturing photos. Path to your data should be added accordingly:

   ```python
   data_root = '/samples/cameraModel/dng'	# data path
   save_root = './results'					# result path
   ```

   *This repository provides noise models of 4 devices as referred to the original paper. You need to run the noise model estimation script for other camera models not in the list (see next section).



#### Training from scratch (coming soon...)



#### Raw-to-sRGB Explanation

As the proposed framework generates sRGB image from camera raw data, color space conversion from camera raw to sRGB is required. The simplified pipeline can be decribed as:

**camera raw → CIE XYZ (with D50 white point) → linear sRGB (with D65 white point)**

While the transformation matrix between CIE XYZ and linear sRGB is fixed, the one associated between camera raw and CIE XYZ depends on the camera model and the selected white point, which is interpolated using camera estimated white point and other calibration meta-data coming with the file. A file named *"ISPColor.ipynb"* is provided to cope with all the aforementioned steps, please note that no special treatment is applied to saturated pixels at this moment, so these regions in the results might be incorrect. More details regarding the entire processing procedure can be found in the chapter "Mapping Camera Color Space to CIE XYZ Space" from [Adobe DNG specification](https://helpx.adobe.com/content/dam/help/en/photoshop/pdf/dng_spec_1_6_0_0.pdf).
