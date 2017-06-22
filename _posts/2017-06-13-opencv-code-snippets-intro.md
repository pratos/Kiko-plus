---
layout: post
published: true
title: Understanding OpenCV - Code Snippets
---


I made a jump directly to Computer Vision by using Deep Learning, mostly using CNNs & Keras. CNN & Keras are like Adam West's Batman & Burt Ward's Robin, much simpler & fun days of the bat vigilante.

<span>&nbsp;</span>
<div align="center"><img src="https://www.themarysue.com/wp-content/uploads/2016/08/Batman-and-Robin.jpg" height="600px" width="400px"/></div>
<div align="center"><h4>Source: The Mary Sue</h4></div>

But with a lot of image pre-processing (apart from Keras' own functions) crucial to the final output, I searched for a good library to do it. `skimage` is a great library for doing image pre-processing. It packs enough to get you started, but the most widely used is `OpenCV`.

`OpenCV` has a bit of steep learning curve, but once you get used to it there's no feeling like any other. While preparing this notebook, I never intended it to be a blogpost. Things started off with creating snippets off the official library & with inputs from blogposts by [PyImageSearch](http://www.pyimagesearch.com/) which is frankly the most comprehensive blog for Computer Vision. Let's dive into the actual notebook.

***

### Downloading the image


```python
!curl "https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/screenshot1.png" > screeshot.png
```

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 74844  100 74844    0     0  73184      0  0:00:01  0:00:01 --:--:-- 78866



```python
!ls
```

    Amazon Kaggle EDA.ipynb			      Pre Processing Notebook.ipynb
    Image Processing using OpenCV - Part 1.ipynb  screeshot.png


***
### Import the image through OpenCV 


```python
%matplotlib inline
import matplotlib.pyplot as plt
import signal
import numpy as np
import cv2
```

- Open an image using cv2.imread()
    * Import a color image: cv2.IMREAD_COLOR (arg = 1)
    * Import a color image: cv2.IMREAD_GREYSCALE (arg = 0)
    * Import a color image: cv2.IMREAD_UNCHANGED (arg = -1)


```python
screen_img = cv2.imread('./screeshot.png', 1)
```


```python
plt.imshow(screen_img)
plt.xticks([]), plt.yticks([])  # to hide tick values on X and Y axis
plt.show()
```


![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_8_0.png)


### Drawing shapes on an image


```python
# Create a black image
img = np.zeros((512,512,3), np.uint8)
```


```python
# Draw a diagonal white line with thickness of 6 px
cv2.line(img, (0,0),(511,511),(255,255,255),6)
```




    array([[[255, 255, 255],
            [255, 255, 255],
            [255, 255, 255],
            ..., 
            [  0,   0,   0],
            [  0,   0,   0],
            [  0,   0,   0]],
    
           [[255, 255, 255],
            [255, 255, 255],
            [255, 255, 255],
            ..., 
            [  0,   0,   0],
            [  0,   0,   0],
            [  0,   0,   0]],
    
           [[255, 255, 255],
            [255, 255, 255],
            [255, 255, 255],
            ..., 
            [  0,   0,   0],
            [  0,   0,   0],
            [  0,   0,   0]],
    
           ..., 
           [[  0,   0,   0],
            [  0,   0,   0],
            [  0,   0,   0],
            ..., 
            [255, 255, 255],
            [255, 255, 255],
            [255, 255, 255]],
    
           [[  0,   0,   0],
            [  0,   0,   0],
            [  0,   0,   0],
            ..., 
            [255, 255, 255],
            [255, 255, 255],
            [255, 255, 255]],
    
           [[  0,   0,   0],
            [  0,   0,   0],
            [  0,   0,   0],
            ..., 
            [255, 255, 255],
            [255, 255, 255],
            [255, 255, 255]]], dtype=uint8)




```python
plt.imshow(img)
```




    <matplotlib.image.AxesImage at 0x7f3c2ec681d0>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_12_1.png)



```python
# cv2.rectangle(image, dim1-coordinates, dim2-coordinates, color, px size)
cv2.rectangle(img, (250, 250), (300, 300), (255,255,255), 4)
```




    array([[[255, 255, 255],
            [255, 255, 255],
            [255, 255, 255],
            ..., 
            [  0,   0,   0],
            [  0,   0,   0],
            [  0,   0,   0]],
    
           [[255, 255, 255],
            [255, 255, 255],
            [255, 255, 255],
            ..., 
            [  0,   0,   0],
            [  0,   0,   0],
            [  0,   0,   0]],
    
           [[255, 255, 255],
            [255, 255, 255],
            [255, 255, 255],
            ..., 
            [  0,   0,   0],
            [  0,   0,   0],
            [  0,   0,   0]],
    
           ..., 
           [[  0,   0,   0],
            [  0,   0,   0],
            [  0,   0,   0],
            ..., 
            [255, 255, 255],
            [255, 255, 255],
            [255, 255, 255]],
    
           [[  0,   0,   0],
            [  0,   0,   0],
            [  0,   0,   0],
            ..., 
            [255, 255, 255],
            [255, 255, 255],
            [255, 255, 255]],
    
           [[  0,   0,   0],
            [  0,   0,   0],
            [  0,   0,   0],
            ..., 
            [255, 255, 255],
            [255, 255, 255],
            [255, 255, 255]]], dtype=uint8)




```python
plt.imshow(img)
```




    <matplotlib.image.AxesImage at 0x7f3c2ec48e10>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_14_1.png)


### Accessing Image properties


```python
img.shape
```




    (512, 512, 3)




```python
type(img)
```




    numpy.ndarray



### Image ROI (Region of Image)


```python
# Getting google logo
logo = screen_img[200:400, 500:900]
```


```python
plt.imshow(logo)
```




    <matplotlib.image.AxesImage at 0x7f3c2e5264e0>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_20_1.png)


### Image arithmatic


```python
x = np.uint8([250])
```


```python
x
```




    array([250], dtype=uint8)




```python
y = np.uint8([10])
y
```




    array([10], dtype=uint8)




```python
x+y
```




    array([4], dtype=uint8)




```python
cv2.add(x, y)
```




    array([[255]], dtype=uint8)



There is a difference between OpenCV addition and Numpy addition. OpenCV addition is a saturated operation while Numpy addition is a modulo operation.

### Blending 2 images

It is a type of Image addition, but different weights are provided to the pixels (to add opaqueness/transparency).

The equation is:

$g(x)\;=\;(1-\alpha)f_{0}(x)\;+\;{\alpha}f_{1}(x)$

Varying $\alpha$ from 0 $\rightarrow$ 1, we can change the blending.

The operation is performed using `cv2.addWeighted()`.


```python
! wget "http://www.satupedia.com/wp-content/uploads/2017/03/arsenalb40ddb51f5f44099ae80dc5d7e1c59880524d72a24e0d4033ef4c60a39c7dcf1_large.jpg"
```

    --2017-06-15 00:04:49--  http://www.satupedia.com/wp-content/uploads/2017/03/arsenalb40ddb51f5f44099ae80dc5d7e1c59880524d72a24e0d4033ef4c60a39c7dcf1_large.jpg
    Resolving www.satupedia.com (www.satupedia.com)... 45.32.102.146
    Connecting to www.satupedia.com (www.satupedia.com)|45.32.102.146|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 57840 (56K) [image/jpeg]
    Saving to: ‘arsenalb40ddb51f5f44099ae80dc5d7e1c59880524d72a24e0d4033ef4c60a39c7dcf1_large.jpg’
    
    arsenalb40ddb51f5f4 100%[===================>]  56.48K   286KB/s    in 0.2s    
    
    2017-06-15 00:04:50 (286 KB/s) - ‘arsenalb40ddb51f5f44099ae80dc5d7e1c59880524d72a24e0d4033ef4c60a39c7dcf1_large.jpg’ saved [57840/57840]
    



```python
!wget "http://upload.inven.co.kr/upload/2014/05/08/bbs/i3945135106.jpg"
```

    --2017-06-15 00:04:52--  http://upload.inven.co.kr/upload/2014/05/08/bbs/i3945135106.jpg
    Resolving upload.inven.co.kr (upload.inven.co.kr)... 114.31.34.170
    Connecting to upload.inven.co.kr (upload.inven.co.kr)|114.31.34.170|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 445045 (435K) [image/jpeg]
    Saving to: ‘i3945135106.jpg’
    
    i3945135106.jpg     100%[===================>] 434.61K   290KB/s    in 1.5s    
    
    2017-06-15 00:04:56 (290 KB/s) - ‘i3945135106.jpg’ saved [445045/445045]
    



```python
!mv arsenalb40ddb51f5f44099ae80dc5d7e1c59880524d72a24e0d4033ef4c60a39c7dcf1_large.jpg arsenal.jpg
```


```python
!mv i3945135106.jpg cesc.jpg
```


```python
!ls
```

    Amazon Kaggle EDA.ipynb  Image Processing using OpenCV - Part 1.ipynb
    arsenal.jpg		 Pre Processing Notebook.ipynb
    cesc.jpg		 screeshot.png



```python
img1 = cv2.imread('arsenal.jpg', 1)
img2 = cv2.imread('cesc.jpg', 1)
```


```python
plt.imshow(img1)
```




    <matplotlib.image.AxesImage at 0x7f3c2e4f8240>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_35_1.png)


__Why is this Blue?__

- OpenCV represents RGB images as multi-dimensional NumPy arrays…but in reverse order! This means that images are actually represented in BGR order rather than RGB!

__How to change it?__

- Convert BGR $\rightarrow$ RGB


```python
plt.imshow(cv2.cvtColor(img1, cv2.COLOR_BGR2RGB))
```




    <matplotlib.image.AxesImage at 0x7f3c2c0c2cf8>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_37_1.png)



```python
plt.imshow(cv2.cvtColor(img2, cv2.COLOR_BGR2RGB))
```




    <matplotlib.image.AxesImage at 0x7f3c2c089908>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_38_1.png)



```python
img1.shape
```




    (575, 1024, 3)




```python
img2.shape
```




    (798, 1200, 3)



We have a problem here, so let's resize `img2` using `cv2.resize`.


```python
img2_resize = cv2.resize(img2, (img1.shape[1], img1.shape[0]))
```


```python
plt.imshow(cv2.cvtColor(img2_resize, cv2.COLOR_BGR2RGB))
```




    <matplotlib.image.AxesImage at 0x7f3c2c059898>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_43_1.png)



```python
img2_resize.shape
```




    (575, 1024, 3)




```python
blended = cv2.addWeighted(img1, 0.3, img2_resize, 0.7, 0)
```


```python
plt.imshow(cv2.cvtColor(blended, cv2.COLOR_BGR2RGB))
```




    <matplotlib.image.AxesImage at 0x7f3c1ff83d30>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_46_1.png)


Who needs photoshop now! Just kidding, but it is fun little way to do interesting things in OpenCV. We'll take a look at it more.

### Bitwise operations

Next up we would try to extract the Google Logo from image `img`, resize it and put it on top of the `blended` image


```python
# Finding the Region of Image for the logo that we already have!
plt.imshow(logo)
```




    <matplotlib.image.AxesImage at 0x7f3c1ff4c1d0>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_50_1.png)



```python
rows, cols, channels = logo.shape
roi = blended[0:rows, 0:cols] #Putting it in the left hand side of the image
```


```python
plt.imshow(roi)
```




    <matplotlib.image.AxesImage at 0x7f3c1ff1a438>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_52_1.png)



```python
# Convert it to color first
logo = cv2.cvtColor(logo, cv2.COLOR_BGR2RGB)
```


```python
plt.imshow(logo)
```




    <matplotlib.image.AxesImage at 0x7f3c1fee7ef0>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_54_1.png)



```python
logo2gray = cv2.cvtColor(logo,cv2.COLOR_BGR2GRAY)
```


```python
plt.imshow((logo2gray), cmap='gray')
```




    <matplotlib.image.AxesImage at 0x7f3c1feba828>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_56_1.png)


Doesn't give anything, well threshold are hard to determine manually


```python
# initialize the list of threshold methods
methods = [
	("THRESH_BINARY", cv2.THRESH_BINARY),
	("THRESH_BINARY_INV", cv2.THRESH_BINARY_INV),
	("THRESH_TRUNC", cv2.THRESH_TRUNC),
	("THRESH_TOZERO", cv2.THRESH_TOZERO),
	("THRESH_TOZERO_INV", cv2.THRESH_TOZERO_INV)]
 
# loop over the threshold methods
for (threshName, threshMethod) in methods:
	# threshold the image and show it
	(T, thresh) = cv2.threshold(logo2gray, 10, 255, threshMethod)
	cv2.imshow(threshName, thresh)
	cv2.waitKey(0)
```

If you see, the THRESH_TOZERO is way better so we'll use that.


```python
#Create its mask
# cv2.threshold(src, thresh, maxval, type)
ret, mask = cv2.threshold(logo2gray, 200, 255, cv2.THRESH_BINARY_INV)
print(ret)
print("--------------------")
print(plt.imshow(mask))
```

    200.0
    --------------------
    AxesImage(54,36;334.8x217.44)



![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_60_1.png)



```python
mask_inv = cv2.bitwise_not(mask)
print(plt.imshow(mask))
```

    AxesImage(54,36;334.8x217.44)



![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_61_1.png)



```python
# Blackout the area of logo in ROI
logo_bg = cv2.bitwise_and(roi, roi, mask=mask_inv)
```


```python
logo_b = cv2.bitwise_and(roi, roi, mask=mask)
```


```python
plt.imshow(logo_bg)
```




    <matplotlib.image.AxesImage at 0x7f3c1fdaf160>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_64_1.png)



```python
plt.imshow(logo_b)
```




    <matplotlib.image.AxesImage at 0x7f3c1fd7b9e8>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_65_1.png)



```python
# Take only region of logo from logo image.
img2_fg = cv2.bitwise_and(logo_bg,logo_bg,mask = mask)
```


```python
img2_fg.shape
```




    (200, 400, 3)




```python
plt.imshow(img2_fg)
```




    <matplotlib.image.AxesImage at 0x7f3c1fcce208>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_68_1.png)



```python
final = cv2.add(img2_fg, logo_bg)
plt.imshow(cv2.cvtColor(final, cv2.COLOR_BGR2RGB))
```




    <matplotlib.image.AxesImage at 0x7f3c1fc9dcc0>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_69_1.png)


Adding the logo to our blended image (inside the ROI)


```python
blended[0:rows, 0:cols] = final
```


```python
plt.imshow(cv2.cvtColor(blended, cv2.COLOR_BGR2RGB))
```




    <matplotlib.image.AxesImage at 0x7f3c1fc6f3c8>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_72_1.png)


## Image Processing

### Changing Colorspaces

Convert images from one color space to another, BGR $\leftrightarrow$ Gray or BGR $\leftrightarrow$ HSV. Useful, when extracting a colored image from a video feed or image. 

__Reads:__
1. [HSV & HSL](https://en.wikipedia.org/wiki/HSL_and_HSV)
2. [Why HSV for object detection](https://www.thoughtco.com/what-is-hsv-in-design-1078068)


```python
! curl "https://fsmedia.imgix.net/9f/50/d1/5b/6c4e/419a/800e/e942305776e7/imagenes-de-power-rangers-furia-animaljpg.jpeg" > power_rangers.jpg
```

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100  339k  100  339k    0     0   225k      0  0:00:01  0:00:01 --:--:--  285k



```python
color_sample = cv2.imread('power_rangers.jpg')
plt.imshow(cv2.cvtColor(color_sample, cv2.COLOR_BGR2RGB))
```




    <matplotlib.image.AxesImage at 0x7f3c1fe27fd0>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_76_1.png)


What we have to do here is to detect the Red Power Ranger. To detect the color, we need to define the colors in HSV (define boundary values).


```python
boundaries = [([0, 0, 50], [30, 40, 255]), ([86, 31, 4], [220, 88, 50]), ([25, 146, 190], [62, 174, 250]), \
             ([103, 86, 65], [145, 133, 128])]
```


```python
boundaries[0][0]
```




    [0, 0, 50]




```python
# We need to convert the boundaries to numpy arrays
lower = np.array(boundaries[0][0], dtype='uint8')
upper = np.array(boundaries[0][1], dtype='uint8')
```


```python
# We'll find the mask
mask = cv2.inRange(color_sample, lower, upper)
```


```python
plt.imshow(mask)
```




    <matplotlib.image.AxesImage at 0x7f3c1fe65b70>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_82_1.png)



```python
output = cv2.bitwise_and(color_sample, color_sample, mask = mask)
```


```python
plt.imshow(cv2.cvtColor(output, cv2.COLOR_BGR2RGB))
```




    <matplotlib.image.AxesImage at 0x7f3c1fc35cf8>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_84_1.png)


After the red ranger, let's try to find out the blue ranger.


```python
blue_lower = np.array([86, 20, 4], dtype='uint8')
blue_upper = np.array([255, 120, 120], dtype='uint8')
```


```python
mask_blue = cv2.inRange(color_sample, blue_lower, blue_upper)
```


```python
plt.imshow(mask_blue)
```




    <matplotlib.image.AxesImage at 0x7f3c1fb84588>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_88_1.png)



```python
output_blue = cv2.bitwise_and(color_sample, color_sample, mask=mask_blue)
```


```python
plt.imshow(cv2.cvtColor(output_blue, cv2.COLOR_BGR2RGB))
```




    <matplotlib.image.AxesImage at 0x7f3c1fb6e0f0>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_90_1.png)


### Image Thresholding

In the 1st example (the Arsenal blending), we saw how image can be thresholded manually. In this we'll look at other means of thresholding.

- __Adaptive Thresholding__

Using a global value as a threshold doesn't cut out for real world applications. There are various factors that we need to look in and understand before understanding things. 

The algorithm calculate the threshold for a small regions of the image. So we get different thresholds for different regions of the same image and it gives us better results for images with varying illumination.

It has three ‘special’ input params and only one output argument.

* __Adaptive Method - It decides how thresholding value is calculated.__
    * cv2.ADAPTIVE_THRESH_MEAN_C : threshold value is the mean of neighbourhood area.
    * cv2.ADAPTIVE_THRESH_GAUSSIAN_C : threshold value is the weighted sum of neighbourhood values where weights are a gaussian window.

* __Block Size - It decides the size of neighbourhood area.__
* __C - It is just a constant which is subtracted from the mean or weighted mean calculated.__

Let's look at an example:


```python
!curl "https://upload.wikimedia.org/wikipedia/commons/0/0b/ReceiptSwiss.jpg" > receipt.jpg
```

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100  940k  100  940k    0     0   244k      0  0:00:03  0:00:03 --:--:--  252k



```python
receipt = cv2.imread('receipt.jpg')
```


```python
plt.rcParams["figure.figsize"] = (60,10)
plt.imshow(receipt)
```




    <matplotlib.image.AxesImage at 0x7f3c1fadda20>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_96_1.png)



```python
receipt = cv2.medianBlur(receipt, 5)
```


```python
receipt = cv2.cvtColor(receipt, cv2.COLOR_BGR2GRAY)
```


```python
# Using gloal threshold
ret, th1 = cv2.threshold(receipt, 127, 255, cv2.THRESH_BINARY)
```


```python
th2 = cv2.adaptiveThreshold(receipt, 255, cv2.ADAPTIVE_THRESH_MEAN_C, cv2.THRESH_BINARY,115,2)
```


```python
th3 = cv2.adaptiveThreshold(receipt,255,cv2.ADAPTIVE_THRESH_GAUSSIAN_C, cv2.THRESH_BINARY,115,2)
```


```python
titles = ['Original Image', 'Global Thresholding (v = 127)',
            'Adaptive Mean Thresholding', 'Adaptive Gaussian Thresholding']
```


```python
images = [receipt, th1, th2, th3]
```


```python
plt.rcParams["figure.figsize"] = (60,10)
plt.imshow(th1, cmap='gray')
```




    <matplotlib.image.AxesImage at 0x7f3c1f9af4e0>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_104_1.png)



```python
plt.rcParams["figure.figsize"] = (60,10)
plt.imshow(th2, cmap='gray')
```




    <matplotlib.image.AxesImage at 0x7f3c1f918be0>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_105_1.png)



```python
plt.rcParams["figure.figsize"] = (60,10)
plt.imshow(th3, cmap='gray')
```




    <matplotlib.image.AxesImage at 0x7f3c1f8871d0>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_106_1.png)


### Geometric Transformation of Images

- __Scaling__

Scaling is just resizing of the image. OpenCV comes with a function cv2.resize() for this purpose. The size of the image can be specified manually, or you can specify the scaling factor.


```python
!curl "http://vignette4.wikia.nocookie.net/dragonball/images/4/4b/VegetaItsOver9000-02.png/revision/latest?cb=20100724145819" > vegeta.png
```

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100  310k  100  310k    0     0    99k      0  0:00:03  0:00:03 --:--:--  129k



```python
vegeta = cv2.imread('vegeta.png')
```


```python
vegeta = cv2.cvtColor(vegeta, cv2.COLOR_BGR2RGB)
```


```python
plt.rcParams["figure.figsize"] = (17,8)
plt.imshow(vegeta)
```




    <matplotlib.image.AxesImage at 0x7f3c1f86b3c8>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_112_1.png)


Vegeta is angry coz Goku's power levels are __OVER 9000!__ and also the image is too big. Let's do transform to resize him.


```python
# Image shape::
vegeta.shape
```




    (480, 640, 3)



We'll reduce the dimensions to 200x300, keeping the channels same.


```python
re1 = cv2.resize(vegeta, (200, 100), interpolation=cv2.INTER_CUBIC)
```


```python
plt.imshow(re1)
```




    <matplotlib.image.AxesImage at 0x7f3c1f7d2630>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_117_1.png)



```python
re1.shape
```




    (100, 200, 3)




```python
re2 = cv2.resize(vegeta, (200, 100), interpolation=cv2.INTER_AREA)
plt.imshow(re1)
```




    <matplotlib.image.AxesImage at 0x7f3c1f741748>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_119_1.png)



```python
re2 = cv2.resize(vegeta, (200, 100), interpolation=cv2.INTER_LINEAR)
plt.imshow(re1)
```




    <matplotlib.image.AxesImage at 0x7f3c1f72f4e0>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_120_1.png)


Looks more scary now though!

### Translation

Translation is the shifting of object’s location. If you know the shift in (x,y) direction, let it be $(t_{x},t_{y})$, you can create the transformation matrix $\textbf{M}$ as follows:

$M = \begin{bmatrix} 1 & 0 & t_x \\ 0 & 1 & t_y  \end{bmatrix}$


```python
M = np.float32([[1,0,100],[0,1,160]])
```


```python
final_form = cv2.warpAffine(vegeta, M, (vegeta.shape[0], vegeta.shape[1]))
```


```python
plt.imshow(final_form)
```




    <matplotlib.image.AxesImage at 0x7f3c1f0e8048>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_126_1.png)


### Affine Transformation

In affine transformation, all parallel lines in the original image will still be parallel in the output image.

### Perspective Transformation

A great read would be - [this blog](http://www.pyimagesearch.com/2014/08/25/4-point-opencv-getperspective-transform-example/). Explains how perspective transformation works. We have seen Perspective transformation used in document scanners on our phones, neat application.

### Smoothing Images

- To blur images using low pass filters
- Applying custom made filters to images (2D Convolution)

__2D Convolution or Image Filtering__

`cv2.filter2D()` to convolve an image!

>Definition of `convolution`: _coil or twist_. 

>Mathematically, convolution is a mathematical operation on two functions (f and g); it produces a third function, that is typically viewed as a modified version of one of the original functions, giving the integral of the pointwise multiplication of the two functions as a function of the amount that one of the original functions is translated.

<div align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/2/21/Comparison_convolution_correlation.svg" height="400px;" width="400px;"/></div>
<span>&nbsp;</span>
<span>&nbsp;</span>

In _Image processing_, we would see how `convolution` works!

Consider the Google Logo:


```python
plt.rcParams["figure.figsize"] = (10,3)
plt.imshow(logo)
```




    <matplotlib.image.AxesImage at 0x7f3c1f0507f0>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_134_1.png)


We always need to define a _kernel_, it is a small tool that moves through the entire image so that we get the required transformed image. Read this [Setosa.io](http://setosa.io/ev/image-kernels/) blog that explains _kernels_ in an intuitive way


```python
kernel = np.ones((5,5), np.float32)/25
print(kernel)
```

    [[ 0.04  0.04  0.04  0.04  0.04]
     [ 0.04  0.04  0.04  0.04  0.04]
     [ 0.04  0.04  0.04  0.04  0.04]
     [ 0.04  0.04  0.04  0.04  0.04]
     [ 0.04  0.04  0.04  0.04  0.04]]



```python
# Applying the Kernel over the logo, simple box blur
logo_blur = cv2.filter2D(logo, -1, kernel)
```


```python
plt.rcParams["figure.figsize"] = (10,3)
plt.imshow(logo_blur)
```




    <matplotlib.image.AxesImage at 0x7f3c1efca198>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_138_1.png)


__Image Blurring__

This is useful to remove the noise. Removes the high frequency content(noise, edges) from images, resulting in edges being blurred when filter is applied.

There are various types of blurring techniques.

- __Averaging:__
    
    Done by taking the average of all the pixels under kernel area and replaces the central element with this average. This is done by the function `cv2.blur()` or `cv2.boxFilter()`.


```python
logo_avg_blur = cv2.blur(logo, (6,6))
plt.rcParams["figure.figsize"] = (10,3)
plt.imshow(logo_avg_blur)
```




    <matplotlib.image.AxesImage at 0x7f3c1efbd7b8>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_140_1.png)


- __Gaussian Blur:__
    
    Below is a Gaussian Kernel ([source](http://www.aishack.in/tutorials/image-convolution-examples/))
    
    <div align="center"><img src="http://www.aishack.in/static/img/tut/conv-gaussian-blur.jpg" /></div>


```python
logo_gauss = cv2.GaussianBlur(logo, (5,5), 0)
plt.rcParams["figure.figsize"] = (10,3)
plt.imshow(logo_gauss)
```




    <matplotlib.image.AxesImage at 0x7f3c1ef35198>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_142_1.png)


- __Median Filtering:__
    
    Computes the median of all the pixels under the kernel window & the central pixel is replaced by the median value. Highly effective in removing salt-and-pepper noise. 
    
We'll first download an image having salt-and-pepper noise:


```python
!curl "https://upload.wikimedia.org/wikipedia/commons/f/f4/Noise_salt_and_pepper.png" > sp.png
```

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 82829  100 82829    0     0  39024      0  0:00:02  0:00:02 --:--:-- 51703



```python
sp = cv2.imread('sp.png')
sp_median = cv2.medianBlur(sp,5)

plt.rcParams["figure.figsize"] = (20,7)
plt.subplot(1,2,1),plt.imshow(sp),plt.title('Salt & Pepper')
plt.xticks([]), plt.yticks([])
plt.subplot(1,2,2),plt.imshow(sp_median),plt.title('Processed')
plt.xticks([]), plt.yticks([])
plt.tight_layout()
plt.show()
```


![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_145_0.png)


- __Bilateral Filtering:__
    
    The previous 2 approaches remove the noise as well as the edges. _Bilateral Filtering_ does the noise removal, but it keeps the edges. We'll compare the three images (with noise, Gaussian & Bilateral) side by side, to check the differences.
    
Downloading the image:


```python
!curl "https://upload.wikimedia.org/wikipedia/commons/d/d2/512x512-Gaussian-Noise.jpg" > gauss.jpg
```

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 94314  100 94314    0     0  50518      0  0:00:01  0:00:01 --:--:-- 65541



```python
gauss = cv2.imread('gauss.jpg')
gauss_gauss = cv2.GaussianBlur(gauss, (5,5), 0)

#bilateralFilter(input array (image), output array, diameter of pixel neighbour hood, sigmaColor, sigmaSpace)
gauss_bilateral = cv2.bilateralFilter(gauss, 9, 75, 75)
```


```python
plt.rcParams["figure.figsize"] = (15,6)
plt.subplot(1,3,1), plt.imshow(gauss),plt.title('Original with noise')
plt.xticks([]), plt.yticks([])
plt.subplot(1,3,2), plt.imshow(gauss_gauss),plt.title('Gaussian Filter')
plt.xticks([]), plt.yticks([])
plt.subplot(1,3,3), plt.imshow(gauss_bilateral),plt.title('Bilateral Filter')
plt.xticks([]), plt.yticks([])
plt.show()
```


![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_149_0.png)


As you ca see, the _Bilateral Filter_ doesn't have hazy edges like the _Gaussian_.

### Morphological Transformation

Morphological Transformations are basically playing with the shape of the original image, manipulating the internals of the image. There are 2 major operations: __Erosion__ & __Dilation__.

- __Erosion:__

    Similar to the erosion of banks of a river, we try to erode away boundaries of the foreground object.
    
    ![Erosion](http://www.sciencehq.com/earth/images/soon-to-be-oxbow-lake.jpg)

The kernel slides through the image (as in 2D convolution). A pixel in the original image (either 1 or 0) will be considered 1 only if all the pixels under the kernel is 1, otherwise it is eroded (made to zero). It is advisable to have the foreground as white (for reasons above).


```python
!curl "http://pad1.whstatic.com/images/thumb/e/ef/Divide-Double-Digits-Step-9-Version-5.jpg/aid281771-v4-728px-Divide-Double-Digits-Step-9-Version-5.jpg" > digits.png
```

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 46846  100 46846    0     0  48692      0 --:--:-- --:--:-- --:--:-- 50644



```python
digits = cv2.imread('digits.png')
plt.imshow(digits)
```




    <matplotlib.image.AxesImage at 0x7f3c1ee0c748>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_155_1.png)



```python
digits = cv2.cvtColor(digits, cv2.COLOR_BGR2GRAY)
plt.imshow(digits, cmap=plt.get_cmap('gray'))
```




    <matplotlib.image.AxesImage at 0x7f3c1ed45898>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_156_1.png)



```python
ret, mask = cv2.threshold(digits, 200, 255, cv2.THRESH_BINARY_INV)
plt.imshow(mask, cmap='gray')
```




    <matplotlib.image.AxesImage at 0x7f3c1ee77fd0>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_157_1.png)



```python
kernel = np.ones((5,5), np.uint8)
eroded = cv2.erode(mask, kernel, iterations=1)
plt.imshow(eroded, cmap='gray')
```




    <matplotlib.image.AxesImage at 0x7f3c1edce4a8>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_158_1.png)


- __Dilation:__

    You just make it fat! Remember the phrase, "Dilation of Pupil".
    
    <img src="https://upload.wikimedia.org/wikipedia/commons/a/a3/Eye_dilate.gif" height="200px;" width="300px;"/>


```python
kernel = np.ones((5,5), np.uint8)
dilated = cv2.dilate(mask, kernel, iterations=1)
plt.imshow(dilated, cmap='gray')
```




    <matplotlib.image.AxesImage at 0x7f3c1ecf5cc0>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_160_1.png)


- __Opening & Closing:__
    
    Opening is just another name of erosion followed by dilation. It is useful in removing noise.
    
    Closing is reverse of Opening, Dilation followed by Erosion. It is useful in closing small holes inside the foreground objects, or small black points on the object.
    
    How do we remove noise? If we have white holes in the object as below:


```python
ret, mask = cv2.threshold(sp, 100, 255, cv2.THRESH_BINARY)
plt.imshow(mask, cmap='gray')
```




    <matplotlib.image.AxesImage at 0x7f3c1ec670b8>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_162_1.png)


We'll apply _Opening_ to the image.


```python
opening = cv2.morphologyEx(mask, cv2.MORPH_OPEN, kernel)
plt.imshow(opening, cmap='gray')
```




    <matplotlib.image.AxesImage at 0x7f3c1ebdb4e0>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_164_1.png)


As we can see above, there were still black dots inside the image that weren't taken care of. We'll do that using Closing


```python
closing = cv2.morphologyEx(opening, cv2.MORPH_CLOSE, kernel)
plt.imshow(closing, cmap='gray')
```




    <matplotlib.image.AxesImage at 0x7f3c1eb4d940>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_166_1.png)


The final outcome is disastrous, but you get the point right! Next in line is _Morphological Gradient_.

- __Morphological Gradient__

    Difference between dilation & erosion of an image.


```python
ret, mask = cv2.threshold(digits, 200, 255, cv2.THRESH_BINARY_INV)
kernel = np.ones((3,3), np.uint8)
gradient = cv2.morphologyEx(mask, cv2.MORPH_GRADIENT, kernel)

plt.subplot(1,2,1), plt.imshow(mask, cmap='gray'),plt.title('Original')
plt.xticks([]), plt.yticks([])
plt.subplot(1,2,2), plt.imshow(gradient, cmap='gray'),plt.title('Morphological Gradient')
plt.xticks([]), plt.yticks([])
plt.show()
```


![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_169_0.png)


### Image Gradients 

<div align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/0/0f/Gradient2.svg" height="300px;" width="500px;"/></div>

>Image gradients can be used to extract information from images. Gradient images are created from the original image (generally by convolving with a filter, one of the simplest being the Sobel filter) for this purpose. Each pixel of a gradient image measures the change in intensity of that same point in the original image, in a given direction. To get the full range of direction, gradient images in the x and y directions are computed.

>One of the most common uses is in edge detection. One example of an edge detection algorithm that uses gradients is the Canny edge detector.

Consider the image, `receipt`, which we used previously.


```python
plt.rcParams["figure.figsize"] = (20,10)
plt.imshow(receipt, cmap='gray')
```




    <matplotlib.image.AxesImage at 0x7f3c1ea76c18>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_171_1.png)


__1. Sobel & Scharr Derivatives__

$\textbf{Sobel Operators = Gaussian Smooting + Differentiation Operator}$

In the image at the start of this topic, we can see the directions. These directions are specified by the `yorder` & `xorder` (vertical & horizontal respectively). 

Size of the kernel, `ksize` can be specified (any value, generally if `ksize=5` the kernel is 5x5).

If `ksize=-1`, a Scharr filter is used (?)

__2. Laplacian Derivatives__

Laplacian of the image is given by: $\Delta src = \frac{\partial ^2{src}}{\partial x^2} + \frac{\partial ^2{src}}{\partial y^2}$

Each derivative is found using _Sobel derivatives_.

[Reading](http://www.cse.psu.edu/~rtc12/CSE486/lecture05_6pp.pdf)


```python
plt.rcParams["figure.figsize"] = (20,10)
plt.imshow(receipt,cmap = 'gray')
plt.title('Original'), plt.xticks([]), plt.yticks([])
```




    (<matplotlib.text.Text at 0x7f3c1e9b8ba8>,
     ([], <a list of 0 Text xticklabel objects>),
     ([], <a list of 0 Text yticklabel objects>))




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_173_1.png)



```python
sobelx = cv2.Sobel(receipt, cv2.CV_8U, 1, 0, ksize=3)
plt.rcParams["figure.figsize"] = (20,10)
plt.imshow(sobelx, cmap='gray')
plt.show()
```


![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_174_0.png)



```python
sobely = cv2.Sobel(receipt, cv2.CV_16U, 0, 1, ksize=5)
plt.rcParams["figure.figsize"] = (20,10)
plt.imshow(sobely, cmap='gray')
plt.show()
```


![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_175_0.png)



```python
scharrx = cv2.Sobel(receipt, cv2.CV_16U, 1, 0, ksize=-1)
plt.rcParams["figure.figsize"] = (20,10)
plt.imshow(scharrx, cmap='gray')
plt.show()
```


![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_176_0.png)



```python
scharry = cv2.Sobel(receipt, cv2.CV_16U, 0, 1, ksize=-1)
plt.rcParams["figure.figsize"] = (20,10)
plt.imshow(scharry, cmap='gray')
plt.show()
```


![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_177_0.png)



```python
laplacian = cv2.Laplacian(receipt, cv2.CV_8U)
plt.rcParams["figure.figsize"] = (20,10)
plt.imshow(laplacian, cmap='gray')
plt.show()
```


![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_178_0.png)


### Canny Edge Detection

Canny Edge Detection is a popular edge detection algorithm. It was developed by John F. Canny in 1986. It is a multi-stage algorithm. 

It goes through the following stages:

1. Noise Reduction:
    
    Edge detection, as in the previous few topics, we know that it is succeptible to noise. Canny Edge detector takes care of that.
    
2. Finding Intensity Gradient of the Image. 

    The image is then passed through Sobel filter, both in horizontal & vertical direction.
    
3. Non-maximum suppression

    (Difficult to explain)
    
4. Hysteresis Thresholding

    This stage decides which are edges and which are not. This also removes small pixel noises on the assumption that the edges are along the lines. 


```python
edge_receipt = cv2.Canny(receipt, 150, 300)
plt.imshow(edge_receipt, cmap='gray')
plt.show()
```


![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_180_0.png)


Probably, not a great image to do edge detection. Let's look at detecting road lanes.


```python
!curl "http://www.richmondregional.org/images/monthly_flyer/September_2010_Graphics/DTE_ORT_lanes.jpg" > road_lanes.jpg
```

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100  496k  100  496k    0     0  78521      0  0:00:06  0:00:06 --:--:--  127k



```python
road_lanes = cv2.imread('road_lanes.jpg')
```


```python
road_lanes = cv2.cvtColor(road_lanes, cv2.COLOR_BGR2GRAY)
plt.imshow(road_lanes, cmap='gray')
```




    <matplotlib.image.AxesImage at 0x7f3c1e88a6a0>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_184_1.png)



```python
road_canny = cv2.Canny(road_lanes, 100,200)
plt.rcParams['figure.figsize'] = (15,7)
plt.imshow(road_canny, cmap='gray')
plt.show()
```


![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_185_0.png)


### Contours 

Contours by definition: _An outline representing or bounding the shape or form of something._
    
In the above road lane detection, we got the threholded image. To create a boundary around it, we need the help of contours. [More reading](http://www.aishack.in/tutorials/introduction-contours/)

[Berkley Computer Vision Group](https://www2.eecs.berkeley.edu/Research/Projects/CS/vision/grouping/index2.html)


```python
!curl "https://www.echalk.co.uk/amusements/Games/Tetrominoes/shareIcons/shareIcon.jpg" > tetris.jpg
```

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 30401  100 30401    0     0  17879      0  0:00:01  0:00:01 --:--:-- 19740



```python
tetris = cv2.imread('tetris.jpg')
tetris_gray = cv2.cvtColor(tetris, cv2.COLOR_BGR2GRAY)
ret, mask = cv2.threshold(tetris_gray, 200, 255, cv2.THRESH_BINARY_INV)
plt.imshow(mask, cmap='gray')
```




    <matplotlib.image.AxesImage at 0x7f3c1eb1a470>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_188_1.png)


We'll get the blue blocks, for that we need to use `cv2.inRange()`


```python
tetris_color = cv2.cvtColor(tetris, cv2.COLOR_BGR2RGB)
```


```python
blue_lower = np.array([50, 0, 0], dtype='uint8')
blue_upper = np.array([255, 0, 0], dtype='uint8')
```


```python
mask_blue = cv2.inRange(tetris_color, blue_lower, blue_upper)
```


```python
plt.imshow(mask_blue)
```




    <matplotlib.image.AxesImage at 0x7f3c1e790710>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_193_1.png)



```python
output = cv2.bitwise_and(tetris_color, tetris_color, mask = mask_blue)
```


```python
plt.imshow(output)
```




    <matplotlib.image.AxesImage at 0x7f3c1e779e80>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_195_1.png)



```python
output = cv2.cvtColor(output, cv2.COLOR_BGR2RGB)
plt.imshow(output)
```




    <matplotlib.image.AxesImage at 0x7f3c1e6e84e0>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_196_1.png)



```python
output_gray = cv2.cvtColor(output, cv2.COLOR_RGB2GRAY)
plt.imshow(output_gray, cmap='gray')
```




    <matplotlib.image.AxesImage at 0x7f3c1e651b00>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_197_1.png)



```python
_, contours, hierarchy  = cv2.findContours(output_gray.copy(), cv2.RETR_LIST, cv2.CHAIN_APPROX_SIMPLE)
```


```python
cont_digits = cv2.drawContours(output.copy(), contours, -1, (0, 255, 0), 3)
```


```python
plt.imshow(cont_digits, cmap='gray')
plt.show()
```


![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_200_0.png)


### Contour Features

In statistics, Moments are the quantitative measures to define data points. The moments defined are:

- Total Probability (Zeroth moment)
- Mean (1st moment)
- Variance (2nd moment)
- Skewness (3rd moment)
- Kurtosis (4th moment)

Image moments helps us to create features: center of mass of object, area of the object.

We already have to contours from the above image, calculating the moments.


```python
!curl "http://vignette3.wikia.nocookie.net/marvel_dc/images/d/df/Flash_Logo_01.png/revision/latest?cb=20140529051349" > flash.png
```

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100  143k  100  143k    0     0   148k      0 --:--:-- --:--:-- --:--:--  161k



```python
flash = cv2.imread('flash.png')
flash = cv2.cvtColor(flash, cv2.COLOR_BGR2RGB)
plt.imshow(flash)
```




    <matplotlib.image.AxesImage at 0x7f3c1e5ad7b8>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_204_1.png)



```python
flash_gray = cv2.cvtColor(flash, cv2.COLOR_RGB2GRAY)
ret, mask = cv2.threshold(flash_gray, 150, 255, cv2.THRESH_BINARY_INV)
plt.imshow(mask, cmap='gray')
```




    <matplotlib.image.AxesImage at 0x7f3c1e526080>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_205_1.png)



```python
_, contours, hierarchy  = cv2.findContours(mask.copy(), cv2.RETR_LIST, cv2.CHAIN_APPROX_SIMPLE)
```


```python
cont_flash = cv2.drawContours(flash.copy(), contours[6], -1, (0, 255, 0), 3)
```


```python
plt.imshow(cont_flash, cmap='gray')
```




    <matplotlib.image.AxesImage at 0x7f3c1e49a5c0>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_208_1.png)


__NOTE:__ The official documentation as well as the popular blogs do not mention this regarding the contours. You need to find the specific contours for the shape in the image. So our _flash_ contour would have a separate set of values here, likewise the circle inside which the _flash_ sign is. 

This is true everywhere, there will be situations where there are more than 100 contour values. God bless us then!


```python
len(contours)
```




    10




```python
cnt = contours[6]
moments = cv2.moments(cnt)
print(moments)
```

    {'nu11': -0.1907517670263885, 'nu30': -0.016394871693779282, 'nu03': 0.01208933681388529, 'mu30': -172121780.97688293, 'm01': 2010266.8333333333, 'nu21': 0.020979023957747672, 'mu02': 34511693.70050317, 'nu20': 0.1388066996783884, 'mu20': 14431539.936564565, 'mu21': 220248565.17384815, 'm10': 2147855.6666666665, 'nu12': -0.02132463374655408, 'm21': 83909645927.7, 'mu12': -223876954.18984604, 'mu11': -19832196.50196892, 'm12': 82711294309.23334, 'm00': 10196.5, 'm02': 430841095.0833333, 'm30': 104252150772.20001, 'm11': 403623205.7916666, 'mu03': 126920065.13383484, 'nu02': 0.33194339092953673, 'm20': 466869529.9166666, 'm03': 98676519466.85}



```python
#To find centroid:
cx = int(moments['m10']/moments['m00'])
cy = int(moments['m01']/moments['m00'])
```


```python
print("The centroid is ({},{})".format(cx,cy))
```

    The centroid is (210,197)



```python
print("The contour area is {}".format(moments['m00']))
```

    The contour area is 10196.5


We can find the contour area using `cv2.contourArea()`


```python
print("The contour area is {}".format(cv2.contourArea(cnt)))
```

    The contour area is 10196.5



```python
# Finding the contour perimeter
perimeter = cv2.arcLength(cnt, True)
print(perimeter)
```

    947.6336801052094


- Contour Approximation


```python
approx = cv2.approxPolyDP(cnt, perimeter, True)
print(approx)
```

    [[[301  52]]]


- Bounding Rectangles
    
    Selecting the bounding box only for the flash!


```python
x, y, w, h = cv2.boundingRect(cnt)
rect = cv2.rectangle(flash.copy(), (x,y), (x+w, y+h), (0,255,0), 5)
plt.imshow(rect)
```




    <matplotlib.image.AxesImage at 0x7f3c1e41c6d8>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_221_1.png)



```python
rect = cv2.minAreaRect(cnt)
box = cv2.boxPoints(rect)
box = np.int0(box)
rect_draw = cv2.drawContours(flash.copy(), [box], 0, (255,0,0), 5)
```


```python
plt.imshow(rect_draw, cmap='gray')
```




    <matplotlib.image.AxesImage at 0x7f3c1e389cf8>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_223_1.png)


- Minimum Enclosing Circle

    Cover the 'flash' sign using a circle.


```python
(x, y), radius = cv2.minEnclosingCircle(cnt)
center = (int(x), int(y))
radius = int(radius)
circle_draw = cv2.circle(flash.copy(), center, radius, (0,255,0), 4)
plt.imshow(circle_draw)
```




    <matplotlib.image.AxesImage at 0x7f3c1e308828>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_225_1.png)


- Fitting an Ellipse

    To fit an ellipse to the _flash_ sign.


```python
ellipse = cv2.fitEllipse(cnt)
draw_ellipse = cv2.ellipse(flash.copy(), ellipse, (0,255,0), 4)
plt.imshow(draw_ellipse)
```




    <matplotlib.image.AxesImage at 0x7f3c1e677048>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_227_1.png)


### Histograms

A histogram represents the distribution of colors in an image. It can be visualized as a graph (or plot) that gives a high-level intuition of the intensity (pixel value) distribution. We are going to assume a RGB color space in this example, so these pixel values will be in the range of 0 to 255.

By looking at the histogram of an image, you get intuition about contrast, brightness, intensity distribution etc of that image. 


```python
!curl "https://media1.britannica.com/eb-media/54/155954-004-4BF4BBF7.jpg" > everest.jpg
```

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 29947  100 29947    0     0  59170      0 --:--:-- --:--:-- --:--:--  117k



```python
everest = cv2.imread('everest.jpg')
everest_gray = cv2.cvtColor(everest, cv2.COLOR_BGR2GRAY)
plt.imshow(everest_gray, cmap='gray')
```




    <matplotlib.image.AxesImage at 0x7f3c1eb7f630>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_230_1.png)


- Plot histograms using just matplotlib


```python
plt.hist(everest_gray.ravel(), 265, [0,256])
plt.show()
```


![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_232_0.png)



```python
color = ('b', 'g', 'r')
for i, col in enumerate(color):
    histr = cv2.calcHist([everest], [i], None, [256], [0,256])
    plt.plot(histr, color=col)
    plt.xlim([0,256])
    
plt.show()
```


![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_233_0.png)


- Multi-Dimensional Histograms


```python
hsv = cv2.cvtColor(everest, cv2.COLOR_BGR2HSV)
plt.imshow(hsv)
```




    <matplotlib.image.AxesImage at 0x7f3c1df17e48>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_235_1.png)



```python
# calcHist(images, channels, mask, histSize, ranges[, hist[, accumulate]])
hist = cv2.calcHist([hsv], [0,1], None, [180, 256], [0, 180, 0, 256])
```


```python
plt.imshow(hist, interpolation='nearest')
```




    <matplotlib.image.AxesImage at 0x7f3c1df033c8>




![png](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/opencv/output_237_1.png)


Applications of histograms still feels as something very obscure at this point of time. Let's continue the discussion after I understand things.

This should get you started off with using OpenCV at a very primary level. Even I've not mastered it, still to cover a lot of ground. Next part would be using all the learnings to create an application or maybe a Kaggle competition dataset for practice.
