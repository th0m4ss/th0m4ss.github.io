---
title:  "TJCTF 2022 -- web/lamb-sauce"
date:   2022-05-17 00:00:00 -0400
categories: TJCTF2022 web
tags: TJCTF2022 web
# classes: wide
---
First web challenge, very easy.

# Prompt
![Challenge Prompt][img-prompt]

# Solution
To start off the web challenges, we get a link to [lamb-sauce.tjc.tf][challenge-link], where we're greeted with a video of Chef Ramsey:

![Chef Ramsey wants to know where the lamb sauce is][img-page]

Besides that, the page seems to be empty and therefore, we go take a look at the [page source][page-source]:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>where's the lamb sauce</title>
    <style>
      html, body {
        height: 100%;
        margin: 0;
      }

      body {
        font-family: Arial, Helvetica, sans-serif;
        display: flex;
        justify-content: center;
        align-items: center;
      }
    </style>
  </head>

  <body>
    <main>
      <h1>where's the lamb sauce</h1>
      <iframe width="560" height="315" src="https://www.youtube.com/embed/-wlDVf-qwyU?controls=0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
      <!-- <a href="/flag-9291f0c1-0feb-40aa-af3c-d61031fd9896.txt"> is it here? </a> -->
    </main>
  </body>
</html>
```

We can observe a [link to the flag][link-flag] file that's been commented out. So, we navigate to it, and the flag is given.

## Flag
The flag is: `tjctf{idk_man_but_here's_a_flag_462c964f0a177541}`

[img-prompt]: /assets/img/tjctf2022/lamb-sauce/0-prompt.png
[challenge-link]: https://lamb-sauce.tjc.tf
[img-page]: /assets/img/tjctf2022/lamb-sauce/0-page.png
[page-source]: view-source:https://lamb-sauce.tjc.tf
[link-flag]: https://lamb-sauce.tjc.tf/flag-9291f0c1-0feb-40aa-af3c-d61031fd9896.txt
