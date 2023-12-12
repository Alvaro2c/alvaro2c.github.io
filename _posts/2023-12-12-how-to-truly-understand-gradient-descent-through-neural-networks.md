---
layout: post
title: "How to truly understand gradient descent... through neural networks"
author: "Alvaro Carranza"
categories: blog
tags: [blog]
image: gradient-descent.png
---

*Image from 3Blue1Brown gradient descent lesson, which is just another great way to better understand gradient descent. Check him out if you haven't yet! (<a href="https://www.3blue1brown.com/lessons/gradient-descent" target="_blank">source</a>)*
{: style="text-align: center"}


I have recently been studying a lot, not only Airflow as mentioned on my [previous entry](/xml-scraping-101-elementtree-and-namespaces) but also following Andrej Karpathy's <a href="https://www.youtube.com/playlist?list=PLAqhIrjkxbuWI23v9cThsA9GvCAUhRvKZ" target="_blank">"Neural Networks: Zero to Hero"</a> Youtube video series. I am writing and - most importantly - understanding the code necessary to build <a href="https://github.com/karpathy/makemore" target="_blank">makemore</a>, an autoregressive character-level language model, with a wide choice of models from bigrams all the way to a Transformer (exactly as seen in GPT).
{: style="text-align: justify"}

However, I found it really interesting to have an aha moment when watching the <a href="https://www.youtube.com/watch?v=VMj-3S1tku0" target="_blank">first video</a> of the series, which made me really understand **gradient descent**, even though it is one of the first optimization algorithm introduced to train machine learning. In this video he shows how to build <a href="https://github.com/karpathy/micrograd" target="_blank">micrograd</a>, a small Autograd engine that implements backpropagation over a dynamically built DAG and a small neural networks library on top of it with a PyTorch-like API.
{: style="text-align: justify"}

![image](https://i.ibb.co/M60qKnY/andrej.png)

The aha moment came at <a href="https://youtu.be/VMj-3S1tku0?si=UasQvZOa91kTvDb1&t=72722:01:12" target="_blank">2:01:12</a>, when Andrej shows gradient descent optimization manually, but honestly the whole two and a half hours is more than worth it. I won't pretend I am able to explain it better than the man himself, so I will only repeat myself and wholeheartedly recommend to watch the video AND the full series.
{: style="text-align: justify"}
