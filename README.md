# Image_Prevalent_Color
![](https://img.shields.io/badge/python-3.5-green.svg?style=flat)

## Table of Contents

- [Objective](#objective)
- [Installation](#installation)
- [Files](#files)
- [Validation](#validation)
- [Approach](#approach)

## Objective

Given a list of links leading to an image, read this list of images and find 3 most prevalent colors in the RGB scheme in hexadecimal format (#000000 - #FFFFFF) in each image, and write the result into a CSV file in a form of url,color,color,color.

Focus is on speed and resources. The solution should be able to handle input files with more than a billion URLs, using limited resources (e.g. 1 CPU, 512MB RAM). Keep in mind that there is no limit on the execution time, but make sure you are utilizing the provided resources as much as possible at any time during the program execution.

## Installation
1. Install the required packages mentioned in requirement.txt

## Files
- main.py : Starting point, containing implementation of multiprocessing pool as well
- DataLoader.py : Loading the URLs from the input file passed through command line argument
- SamplePreprocessing.py : Preprocessing the image
- clustering.py : KMeans++ implementation
- DominantColor.py: Counting the pixels associated with each cluster centroid
- output.py: Using queue, writing in the output file passed through command line argument
 

## Validation

To get the 3 most prevalent color of an image, run the below command:

```console
python main.py input.txt output.csv
```

input.txt = file containing URLs of images 

outputcsv = file storing 3 most prevalent color along with corresponging image url

## Approach

#### Algorithm

- To identify the most prevalent color, clustering alogrithm **KMeans++** is applied on each image. 
    - Clustering alogrithm would segment the image in different cluster using color as one of the feature.After observing results on some of the data, common cluster number 10 is selected. 
    - To get better result, DBSCAN clustering alogothrim could have been used in place of KMeans++ as it would have removed the dependency on number numbers. But as our focus is not speed and resources, considering DBSCAN took a lot of time, I have used KMeans++ for my final results.

- Once the image is segmented into different cluster, based on the count of pixels associated with every centroid, centroid with 3 large pixels points count represented 3 most prevalent color in image. 

#### Resources Utilization

- All the images are reduced to a fixed size (150,150) to reduce the time.
- Once the images are loaded, multiprocessing is used to preprocess, cluster and count pixels associated with centroid. This helped in processing multiple images at the same time based on the number of cores avaiable in the CPU. To implement this python library `multiprocessing.pool` is used which maps the input to the different processors and collects the output from all the processors.
- Queue manager is used to send the writing tasks to a dedicated process that has sole write access to the output file. All the other workers have read only access. This will eliminate collisions.
