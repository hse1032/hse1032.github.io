---
layout: project
title: Scalable GANs with Transformers
authors:
    - {name: "Sangeek Hyun", link: "https://hse1032.github.io/", org: 0}
    - {name: "MinKyu Lee", link: "https://scholar.google.com/citations?user=t20G_eAAAAAJ&hl=ko", org: 0}
    - {name: "Jae-Pil Heo", link: "https://sites.google.com/site/jaepilheo", org: 0, corresponding: "True"}
affiliations:
    - "Sungkyunkwan University"

paper_link: https://arxiv.org/abs/2509.24935
supplementary_link: 
arxiv_link: https://arxiv.org/abs/2509.24935
github_link: 
openreview_link: 
conference: Arxiv 2025

tldr: We successfully scale up pure-Transformer GANs and beat diffusion/flow models in one-step class-conditional generation on ImageNet-256.

data_host: &data_host "/static/projects/GAT"

title_image: 
    url: /static/projects/GAT/resources/samples_intro.jpg
  

sections:
    - title: "Abstract"
      is_light: is-light
      paragraphs:
        - type: text
          content: >-
            Scalability has driven recent advances in generative modeling, yet its principles remain underexplored for adversarial learning. We investigate the scalability of Generative Adversarial Networks (GANs) through two design choices that have proven to be effective in other types of generative models: training in a compact Variational Autoencoder latent space and adopting purely transformer-based generators and discriminators. Training in latent space enables efficient computation while preserving perceptual fidelity, and this efficiency pairs naturally with plain transformers, whose performance scales with computational budget.
            Building on these choices, we analyze failure modes that emerge when naively scaling GANs. 
            Specifically, we find issues as underutilization of early layers in the generator and optimization instability as the network scales. Accordingly, we provide simple and scale-friendly solutions as lightweight intermediate supervision and width-aware learning-rate adjustment. 
            Our experiments show that GAT, a purely transformer-based and latent-space GANs, can be easily trained reliably across a wide range of capacities (S through XL). Moreover, GAT-XL/2 achieves state-of-the-art single-step, class-conditional generation performance (FID of 2.96) on ImageNet-256 in just 40 epochs, 6x fewer epochs than strong baselines.
    - title: "Method"
      paragraphs:
        - type: image
          max_width: 1200
          url: /static/projects/GAT/resources/method.jpg
          caption: We revisit GANs through the lens of scalability, pairing two proven ingredients of scalable generative models—training in a compact VAE latent space for efficient, high-fidelity computation and pure transformer architectures that scale with width, depth, data, and compute, to build and study a transformer-only latent-space GAN across substantial capacity ranges. <br/> Our analysis surfaces two scale-coupled failure modes (1) early generator layers become inactive, contributing little to synthesis, and (2) naïvely increasing depth/width with fixed hyperparameters accelerates per-step output changes, destabilizing training. To resolve (1), we introduce Multi-level Noise-perturbed image Guidance (MNG), a noise-hierarchical supervision that aligns intermediate generator outputs with progressively less-corrupted real images, restoring early-layer influence and improving layer-wise utilization. To resolve (2), we propose a simple, width-aware scaling rule, especially for the learning rate, that preserves a near-constant magnitude of output change per optimization step, stabilizing adversarial training as models grow. <br/><br/><br/>
        
        - type: image
          max_width: 1200
          url: /static/projects/GAT/resources/scalability.png
          caption: Our experiments demonstrate the scalability of GAT; (a) increasing model size yields consistently better quality throughout training, not just at convergence; (b) training remains robust across tokenization granularity (e.g., larger patch sizes), while larger patch size achieves consistently lower FID; and (c) performance improves systematically with compuitational cost (GFlops), showing the method effectively converts additional capacity into gains. In short, GAT is a scale-friendly framework whose performance rises smoothly with model size, tokenization choices, and computational budget.



    - title: "Generated examples from GAT-XL/2"
      paragraphs:
        - type: image
          max_width: 1200
          url: /static/projects/GAT/resources/examples/89.jpg
          caption: <span style="display:block; text-align:center">Uncurated examples (ImageNet-256, class 89)</span> <br/><br/>
        - type: image
          max_width: 1200
          url: /static/projects/GAT/resources/examples/279.jpg
          caption: <span style="display:block; text-align:center">Uncurated examples (ImageNet-256, class 279)</span> <br/><br/>
        - type: image
          max_width: 1200
          url: /static/projects/GAT/resources/examples/207.jpg
          caption: <span style="display:block; text-align:center">Uncurated examples (ImageNet-256, class 207)</span> <br/><br/>
        - type: image
          max_width: 1200
          url: /static/projects/GAT/resources/examples/387.jpg
          caption: <span style="display:block; text-align:center">Uncurated examples (ImageNet-256, class 387)</span> <br/><br/>
        - type: image
          max_width: 1200
          url: /static/projects/GAT/resources/examples/933.jpg
          caption: <span style="display:block; text-align:center">Uncurated examples (ImageNet-256, class 933)</span> <br/><br/>
        - type: image
          max_width: 1200
          url: /static/projects/GAT/resources/examples/817.jpg
          caption: <span style="display:block; text-align:center">Uncurated examples (ImageNet-256, class 817)</span> <br/><br/>
        - type: image
          max_width: 1200
          url: /static/projects/GAT/resources/examples/417.jpg
          caption: <span style="display:block; text-align:center">Uncurated examples (ImageNet-256, class 417)</span> <br/><br/>
        - type: image
          max_width: 1200
          url: /static/projects/GAT/resources/examples/979.jpg
          caption: <span style="display:block; text-align:center">Uncurated examples (ImageNet-256, class 979)</span> <br/><br/>
          
    - title: "Latent interpolation across classes and noise seeds"
      paragraphs:
        - type: videos
          video_width: 1050
          autoplay: true      # autoplay
          controls: false     # control bar
          loop: true          # loop
          host: *data_host
          videos:
          - {url: /resources/interpolation/interp1.mp4}
          - {url: /resources/interpolation/interp2.mp4}
          - {url: /resources/interpolation/interp3.mp4}
          - {url: /resources/interpolation/interp4.mp4}
          caption: We show latent-space interpolations performed in style (w) space. Our pure-Transformer GAN inherits key properties of classic GANs, notably a semantic latent space where continuous moves correspond to coherent changes in content and appearance. As we traverse between different classes and noise seeds, images evolve smoothly. These results indicate that, despite its architectural departure and the space to learn (vae latent), GAT preserves the controllable geometry of GAN latent spaces, enabling image-to-image morphs via simple interpolation in w-space.
   

contact: >-
  For any further questions, please contact hse1032@gmail.com
bibtex: >-
  @misc{hyun2025scalableganstransformers,
        title={Scalable GANs with Transformers}, 
        author={Sangeek Hyun and MinKyu Lee and Jae-Pil Heo},
        year={2025},
        eprint={2509.24935},
        archivePrefix={arXiv},
        primaryClass={cs.CV},
        url={https://arxiv.org/abs/2509.24935}, 
  }
---
