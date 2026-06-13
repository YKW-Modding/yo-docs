---
title: How to Fix Models to Work on Yo-kai Watch 1S/M
layout: default
parent: Models and Animations
grand_parent: Modding Guides
---

# How to Fix Models to Work on Yo-kai Watch 1S/M
> **Original guide by @heartyalexii on discord, modified by @n123original**

## How to port New Models in YW1 HD
* First, get the model you want to port. Whether it be an existing model from the 3DS game, or a custom one - we will call this the target model.
* Next, get an HD (Switch/Mobile) Model (this can be any model - even an unrelated one), and extract *a* `.xi` and *the* `.sil` file. We will call this the sample model.
* Next, export all textures to `.png` from the `.xi` files located in your target model.
  * You can do this via Kuriimu2.
* In Kuriimu2, in your target model, replace all the `.xi`'s with equivalents from your sample model.
* Next, swap the `.sil` in your target model with the `.sil` from your sample model.
* Now that you have copied the `.xi`'s from the sample model into your target model, change the textures (the contained `.png`, not the `.xi` itself) of the new `.xi` files to what it originally was, using Kuriimu2.
  * This indirectly performs a platform transfer, we modify the textures inside the HD `.xi` files to match the target model's textures while still retaining the platform of the sample model. 
* Save the `.xc` and enjoy!

