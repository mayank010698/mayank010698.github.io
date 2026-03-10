---
layout: page
title: Misc
date: 2025-10-16
tags: [Controlled-Generation, debugging]
mathjax: true
---

Training Free Guided Flow Matching with Optimal Control (ICLR 2025)


the celebA dataset contains 40 binary attribute categories each labelled as present or absent.

```
text_prompts = ['A photo of a smiling face.']
```

```
ema.copy_to(score_model.parameters())
N = 1
batch_size = 1
```

calculate `clip_semantic_loss` between original image and inverted one.

`traj_oc` is storing the entire trajectory.

`len(traj) = 101` 

`traj[0].shape
torch.Size([1, 3, 256, 256])`


`embed_to_latent:`
```
def embed_to_latent(model_fn, img):
  device = img.device
  def ode_func(t, x):
    x = from_flattened_numpy(x, img.shape).to(device).type(torch.float32)
    vec_t = torch.ones(img.shape[0], device=x.device) * t
    drift = model_fn(x, vec_t*999)
    return to_flattened_numpy(drift)
```


