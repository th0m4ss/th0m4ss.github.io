---
title:  "TJCTF 2022 -- forensics/cool-school"
date:   2022-05-17 01:30:00 -0400
categories: TJCTF2022 forensics
tags: TJCTF2022 forensics steganography
# classes: wide
---
Given a photo, can we find hidden things in it?

# Prompt
![Challenge Prompt][img-prompt]

# Solution
We are given this image:
![Cool school, huh][img-school]

Looking at the image, I don't particularly see anything interesting. Maybe there's secrets hidden the different color channels of the image?

[aperisolve](https://aperisolve.fr) is a nice online tool that performs layer analysis on images provided. Running the provided image through aperisolve, we are shown a few pieces of metadata and different images:

![Aperisolve output][img-aperi]

It's a little hard to make out, but we can see something hidden in the top left corners of the images shown under `Supperimposed`:

![Aperisolve superimposed output][img-super-0]  

![Aperisolve superimposed output][img-super-1]  

![Aperisolve superimposed output][img-super-2]

## Flag
The flag is: `tjctf{l0l_st3g_s0_co0l}`

[img-prompt]: /assets/img/tjctf2022/cool-school/0-prompt.png
[img-school]: /assets/img/tjctf2022/cool-school/1-image.png
[img-aperi]: /assets/img/tjctf2022/cool-school/2-aperisolve.png
[img-super-0]: /assets/img/tjctf2022/cool-school/3-super-0.png
[img-super-1]: /assets/img/tjctf2022/cool-school/3-super-1.png
[img-super-2]: /assets/img/tjctf2022/cool-school/3-super-2.png


