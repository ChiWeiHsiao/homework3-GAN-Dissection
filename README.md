# homework3-GAN-Dissection

## Generate images with GANPaint
| Original |   draw   |   remove   |
| :------: | :------: | :--------: |
| ![](assets/GANPaint/1-ori.png) | ![](assets/GANPaint/1-draw.png)(draw grass)| ![](assets/GANPaint/1-remove.png)(remove tree)|
| ![](assets/GANPaint/5-ori.png) | ![](assets/GANPaint/5-draw.png)(draw brick)| ![](assets/GANPaint/5-remove.png)(remove grass)|
| ![](assets/GANPaint/6-ori.png) | ![](assets/GANPaint/6-draw.png)(draw tree)| ![](assets/GANPaint/6-remove.png)(remove brick)|

#### Failure Cases caused by unreasonable draw
- Curious about how the network would response to unreasonable draw, we draw brick/dome/door/grass at the upleft corner (sky).
- While drawing brick/dome/grass results in strong artifacts in all the painted area, drawing door for the same area only lead to small pieces of artifacts.

| Original | draw brick | draw dome | draw door | draw grass |
| :------: | :--------: | :-------: | :-------: | :--------: |
| ![](assets/GANPaint/2-ori.png) | ![](assets/GANPaint/2-draw-brick.png) | ![](assets/GANPaint/2-draw-dome.png) | ![](assets/GANPaint/2-draw-door.png) | ![](assets/GANPaint/2-draw-grass.png) |
| ![](assets/GANPaint/4-ori.png) | ![](assets/GANPaint/4-draw-brick.png) | ![](assets/GANPaint/4-draw-dome.png) | ![](assets/GANPaint/4-draw-door.png) | ![](assets/GANPaint/4-draw-grass.png) |

#### Affect the artifacts by drawing/removeing the same class
- The orignal image contains slight artifact in the sky, we found that drawing sky could help to remove the artifact, while removing sky would intensify the artifact.

| Original | draw sky | remove sky |
| :------: | :------: | :--------: |
| ![](assets/GANPaint/2-ori.png) | ![](assets/GANPaint/2-draw-sky.png) | ![](assets/GANPaint/2-remove-sky.png) |
| ![](assets/GANPaint/4-ori.png) | ![](assets/GANPaint/4-draw-sky.png) | ![](assets/GANPaint/4-remove-sky.png) |


## Dissect GAN model
#### Restaurant
- layer 5 Unit class distribution
    - ![](assets/restaurant/restaurant-layer5.svg)
- layer 8 Unit class distribution
    - ![](assets/restaurant/restaurant-layer8.svg)
- layer 5 examples

    | layer5   | remove window  | remove window | remove table | remove table | remove sky | remove sky |
    | :------: | :------------: | :-----------: | :----------: | :----------: | :--------: | :--------: |
    | before   | ![](assets/restaurant/remove-window-1-before.jpeg) | ![](assets/restaurant/remove-window-2-before.jpeg) | ![](assets/restaurant/remove-table-1-before.jpeg) | ![](assets/restaurant/remove-table-2-before.jpeg) | ![](assets/restaurant/remove-sky-1-before.jpeg) | ![](assets/restaurant/remove-sky-2-before.jpeg) |
    | after    | ![](assets/restaurant/remove-window-1-after.jpeg)  | ![](assets/restaurant/remove-window-2-after.jpeg) | ![](assets/restaurant/remove-table-1-after.jpeg) | ![](assets/restaurant/remove-table-2-after.jpeg) | ![](assets/restaurant/remove-sky-1-after.jpeg) | ![](assets/restaurant/remove-sky-2-after.jpeg) |

#### Church Outdoor 
- layer 5 Unit class distribution
    - ![](assets/outdoor/outdoor-layer5.svg)
- layer 8 Unit class distribution
    - ![](assets/outdoor/outdoor-layer8.svg)
- layer 8 examples

    | layer8   | remove foliage  | remove foliage | remove stone | remove stone |
    | :------: | :------------: | :-----------: | :----------: | :----------: |
    | before   | ![](assets/outdoor/remove-foliage-1-before.jpg) | ![](assets/outdoor/remove-foliage-2-before.jpg) | ![](assets/outdoor/remove-stone-1-before.jpg) | ![](assets/outdoor/remove-stone-2-before.jpg) |
    | after    | ![](assets/outdoor/remove-foliage-1-after.jpg)  | ![](assets/outdoor/remove-foliage-2-after.jpg) | ![](assets/outdoor/remove-stone-1-after.jpg) | ![](assets/outdoor/remove-stone-2-after.jpg) |

## Compare with [inpainting](https://github.com/akmtn/pytorch-siggraph2017-inpainting)
Below we compare the scene ablation result of GANPaint and [Globally and Locally Consistent Image Completion](http://iizuka.cs.tsukuba.ac.jp/projects/completion/en/) using same source image and similar mask.

| id  | source | mask | inpainting | ganpaint |
| :-: | :------: | :------: | :--------: | :-----: |
| (a) | ![](assets/GANPaint/1-ori.png) | ![](assets/inpaint/1-masktree.png) | ![](assets/inpaint/1-masktree.png.out.png) (unreasonable scene) | ![](assets/inpaint/1-masktree.ganpaint.out.png) (slightly better) |
| (b) | ![](assets/GANPaint/1-ori.png) | ![](assets/inpaint/1-maskwindow.png) | ![](assets/inpaint/1-maskwindow.png.out.png) (blurry result) | ![](assets/inpaint/1-maskwindow.ganpaint.out.png) (doesn't remove window) |
| (c) | ![](assets/GANPaint/2-ori.png) | ![](assets/inpaint/2-maskdoor.png) | ![](assets/inpaint/2-maskdoor.png.out.png) (blurry result) | ![](assets/inpaint/2-maskdoor.ganpaint.out.png) (acceptable) |
| (d) | ![](assets/GANPaint/2-ori.png) | ![](assets/inpaint/2-maskgrass.png) | ![](assets/inpaint/2-maskgrass.png.out.png) (unreasonable scene) | ![](assets/inpaint/2-maskgrass.ganpaint.out.png) (from grass to rock) |
| (e) | ![](assets/GANPaint/4-ori.png) | ![](assets/inpaint/4-maskdome.png) | ![](assets/inpaint/4-maskdome.png.out.png) (try to add wooden roof?) | ![](assets/inpaint/4-maskdome.ganpaint.out.png) (acceptable) |
| (f) | ![](assets/GANPaint/4-ori.png) | ![](assets/inpaint/4-maskdoor.png) | ![](assets/inpaint/4-maskdoor.png.out.png) (blurry result) | ![](assets/inpaint/4-maskdoor.ganpaint.out.png) (doesn't remove anything) |
| (g) | ![](assets/GANPaint/5-ori.png) | ![](assets/inpaint/5-maskdoor.png) | ![](assets/inpaint/5-maskdoor.png.out.png) (acceptable) | ![](assets/inpaint/5-maskdoor.ganpaint.out.png) (doesn't remove anything) |
| (h) | ![](assets/GANPaint/5-ori.png) | ![](assets/inpaint/5-masktree.png) | ![](assets/inpaint/5-masktree.png.out.png) (doesn't remove anything) | ![](assets/inpaint/5-masktree.ganpaint.out.png) (inference the structure behind tree!!) |

From the above results, we observe that the inpainting method often failed in case of removing large part of scene (e.g. tree, grass) resulting in a totaly unreasonable scene. On the other hands, ganpaint can yield better result in such scenario. In sample (d), ganpaint succefully convert the grass to rock and the overall scene look reasonable. In sample (h), to our surprise, ganpaint even infer the building structure behind the tree.

In case of smaller object (e.g. door, window), the inpainting method can often remove the object but with blurry result ((b), (c), (f), (g)). Ganpaint, in this case, often remain the original things which could be dut to the high probability that a door or window on that position.
