---
layout: project
title: "CAT: Cross-scale Aligned Supervision for Training GANs"
authors:
  - {name: "Sangeek Hyun", link: "https://hse1032.github.io/", org: 0}
  - {name: "MinKyu Lee", link: "https://scholar.google.com/citations?user=t20G_eAAAAAJ&hl=ko", org: 0}
  - {name: "Jae-Pil Heo", link: "https://sites.google.com/site/jaepilheo", org: 0, corresponding: "True"}
affiliations:
  - "Sungkyunkwan University"
# paper_link: "https://arxiv.org/pdf/2605.26449"
# supplementary_link: ""
arxiv_link: "https://arxiv.org/abs/2605.26449"
# github_link: ""
# openreview_link: ""
conference: "arXiv 2026"

tldr: "CAT makes multi-scale adversarial supervision coherent: it keeps discriminator feedback scale-wise, aligns intermediate generator outputs with the final image, and reaches FID-50K 1.56 on ImageNet-256 with one-step generation on in 60 training epochs."

title_image:
  url: /static/projects/CAT/resources/paper_fig1_method_overview.png

sections:
  - title: "Overview"
    is_light: is-light
    paragraphs:
      - type: text
        content: >-
          <style>
            img[src*="/static/projects/CAT/"] {
              display: block !important;
              margin-left: auto !important;
              margin-right: auto !important;
            }
            p:has(img[src*="/static/projects/CAT/"]) {
              text-align: center !important;
            }
          </style>
      - type: text
        content: >-
          Multi-stage generators naturally produce intermediate outputs, and a common way to train them is to apply adversarial supervision at multiple scales. However, making every intermediate output realistic at its own resolution is not the same as making all stages describe the same generated sample. A low-resolution output can be pushed toward one plausible mode, while later stages may move toward another.
      - type: text
        content: >-
          CAT addresses this cross-scale trajectory misalignment. The discriminator remains scale-wise, so each output receives direct adversarial feedback at its own resolution. The generator is then regularized to keep intermediate outputs aligned with the final output, making scale-wise supervision contribute to a shared coarse-to-fine synthesis trajectory.
      - type: text
        content: >-
          This simple organization makes Transformer GAN training substantially more effective. On class-conditional ImageNet-256, CAT-H/2 achieves FID-50K 1.56 with a single generator forward pass, while also reducing cross-scale discrepancy, inter-stage rewriting, and misaligned refinement directions.

  - title: "Why scale-wise realism is not enough"
    paragraphs:
      - type: text
        content: >-
          Scale-wise supervision provides useful local signals, but it does not specify how different stages should coordinate. Each discriminator head only asks whether an image looks realistic at a particular scale. As a result, independently valid gradients can pull different generator stages toward different samples, breaking the intended coarse-to-fine hierarchy.
      - type: image
        max_width: 920
        url: /static/projects/CAT/resources/paper_fig2_failure_modes.png
        caption: >-
          Standard scale-wise supervision can create realistic intermediate outputs without enforcing sample-wise alignment. Later stages may therefore rewrite earlier outputs instead of consistently refining them.

  - title: "Cross-scale aligned supervision"
    is_light: is-light
    paragraphs:
      - type: text
        content: >-
          CAT separates two roles that are often entangled in multi-scale GANs. The discriminator is responsible for direct scale-specific realism feedback. The generator-side consistency term is responsible for cross-scale coordination. This keeps the adversarial objective clean while giving the multi-stage generator an explicit alignment target.
      - type: image
        max_width: 780
        url: /static/projects/CAT/resources/paper_fig3_analysis_setup.png
        caption: >-
          Scale-wise discrimination is implemented by preventing cross-scale token exchange inside the discriminator. Each scale-specific prediction is computed from its corresponding image, while cross-scale alignment is handled on the generator side.
      - type: text
        content: >-
          The consistency loss uses the final-stage output as the common anchor. Lower-scale outputs are encouraged to remain compatible with this final image, so their adversarial feedback is more likely to support the same final synthesis rather than a competing trajectory.

  - title: "Diagnosing cross-scale alignment"
    paragraphs:
      - type: text
        content: >-
          CAT is designed around a measurable behavior: intermediate outputs should become progressively aligned with the final output. We evaluate this with three diagnostics: distance to the highest-scale output, inter-stage rewrite magnitude, and rewrite direction alignment.
      - type: image
        max_width: 920
        url: /static/projects/CAT/resources/paper_fig4_cross_scale_consistency.png
        caption: >-
          Generator-side consistency reduces the gap between intermediate and final outputs, decreases the amount of rewriting between stages, and makes stage-wise updates better aligned with the remaining direction toward the final image.
      - type: image
        max_width: 920
        url: /static/projects/CAT/resources/paper_fig6_consistency_effect.png
        caption: >-
          The alignment effect is consistent across model sizes. CAT improves the internal coherence of multi-scale generation, not only the final FID.

  - title: "Why not aggregate all scales in the discriminator?"
    is_light: is-light
    paragraphs:
      - type: text
        content: >-
          A natural alternative is to give the discriminator the entire image pyramid and let it judge cross-scale consistency directly. CAT avoids this design because it can entangle scale-specific adversarial feedback: a discriminator logit for one scale may depend on evidence from other scales, making the learning signal less directly tied to the image being supervised.
      - type: image
        max_width: 760
        url: /static/projects/CAT/resources/paper_fig7_scale_aggregated_discriminator.png
        caption: >-
          When cross-scale interaction is allowed inside the discriminator, attention becomes strongly coupled across resolutions and performance degrades. This supports the CAT design: keep the discriminator scale-wise and impose cross-scale alignment on the generator.

  - title: "ImageNet-256 results"
    paragraphs:
      - type: text
        content: >-
          CAT targets one-step generation. After training, sampling requires only one generator forward pass. The method achieves strong ImageNet-256 FID while using far fewer training epochs than recent one-step diffusion and flow baselines.
      - type: table
        max_width: 920
        content: >-
          <table class="table is-striped is-hoverable is-fullwidth"><thead><tr><th>Method</th><th>Params</th><th>GFLOPs</th><th>Epochs</th><th>FID-50K</th></tr></thead><tbody><tr><td colspan="5"><em><strong>1-NFE diffusion/flow from scratch</strong></em></td></tr><tr><td>MeanFlow-XL/2</td><td>676M</td><td>119</td><td>240</td><td>3.43</td></tr><tr><td>&alpha;-Flow-XL/2</td><td>676M</td><td>119</td><td>300</td><td>2.58</td></tr><tr><td>FACM</td><td>675M</td><td>119</td><td>800</td><td>2.27</td></tr><tr><td>iMF-XL/2</td><td>610M</td><td>175</td><td>800</td><td>1.72</td></tr><tr><td colspan="5"><em><strong>1-NFE GANs from scratch</strong></em></td></tr><tr><td>GigaGAN</td><td>569M</td><td>-</td><td>480</td><td>3.45</td></tr><tr><td>AdvFlow-XL/2</td><td>673M</td><td>-</td><td>125</td><td>2.38</td></tr><tr><td>StyleGAN-XL</td><td>166M</td><td>1574</td><td>-</td><td>2.30</td></tr><tr><td>GAT-XL/2</td><td>602M</td><td>119</td><td>60</td><td>2.18</td></tr><tr><td><strong>CAT-M/2 (Ours)</strong></td><td><strong>261M</strong></td><td><strong>46</strong></td><td><strong>40</strong></td><td><strong>1.93</strong></td></tr><tr><td><strong>CAT-H/2 (Ours)</strong></td><td><strong>960M</strong></td><td><strong>167</strong></td><td><strong>60</strong></td><td><strong>1.56</strong></td></tr></tbody></table>
      # - type: image
      #   max_width: 520
      #   url: /static/projects/CAT/resources/paper_fig5_fid_curve.png
      #   caption: >-
      #     Larger CAT generators continue to benefit from training, showing stable scaling from B/2 to H/2.

  - title: "Generated samples"
    is_light: is-light
    paragraphs:
      - type: text
        content: >-
          CAT produces diverse ImageNet-256 samples with one-step inference. Latent interpolation also remains smooth, indicating that the improved multi-scale supervision preserves a coherent GAN latent space.
      - type: image
        max_width: 920
        url: /static/projects/CAT/resources/paper_fig8_qualitative_grid.jpg
        caption: >-
          Uncurated generated samples and latent interpolation on ImageNet-256.

contact: >-
  For any further questions, please contact hse1032@gmail.com

bibtex: >-
  @misc{hyun2026crossscalealignedsupervision,
        title={Cross-scale Aligned Supervision for Training GANs},
        author={Sangeek Hyun and MinKyu Lee and Jae-Pil Heo},
        year={2026},
        eprint={2605.26449},
        archivePrefix={arXiv},
        primaryClass={cs.CV},
        url={https://arxiv.org/abs/2605.26449},
  }
---
