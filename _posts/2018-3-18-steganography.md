---
layout: post
title: Steganography - Hiding an image inside another
---

<img src="{{ site.baseurl }}/images/steganography.jpeg" alt="Steganography" style="width: 600px;"/>

In this story, we will learn some image processing concepts and how to hide an image inside another image file.

To provide a functional example, it was implemented a [**Python** class](https://github.com/kelvins/steganography/blob/master/steganography.py) to perform the procedures mentioned in the end of this story.

First of all, let’s understand what is steganography, digital images, pixels, and color models.

### What is steganography?
> [Steganography](https://en.wikipedia.org/wiki/Steganography) is the practice of concealing a file, message, image, or video within another file, message, image, or video.

### What is the advantage of steganography over cryptography?
> The advantage of steganography over [cryptography](https://en.wikipedia.org/wiki/Cryptography) alone is that the intended secret message does not attract attention to itself as an object of scrutiny. Plainly visible encrypted messages, no matter how unbreakable they are, arouse interest and may in themselves be incriminating in countries in which [encryption](https://en.wikipedia.org/wiki/Encryption) is illegal.

In other words, steganography is more discreet than cryptography when we want to send a secret information. On the other hand, the hidden message is easier to extract.

### What is a digital image?

Ok, now that we know the basics of steganography, let’s learn some simple image processing concepts.

Before understanding how can we hide an image inside another, we need to understand what a digital image is.

We can describe a **digital image** as a finite set of digital values, called pixels. Pixels are the smallest individual element of an image, holding values that represent the brightness of a given color at any specific point. So we can think of an image as a matrix (or a two-dimensional array) of pixels which contains a fixed number of rows and columns.

![](https://cdn-images-1.medium.com/max/2000/1*-Vreo05sajRL8dRibXIWXA.png)

When using the “**digital image**” term here, we are referencing to the “**raster graphics**”, which are basically a dot matrix data structure, representing a grid of pixels, which in turn can be stored in image files with varying formats. You can read more about [digital images](https://en.wikipedia.org/wiki/Digital_image), [raster graphics](https://en.wikipedia.org/wiki/Raster_graphics), and [bitmaps](https://en.wikipedia.org/wiki/Bitmap) at the [Wikipedia](https://en.wikipedia.org/wiki/Main_Page) website.

### Pixel concept and color models

As already mentioned, pixels are the smallest individual element of an image. So, each pixel is a sample of an original image. It means, more samples provide more accurate representations of the original. The [intensity](https://en.wikipedia.org/wiki/Intensity_(physics)) of each pixel is variable. In color imaging systems, a color is typically represented by three or four component intensities such as [red, green, and blue](https://en.wikipedia.org/wiki/RGB_color_model), or [cyan, magenta, yellow, and black](https://en.wikipedia.org/wiki/CMYK_color_model).

Here, we will work with the [RGB color model](https://en.wikipedia.org/wiki/RGB_color_model). As you can imagine, the RGB color model has 3 channels, red, green and blue.
> The **RGB color model** is an additive [color model](https://en.wikipedia.org/wiki/Color_model) in which red, green and blue light are added together in various ways to reproduce a broad array of colors. The name of the model comes from the initials of the three additive primary colors, red, green, and blue. The main purpose of the RGB color model is for the sensing, representation and display of images in electronic systems, such as televisions and computers, though it has also been used in conventional [photography](https://en.wikipedia.org/wiki/Photography).

![](https://cdn-images-1.medium.com/max/2000/1*tcTa2Cst3FXkDpxTg-_1mA.jpeg)

So, each pixel from the image is composed of 3 values (red, green, blue) which are [8-bit](https://en.wikipedia.org/wiki/8-bit) values (the range is 0–255).

![](https://cdn-images-1.medium.com/max/2000/1*Mt3yDPhS3aq_spPfWTW9BA.png)

As we can see in the image above, for each pixel we have three values, which can be represented in [binary code](https://en.wikipedia.org/wiki/Binary_code) (the computer language).

When working with binary codes, we have more significant bits and less significant bits, as you can see in the image below.

![](https://cdn-images-1.medium.com/max/2000/1*YQGZLBpDn2U9Bu8sZphzXQ.jpeg)

The **leftmost** bit is the **most significant bit**. If we change the leftmost bit it will have a large impact on the final value. For example, if we change the leftmost bit from **1** to **0** (**11111111** to **01111111**) it will change the decimal value from **255** to **127**.

On the other hand, the **rightmost** bit is the **less significant bit**. If we change the rightmost bit it will have less impact on the final value. For example, if we change the leftmost bit from **1** to **0** (**11111111** to **11111110**) it will change the decimal value from **255** to **254**. Note that the rightmost bit will change only 1 in a range of 256 (it represents less than 1%).

**Summarizing:** each pixel has three values (RGB), each RGB value is 8-bit (it means we can store 8 binary values) and the rightmost bits are less significant. So, if we change the rightmost bits it will have a small visual impact on the final image. This is the steganography key to hide an image inside another. Change the less significant bits from an image and include the most significant bits from the other image.

![](https://cdn-images-1.medium.com/max/2000/1*kpDa0jt6ftSce4b4DQA2MQ.png)

### Hiding an image inside another

Since we understood the pixel concept and color models, we can talk about the procedure of hiding an image inside another.

In this section, we can find a step-by-step of the hide and reveal process using Python code.

**Hiding an Image**:

1. To hide an image inside another, the image which will be hidden needs to have at most the same size of the image which will hide it.

<script src="https://gist.githubusercontent.com/kelvins/c0859873c85b6dc4cbc79aa899722d5c.js?file=steganography.py"></script>

2. We must create two loops to go through all rows and columns (actually each pixel) from the images.

<script src="https://gist.githubusercontent.com/kelvins/c7fe8934ea5867b5467798c840c59edb.js?file=steganography.py"></script>

3. So, we get the RGB from the image 1 and image 2 as binary values:

<script src="https://gist.githubusercontent.com/kelvins/c100b72bf19c2200233f656c425c53ff.js?file=steganography.py"></script>

We can use the **__int_to_bin** method to convert a decimal value to a binary value:

<script src="https://gist.githubusercontent.com/kelvins/63e3bba3c7269c2f60113ebd5747d19a.js?file=steganography.py"></script>

4. We merge the most significant bits from the image 1 with the most significant bits from the image 2:

<script src="https://gist.githubusercontent.com/kelvins/549df16cd999d44387320bc7083ebab5.js?file=steganography.py"></script>

Using the **__merge_rgb** method:

<script src="https://gist.githubusercontent.com/kelvins/983bc8c775944167cece866f48848226.js?file=steganography.py"></script>

Note that the **__merge_rgb** function is using the 4 most significant bits from each image, but it could be changed. Keep in mind that using fewer bits from the hidden image will result in low quality of the recovery image.

5. Finally, we convert the new binary value to a decimal value:

<script src="https://gist.githubusercontent.com/kelvins/5b94599074d89cb8cbb30cda44f621ac.js?file=steganography.py"></script>

Using the **__bin_to_int** method:

<script src="https://gist.githubusercontent.com/kelvins/8b95893dd1c7ab2e56bfb9c21f8ccfe5.js?file=steganography.py"></script>

And set it to a new pixel position from the resulted image.

Now we have an image hidden inside another image.

The entire **merge** method can be found [here](https://github.com/kelvins/steganography/blob/master/steganography.py#L49).

**Revealing an Image**:

1. To reveal an image, we must know how many bits were used to hide the image. In this case, we are using a fixed number of 4 bits.

1. First of all, we need to create two loops to go through all pixels from the image:

<script src="https://gist.githubusercontent.com/kelvins/4dd53121a80b8a02841157368780b50c.js?file=steganography.py"></script>

3. So, we extract each RGB channel as a binary value from the current pixel:

<script src="https://gist.githubusercontent.com/kelvins/dccb4376f3b6c86f789dfa3a77e94703.js?file=steganography.py"></script>

Using the **__int_to_bin** method:

<script src="https://gist.githubusercontent.com/kelvins/5e07d625af1ba407c54dfd76fb78854b.js?file=steganography.py"></script>

4. Then, we create a new RGB value by concatenating only the 4 rightmost bits from the current pixel with zero values (to create a new 8-bit value):

<script src="https://gist.githubusercontent.com/kelvins/fe34b42f0819b114398b905cf70a34b4.js?file=steganography.py"></script>

5. Finally, we convert the binary value to a decimal value and set it to the current pixel in the new image:

<script src="https://gist.githubusercontent.com/kelvins/dbc9300cd3433148f8cff465bb9139fe.js?file=steganography.py"></script>

6. The developed algorithm has only one more last step to remove the black borders when the hidden image was smaller than the image which is hiding it.

<script src="https://gist.githubusercontent.com/kelvins/916b0e236836252a463a878c1742bce9.js?file=steganography.py"></script>

And with this simple code, we can extract an image from another.

The entire **unmerge** method can be found [here](https://github.com/kelvins/steganography/blob/master/steganography.py#L88).

**You can check out the result in the following image**:

The left upper image is the image that will hide the right upper image. The left lower image is the two images merged and the right lower image is the extracted (unmerged) image.

![](https://cdn-images-1.medium.com/max/2000/1*4paRMea_BGeNpJ2VzFGCoA.png)

As you can see in the image above, we lost some image quality in the process, but this does not interfere with image comprehension.

You can find the **Steganography Python code** on **Github**: [https://github.com/kelvins/steganography](https://github.com/kelvins/steganography)
