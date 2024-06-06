---
layout: project
title: Adversarial Generation of Hierarchical Gaussians <br/> for 3D Generative Model
authors:
    - {name: "Sangeek Hyun", link: "", org: 0}
    - {name: "Jae-Pil Heo", link: "https://sites.google.com/site/jaepilheo", org: 0, corresponding: "True"}
affiliations:
    - "Sungkyunkwan University"

paper_link: /gsgan.html
supplementary_link: 
arxiv_link: https://arxiv.org/abs/2406.02968
github_link: 
openreview_link:
conference: Arxiv 2024

tldr: For the first time, we leverage 3D Gaussian representation in 3D GANs for efficient rendering with explicit 3D representation.

data_host: &data_host "/static/projects/gsgan"

title_image: 
    url: /static/projects/gsgan/resources/fig_example_intro.jpg
  

sections:
    - title: "Abstract"
      is_light: is-light
      paragraphs:
        - type: text
          content: >-
            Most advances in 3D Generative Adversarial Networks (3D GANs) largely depend on ray casting-based volume rendering, which incurs demanding rendering costs. One promising alternative is rasterization-based 3D Gaussian Splatting (3D-GS), providing a much faster rendering speed and explicit 3D representation. In this paper, we exploit Gaussian as a 3D representation for 3D GANs by leveraging its efficient and explicit characteristics. However, in an adversarial framework, we observe that a na\"ive generator architecture suffers from training instability and lacks the capability to adjust the scale of Gaussians. This leads to model divergence and visual artifacts due to the absence of proper guidance for initialized positions of Gaussians and densification to manage their scales adaptively. To address these issues, we introduce a generator architecture with a hierarchical multi-scale Gaussian representation that effectively regularizes the position and scale of generated Gaussians. Specifically, we design a hierarchy of Gaussians where finer-level Gaussians are parameterized by their coarser-level counterparts; the position of finer-level Gaussians would be located near their coarser-level counterparts, and the scale would monotonically decrease as the level becomes finer, modeling both coarse and fine details of the 3D scene. Experimental results demonstrate that ours achieves a significantly faster rendering speed (×100) compared to state-of-the-art 3D consistent GANs with comparable 3D generation capability.
    - title: "Method"
      paragraphs:
        - type: image
          max_width: 1200
        #   url: /static/projects/gsgan/resources/fig_main_architecture_img.jpg
        #   url: /static/projects/gsgan/resources/fig_gaussian_hierachy_img.jpg
          url: /static/projects/gsgan/resources/fig_web.jpg
          caption: To handle with the absence of guidance on position and scale of Gaussians, we devise a method to regularize the Gaussian representation for 3D GANs, focusing particularly on the position and scale parameters. To this end, we propose a generator architecture with a hierarchical Gaussian representation. This hierarchical representation models the Gaussians of adjacent levels to be dependent, encouraging the generator to synthesize the 3D space in a coarse-to-fine manner. Specifically, we first introduce a locality constraint whereby the positions of fine-level Gaussians are located near their coarse-level counterpart Gaussians and are parameterized by them, thus reducing the possible positions of newly added Gaussians. Then, we design the scale of Gaussians to monotonically decrease as the level of Gaussians becomes finer, facilitating the generator's ability to model the scene in both coarse and fine details. <br/><br/><br/>
        # - type: images
        #   max_width: 700
        #   host: *data_host
        #   urls:
        #     - /resources/fig_gaussian_hierachy_img.jpg
        #     - /resources/fig_main_architecture_img.jpg
        #   caption: We asdasdasd We asdasdasdWe asdasdasdWe asdasdasdWe asdasdasdWe asdasdasdWe asdasdasdWe asdasdasdWe asdasdasdWe asdasdasdWe asdasdasdWe asdasdasdWe asdasdasd
        - type: videos
          video_width: 450
          autoplay_attr: autoplay
          host: *data_host
          videos:
          - {url: /resources/interpolation_resall_ffhq256_clip_scale_6000-6007.mp4, caption: "<b>Naive model (FID=95.97, FFHQ-256)</b>"}
          - {url: /resources/interpolation_resall_ffhq256_6000-6007.mp4, caption: "<b>Our model (FID=6.59, FFHQ-256)</b>"}
          caption: With this proposed hierarchical Gaussian representation, we significantly enhance the generation capability of 3D GANs with Gaussian representation (FID=6.59) compared to naive model (FID=95.97), while achieving much faster rendering speed (~3ms per image) compared to NeRF-based 3D GANs.

    - title: Quantitative results (FID)
      paragraphs:
        - type: table
          max_width: 1000
          content: >-
                <table>
                  <thead>
                      <tr>
                          <th><span>Methods</span></th>
                          <th><span>3D consistency</span></th>
                          <th><span>FFHQ 256×256</span></th>
                          <th><span>FFHQ 512×512</span></th>
                          <th><span>AFHQ-Cat 256×256</span></th>
                          <th><span>AFHQ-Cat 512×512</span></th>
                          <th><span>Rendering time 256×256 (ms)</span></th>
                          <th><span>Rendering time 512×512 (ms)</span></th>
                      </tr>
                  </thead>
                  <tbody>
                      <tr>
                          <td><a href="https://arxiv.org/abs/2112.07945" target="_blank">EG3D</a></td>
                          <td><span></span></td>
                          <td><span>4.80</span></td>
                          <td><span>4.70</span></td>
                          <td><span>3.41</span></td>
                          <td><span>2.72</span></td>
                          <td><span>-</span></td>
                          <td><span>15.5<sup>*</sup></span></td>
                      </tr>
                      <tr>
                          <td><a href="https://arxiv.org/abs/2112.08867" target="_blank">GRAM</a></td>
                          <td><span>&#10003;</span></td>
                          <td><span>13.8</span></td>
                          <td><span>-</span></td>
                          <td><span>13.4</span></td>
                          <td><span>-</span></td>
                          <td><span>-</span></td>
                          <td><span>-</span></td>
                      </tr>
                      <tr>
                          <td><a href="https://arxiv.org/abs/2207.10642" target="_blank">GMPI</a></td>
                          <td><span>&#10003;</span></td>
                          <td><span>11.4</span></td>
                          <td><span>8.29</span></td>
                          <td><span>-</span></td>
                          <td><span>7.67</span></td>
                          <td><span>-</span></td>
                          <td><span>-</span></td>
                      </tr>
                      <tr>
                          <td><a href="https://arxiv.org/abs/2206.10535" target="_blank">EpiGRAF</a></td>
                          <td><span>&#10003;</span></td>
                          <td><span>9.71</span></td>
                          <td><span>9.92</span></td>
                          <td><span>6.93</span></td>
                          <td><span>-</span></td>
                          <td><span>-</span></td>
                          <td><span>-</span></td>
                      </tr>
                      <tr>
                          <td><a href="https://arxiv.org/abs/2206.07695" target="_blank">Voxgraf</a></td>
                          <td><span>&#10003;</span></td>
                          <td><span>9.60</span></td>
                          <td><span>-</span></td>
                          <td><span>9.60</span></td>
                          <td><span>-</span></td>
                          <td><span>-</span></td>
                          <td><span>-</span></td>
                      </tr>
                      <tr>
                          <td><a href="https://arxiv.org/abs/2206.07255" target="_blank">GRAM-HD</a></td>
                          <td><span>&#10003;</span></td>
                          <td><span>10.4</span></td>
                          <td><span>-</span></td>
                          <td><span>-</span></td>
                          <td><span>7.67</span></td>
                          <td><span>173.0</span></td>
                          <td><span>197.9</span></td>
                      </tr>
                      <tr>
                          <td><a href="https://arxiv.org/abs/2303.09036" target="_blank">Mimic3D</a></td>
                          <td><span>&#10003;</span></td>
                          <td><span><b>5.14</b></span></td>
                          <td><span><b>5.37</b></span></td>
                          <td><span><u>4.14</u></span></td>
                          <td><span><u>4.29</u></span></td>
                          <td><span>106.8</span></td>
                          <td><span>402.1</span></td>
                      </tr>
                      <tr>
                          <td><span>Ours</span></td>
                          <td><span>&#10003;</span></td>
                          <td><span><u>6.59</u></span></td>
                          <td><span><u>5.60</u></span></td>
                          <td><span><b>3.43</b></span></td>
                          <td><span><b>3.79</b></span></td>
                          <td><span><b>2.7</b></span></td>
                          <td><span><b>3.0</b></span></td>
                      </tr>
                  </tbody>
                </table>


    - title: "1. Generated examples on FFHQ-512"
      paragraphs:
        - type: videos
          video_width: 800
          autoplay_attr: autoplay
          host: *data_host
          videos:
          - {url: /resources/ffhq512.mp4, caption: ""}
    - title: "2. Generated examples on FFHQ-256"
      paragraphs:
        - type: videos
          video_width: 1050
          autoplay_attr: autoplay
          host: *data_host
          videos:
          - {url: /resources/ffhq256.mp4, caption: ""}
    - title: "3. Generated examples on AFHQ-512"
      paragraphs:
        - type: videos
          video_width: 800
          autoplay_attr: autoplay
          host: *data_host
          videos:
          - {url: /resources/afhq512.mp4, caption: ""}
    - title: "4. Generated examples on AFHQ-256"
      paragraphs:
        - type: videos
          video_width: 1050
          autoplay_attr: autoplay
          host: *data_host
          videos:
          - {url: /resources/afhq256.mp4, caption: ""}
    - title: "5. Visualization results with shrink of the scale of Gaussians"
      paragraphs:
        - type: videos
          video_width: 384
          autoplay_attr: autoplay
          host: *data_host
          videos:
          - {url: /resources/ffhq512_shrunk.mp4}
          - {url: /resources/afhq512_shrunk.mp4}
        #   caption: We asdasdasd We asdasdasdWe asdasdasdWe asdasdasdWe asdasdasdWe asdasdasdWe asdasdasdWe asdasdasdWe asdasdasdWe asdasdasdWe asdasdasdWe asdasdasdWe asdasdasd
    - title: "6. Visualization results for each level"
      paragraphs:
        - type: videos
          video_width: 1050
          autoplay_attr: autoplay
          host: *data_host
          videos:
          - {url: /resources/ffhq512_levels.mp4, caption: ""}
          - {url: /resources/afhq512_levels.mp4, caption: ""}
    - title: "7. Novel view synthesis with w+ inversion"
      paragraphs:
        - type: images
          max_width: 200
          host: *data_host
          urls:
            - /resources/tom_input.png
            - /resources/jennifer_input.png
            - /resources/will_input.png
        - type: videos
          video_width: 200
          autoplay_attr: autoplay
          host: *data_host
          videos:
          - {url: /resources/tom_inversion.mp4}
          - {url: /resources/jennifer_inversion.mp4}
          - {url: /resources/will_inversion.mp4}

contact: >-
  For any further questions, please contact hse1032@gmail.com
bibtex: >-
    @misc{hyun2024adversarial,
          title={Adversarial Generation of Hierarchical Gaussians for 3D Generative Model}, 
          author={Sangeek Hyun and Jae-Pil Heo},
          year={2024},
          eprint={2406.02968},
          archivePrefix={arXiv},
          primaryClass={cs.CV}
    }
---
