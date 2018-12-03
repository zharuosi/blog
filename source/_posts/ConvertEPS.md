---
title: Convert images to EPS format
date: 2018-10-18 22:25:44
categories: 吴带当风
tags: Linux
---

## Introduction

.eps is a common used format of images in academic writting by $LaTex$. If you want to convert hundreds of .jpg or .png. formats to .eps, the command *convert* in Linux system provides a good solution.

## ImageMagick
<!-- more -->
The command *convert* is a tool from the tool ImageMagick, includes a number of command-line utilities for manipulating images. Please install it if *convert* is not found.

Another utility from is *identify*. It can output the size and other information of the interested image, e.g.,

    identify Behdinan1998.png

The output is:

    Behdinan1998.png PNG 1103x792 1103x792+0+0 8-bit sRGB 96.2KB 0.000u 0:00.000

## Convert

The simplest way is,

    $ convert src.png dst.eps

To change the image size, 

    $ convert  -resize 1920×1080 src.png dst.eps
    # Change the size according to the ratio
    $ convert  -resize 50%x100%! src.png dst.eps
    # Change the size according to the width and keep the origin aspect ratio
    $ convert  -resize 50%x100% src.png dst.eps

To change the dpi of the image,

    $ convert  -density 300 -resize 1920×1080 src.png dst.eps

To rotate the figure (degree),

    $ convert  -rotate 90 src.png dst.eps

To add some text in the figure,

    $ convert -fill black -pointsize 60 -font Times -draw 'text 100,800 "NAOE" ‘ src.png dst.png

However, the -destiny will lead to as large as hundreds of MB for one image. To solve this problem, we use a pdf file as a temporary file.

    $ convert -density 300 -resize 1920×1080 src.png dst.pdf && pdftops -eps dst.pdf

The output file is not large and clear enough.

## Error during converting

When you first use conert src.png dst.pdf, maybe the error message occurs:

    convert: not authorized `pictures.pdf' @ error/constitute.c/WriteImage/1028

As a temporary fix, you can edit **/etc/ImageMagick-6/policy.xml** and change the PDF rights from **none** to **read|write** there.

[[Reference]](https://askubuntu.com/questions/1081695/error-during-converting-jpg-to-pdf)

    The EPS file can not be converted to PNG using this tool. Please tell me if you know how.

## An example

Here is a shell script for you. It can convert all the png files in the current directory to EPS files. The original images are not removed.

    for origin_image in `ls *.png`
    do
        file_name = $(basename $origin_image .png)
        convert -density 300 -resize 1920×1080 $file_name.png $file_name.pdf
        pdftops -eps $file_name.pdf
    done


Enjoy it.