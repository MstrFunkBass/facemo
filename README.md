# facemo - making image mosaics
An interactive python notebook that creates a mosaic of images to represent another image

## Contents
* [General Info](#general-info)
* [Technologies](#technologies)
* [Setup](#setup)
* [Customising Images](#customising-images)

## General Info
The aim of this project was to recreate an image of a face by using a number of other images. The way I approached this was to load the image to be recreated, pixelate it in a way to provide a template for the mosaic images (1 pixel = 1 image), then loop through each pixel in the template and find the image with the closest RGB value. The images would then be laid out side by side to recreate the image.

#### Finding the closest colour
Firstly, the mean Red, Green and Blue values are found for each of the mosaic images. Then the scipy module 'spatial' is used to create a KDTree class using the mosaic image mean values, which can be queried to find the closest neighbour of each pixel colour in the pixelated template. The index of these are then stored so that the original image with that mean value can be called to complete the image.

#### Reducing duplicate images
A problem I kept coming across was that many of the pixels in the main face image were very similar in colour, and therefore when querying the KDTree, it would return the same image. This resulted in end results were you can clearly see duplicate images next to each other in large quantities. In order to overcome this I used the KDTree.query's 'k' parameter which allowed me to return the closest k neighbours. I then generated a random int from a range of k so that it was less likely to pick the same images over and over. There are still duplicate images, but it is much better and using a lot more of the image set.

## Technologies
- Python 3.10
- Packages detailed in pipfile (with specific versions in pipfile.lock)

#### Pipfile
```
[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
pandas = "*"
notebook = "*"
ipykernel = "*"
pillow = "*"
matplotlib = "*"
bs4 = "*"
requests = "*"
scipy = "*"
pipfile = "*"

[dev-packages]

[requires]
python_version = "3.10"
```

## Setup
To run this project:

1. Make sure you have pipenv installed and run the following commands within the root folder 'facemo/'
```
pipenv shell
pipenv install pipfile
```
This will initiate a pipenv virtual environment and use the pipfile to install package dependencies

2. Run the following command to open jupyter notebook
```
jupyter notebook
```

3. Within jupyter navigate open the 'code' folder and run the 'facemo-script.ipynb' file

4. You can now run the cells and follow the md notes to change how the script runs

## Customising Images
This project uses a collection of 1000 face images from nvidia's flickr-Faces-HQ dataset which contain images of faces that are free to use. I have used some of the 128x128px thumbnails.

Details at: https://github.com/NVlabs/ffhq-dataset

To use your own images:
1. First replace the 'face-image.png' in the Images folder with your desired image (this will be the image to be recreated with images). A square image works best!

2. Paste all of the images you want to use as mosaic tiles in the 'Mosaic-Images' folder. Thumbnails work best here, as you don't need much detail!
