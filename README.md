# Inertial_LQG

&nbsp; &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp;  &nbsp;  <img src="https://github.com/ansfl/MEMS-IMU-Denoising/blob/main/figrues/Logo.png?raw=true" width="500" />


### Introduction
Linear quadratic Gaussian (LQG) control is a well-established method for optimal control through state estimation, particularly in stabilizing an inverted pendulum on a cart. In standard laboratory setups, sensor redundancy enables direct measurement of configuration variables using displacement sensors and rotary encoders. However, in outdoor environments, dynamically stable mobile platforms—such as Segways, hoverboards, and bipedal robots—often have limited sensor availability, restricting state estimation primarily to attitude stabilization. Since the tilt angle cannot be directly measured, it is typically estimated through sensor fusion, increasing reliance on inertial sensors and necessitating a lightweight, self-contained perception module. 

&nbsp; &nbsp;  &nbsp; &nbsp; &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp; &nbsp; 
 <img src="https://github.com/Daniboy370/Inertial_LQG/blob/main/data/Fig_Diagram_4.png?raw=true" width="800" class='center'/>

Despite its effectiveness, prior research has not incorporated accelerometer data into the LQG framework for stabilizing pendulum-like systems, as jerk states are not explicitly modeled in the Newton-Euler formalism. This paper addresses this limitation by leveraging local differential flatness to incorporate linear and angular jerk states into the system model:

&nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp; &nbsp; 
 <img src="https://github.com/Daniboy370/Inertial_LQG/blob/main/data/Fig_Augmented_0.png?raw=true" width="1000" class='center'/>

The resulting higher-order system dynamics enhance state estimation, leading to a more robust LQG controller capable of predicting accelerations for dynamically stable mobile platforms : 

&nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp; &nbsp; 
 <img src="https://github.com/Daniboy370/Inertial_LQG/blob/main/data/Fig_Augmented_1.png?raw=true" width="1000" class='center'/>

This refinement improves overall control performance, as demonstrated in the following simulation:

&nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp; &nbsp; 
 <img src="https://github.com/Daniboy370/Inertial_LQG/blob/main/data/Vid_Pend_1.gif?raw=true" width="950" class='center'/>

Next, the stabilization process of the baseline signal can be illustrated, accompanied by the running version of a one standard deviation, i.e. $\pm \sigma (n)$. The top subfigure presents the short-term time scale, while the bottom one shows the asymptotic convergence resulting from the long-lasting averaging attributes. This demonstrates how the inclusion of additional collected samples effectively cancels out the noise effects.

&nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp; &nbsp; 
 <img src="https://github.com/ansfl/Learning-Based-MEMS-Gyrocompassing/blob/main/figures/Fig_w_ie_0.png?raw=true" width="500" class='center'/>

And finally, after training the model' or alternatively, using the pre-trained weights, performances of both the baseline (blue bubbles) and our model (yellow triangles), can be compared with respect to the GT heading angle of $\psi_{\,\text{GT}}$ = 192.1 $^\circ$, is provided for reference in green.

 &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp; &nbsp; 
 <img src="https://github.com/ansfl/Learning-Based-MEMS-Gyrocompassing/blob/main/figures/Fig_GC_ML.png?raw=true" width="500" class='center'/>


In conclusion, this study we present a practical deep learning framework to effectively compensate for the inherent errors in low-performance gyroscopes. By learning its temporal and spatial error patterns, our model demonstrates robustness to internal and environmental disturbances, allowing for accurate gyrocompassing within shorter time intervals. Our main contribution lies in three key aspects.

### Experimental setup & Dataset

To ensure robust generalizability, acquiring data of both quantity and quality is crucial. The figure below illustrates our experimental setup conducted under controlled laboratory conditions, free from external disturbances. The five main components are highlighted in blue parentheses:

1) Control module [MRU-P datasheet](https://www.inertiallabs.com/mru-datasheet): Ensures level conditions and
   provides GT heading angles ($y$) with a static accuracy of $0.2^\circ$.
2) Test module @ [Emcore SDC500 datasheet](https://emcore.com/wp-content/uploads/2022/05/966762_B-SDC500.pdf). Positioned at the opposite end of the diameter, our MEMS-IMU provides stationary measurements ($x_0 , ..., x_t$) at an opposing heading angle ($y-180^\circ$), with bias instability specification of 1 $^\circ$ / $\text{hr}$ and ARW of 0.02 $^\circ$ / $\sqrt{\text{hr}}$ .
3) Rotating plate: Both sensors are positioned on a levelled plate that rotates freely around its azimuth axis, allowing stationary measurements across various heading angles.
4) Power supply: Ensures a stable and reliable source of energy for uninterrupted system functionality.
5) Computing unit: Serves as the central processing hub, facilitating efficient operation and ensuring that real-time data is saved, labeled, and appropriately organized.

&nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp; &nbsp; 
 <img src="https://github.com/ansfl/Learning-Based-MEMS-Gyrocompassing/blob/main/figures/Fig_Setup.jpg?raw=true" width="550" class='center'/>


## Code

For convenience, both inference and training notebooks are provided, GPU-required.

* **Full Mode** - Full solution pipeline from training to inference. 

* **Test Mode** - Direct inference and comparison between competing models, by uploading pretrained model weights, trained over *single Nvidia T4 GPU*. 

### Directory tree
<pre>
[root directory]
├── figures
├── data
├── execution
|   ├── Training Mode
|       └── *Training.ipynb*
|   └── Inference Mode
|       ├── *Inference.ipynb*
|
...
└── requirements.txt
<!--  Readme.md -->
</pre>


## Citation

The authors would appreciate the users stars (on this repo) and citation of our article as well via:
```
@article{engelsman2023towards,
  title={Towards Learning-Based Gyrocompassing},
  author={Engelsman, Daniel and Klein, Itzik},
  journal={arXiv preprint arXiv:2312.12121},
  year={2023}
}
```

[<img src=https://upload.wikimedia.org/wikipedia/commons/thumb/a/a8/ArXiv_web.svg/250px-ArXiv_web.svg.png width=70/>](https://arxiv.org/abs/2312.12121)
