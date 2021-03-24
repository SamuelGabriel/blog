---
toc: true
layout: post
description: Or, How Automatic Augmentation Research Forgot to Consider Baselines.
comments: true
categories: [ML,AI,augmentations,meta-learning]
title: A Cautionary Tale of Assumptions in ML
---

In a recent project, I set out to improve training data in an automated fashion.
One way this is approached already is through learned augmentation policies, like AutoAugment or PBA. 
Therefore, I decided to focus on learned augmentation policies.

The original objective here is as follows. Given a specific model and a training setup for this model, what augmentation policy should I choose such that the validation accuracy at the end of a full training is high.
We cannot optimize for this objective directly in most circumstances since it is very expensive, every single evaluation of an augmentation policy requires training a full model.
Therefore, different people use different approximations to this objective. AutoAugment for example used smaller datasets, models, and fewer epochs, while some other methods used completely different objectives, e.g. Adversarial AutoAugment searches for policies that yield the worst possible performance on the training set or Fast AutoAugment searches for policies that yield high accuracies when applied to images at test-time.
The results for all these methods looked good, though. 

So, I tried to add to this myself using my own again different approximations to the expensive original task. Just like the previous approaches: with some success.

But, isn't it suspicious that so many different approximations of the original objective work well? Shouldn't some work badly? For my approach, I could tweak a lot of hyper-parameters in many ways without a very bad impact.
At some point I went so far as to change my approach such that it just stands still and does not search at all (zeroed the learning rate): It still worked well!!!

This is the point at which you should get suspicious, I guess. From there on, I tried to remove as much as possible from the algorithm, like complicated combinations of augmentations and saw that it still worked well.

In the end, I arrived at something trivial. I call it TrivialAugment. I simply take an augmentation and apply it with a random strength. That is it.

<img src="https://user-images.githubusercontent.com/9828297/112285487-a82f9c00-8c8a-11eb-83b7-cf3630e63c88.jpeg" width="500"/>

This works better than most previously proposed complicated methods like the above-mentioned methods, see our [paper](https://arxiv.org/abs/2103.10158) for thorough experiments.

<img src="https://user-images.githubusercontent.com/9828297/112294831-8981d300-8c93-11eb-8ae8-8ea198f89910.jpg" width="500"/>


How could this be overseen in previous work? I don't know. But I know that we as ML researchers should never forget to question our assumptions. Especially the big one: learning more components is always better. In most cases, it is better, and this is how machine learning evolved, but there certainly are approaches to learning things that do not perform well, as we have shown in this work.
