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


### Simulation Results

To evaluate our hypothesized higher-order system model, we conduct a comprehensive assessment. First, the left figure verifies state solutions under ideal conditions, with a continuous prediction-to-update ratio ($\rho=1$). Then, the right figure examines the same configuration variables and their derivatives under lower ratios ($\rho<1$), where external updates are scarcer and more discontinuous:

 &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp; &nbsp; 
 <img src="https://github.com/Daniboy370/Inertial_LQG/blob/main/data/Fig_Comp_1.png?raw=true" width="1150" class='center'/>

We analyze the impact of update ratios on observer-controller (KF-LQR) sensitivity. At $\rho = 0.5$ (top row), small, bounded errors appear, with the rightmost column showing the corresponding control action. This performance stems from the predictive model compensating for uncertainty. At $\rho = 0.1$ (middle row), greater update discontinuity thickens and adds noise to error patterns, increasing control fluctuations as it struggles to stabilize configuration variables. The response remains stable but less smooth, requiring more control effort. At $\rho = 0.01$ (bottom row), the controller switches abruptly between saturation boundaries, indicating instability. State estimates diverge, causing drift and insufficient control feedback, ultimately leading to unrecoverable perturbations.

 &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp;  &nbsp;  &nbsp; &nbsp;  &nbsp; &nbsp; 
 <img src="https://github.com/Daniboy370/Inertial_LQG/blob/main/data/Fig_Ratio.png?raw=true" width="700" class='center'/>

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
