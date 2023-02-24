---
title: "Robotics Specialization: Aerial Robotics"
date: 2016-02-15T11:30:03+00:00
# weight: 1
# aliases: ["/first"]
tags: ["robotics", "control"]
author: "Me"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: true
draft: false
hidemeta: false
comments: true
description: "Controller design for quadrotors.  "
# canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: true
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: false
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: false
math: true
cover:
    # image: "<image path/url>" # image path/url
    image: "/projects/robotics_specialization/imgs/course1/aerial_robot.png"
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: true # when using page bundles set this to true
    hidden: false # only hide on current single page
    hiddenInList: false
    hiddenInSingle: false
editPost:
    URL: "https://github.com/<path_to_repo>/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

Here's the assignment for the *Robotics Specialization: Aerial Robotics* offered by UPenn on Coursera. 

In this course, we learned some basic ideas about autonomous robots and the design of quadrotors. There are three assignments in this course in which we learned to design a PD controller for a quadrotor in 1D, 2D, and 3D. 
The course provided quadrotor simulators to help with the experiment. The simulator uses MATLAB's ODE solver, ode45, to simulate the behavior of the quadrotor and plt3 to visualize the state of the quadrotor at each time step. 
# 1D Quadrotor Control
For the first part, we only consider the motion in $z$-axis as shown in the figure.
{{< figure align=center src="/projects/robotics_specialization/imgs/course1/1d_quad.png" width="50%" title="Quadrotor's vertical motion">}}
The net-force in vertical direction is given by 
$$F=m\ddot{z} = u - mg$$
where $u$ is the motor thrusts.
The dynamic of quadrotor in vertical direction can be described by the following differential equation
$$\ddot{z}= \frac{u}{m}- g$$

We can measure the current position and velocity of the quadrotor in vertical direction. Then we can use these and desired position and velocity to calculate the errors
$$e = z_{des} - z$$
<!-- $$\dot{e} = \dot{z}_{des} - \dot{z}$$ -->
the control input for a PD controller is
$$u=m(\ddot{z}+ K_pe + K_d\dot{e} +g)$$
By choosing proper values for propotional and derivative gain, we can make the quadrotor response quickly to a step input. The result is as 
{{< figure align=center src="/projects/robotics_specialization/imgs/course1/1d_step.gif" width="100%" title="Quadrotor's response to step input">}}


# 2D Quadrotor Control
For the second part, we learned to control the quadrotor in 2D plane, as shown in the following figure.
{{< figure align=center src="/projects/robotics_specialization/imgs/course1/2d_quad.png" width="80%" title="Quadrotor's planar motion">}}
In the figure, $a_2, a_3$ denote the inertial frame and $b_2, b_3$ denote the body frame. 
In this setup, we want to control the elevation and the roll angle of the quadrotor with two inputs: the thrust force ($u_1$) and the moment ($u_2$). 
The thrust force is defined as the sum of the thrusts at each rotor
$$u_1 = F_1+F_2,$$
and the moment is the difference between the thrusts of two rotors times the arm length of the quadrotor ($L$),
$$u_2 = L(F_1 - F_2).$$

Suppose we can measure the position, velocity, acceleration and the angular acceleration of the robot. Let $r=[y, z]^T$ detnote the position vector of the quadrotor in the inertial frame, and $R$ be the rotation matrix that transfrom vector components from body frame to inertial frame.
$$ R = \begin{bmatrix} \cos(\phi) & -\sin(\phi) \\\ \sin(\phi) & \cos(\phi) \end{bmatrix} $$
The net-force in $Y, Z$ direction is given by
$$ m \begin{bmatrix} \ddot{y} \\\ \ddot{z} \end{bmatrix} = \begin{bmatrix} 0 \\\ -mg \end{bmatrix} + R\begin{bmatrix} 0 \\\ u_1 \end{bmatrix} = \begin{bmatrix} 0 \\\ -mg \end{bmatrix} + \begin{bmatrix}  -u_1 \sin(\phi) \\\ u_1\cos(\phi)\end{bmatrix}.$$
The angular acceleration is given by 
$$ I_{xx}\ddot{\phi} = L(F_1 - F_2) = u_2.$$
Therefore, the system model is given by
$$\begin{bmatrix} \ddot{y} \\\ \ddot{z} \\\ \ddot{\phi}  \end{bmatrix} = \begin{bmatrix} 0 \\\ -g \\\ 0 \end{bmatrix} + \begin{bmatrix} -\frac{1}{m} \sin(\phi) & 0 \\\ \frac{1}{m} \cos(\phi) & 0 \\\ 0 & \frac{1}{I_{xx}}\end{bmatrix} \begin{bmatrix} u_1 \\\ u_2\end{bmatrix}.$$

The dynamic model of the quadrotor is nonlinear, so we need to linearize the equation of motions in order to apply the PD controller. For the quadrotor, the equilibrium point is the configuration at any position $(y^{(t)}, z^{(t)})$ with zero roll angle; and the thrust force is equal to the gravity $mg$. So we have $y^{(t)}, z^{(t)}, \phi^{(t)}=0, u_1^{(t)} = mg, u_2^{(t)} = 0$. By the first-order Taylor approximation, we have $\sin(\phi^{(t)})\approx 0, \cos(\phi^{(t)}) \approx 1$. Thus the above system model can be written as
$$\begin{bmatrix} \ddot{y} \\\ \ddot{z} \\\ \ddot{\phi}  \end{bmatrix}  = \begin{bmatrix} -g\phi \\\ -g + \frac{u_1}{m} \\\ \frac{u_2}{I_{xx}}  \end{bmatrix}, $$
and 
$$ \begin{bmatrix} u_1 \\\ u_2 \\\ \phi \end{bmatrix} = \begin{bmatrix}  m(\ddot{z} +g) \\\ I_{xx}\ddot{\phi} \\\ -\frac{\ddot{y}}{g} \end{bmatrix} $$


Let $s$ denotes the state variable $(y$, $z$ or $\phi)$; $s_T(t)$ be the desired state variable at time $t$. Define the position and velocity errors as
$$\begin{bmatrix} e \\\ \dot{e} \end{bmatrix} = \begin{bmatrix}  s_T(t) - s \\\  \dot s_T(t) - \dot{s}\end{bmatrix}$$
The error should satisfy the following differential equation,
$$ (\ddot r_T(t) - \ddot{r}) + K_d\dot{e}  + K_pe = 0. $$
So the commanded acceleration of the state can be expressed as 
$$ \ddot{r} = \ddot r_T(t) + K_d\dot{e}+  K_pe . $$
The input can be expressed as 
$$ \begin{bmatrix} u_1 \\\ u_2 \\\ \phi \end{bmatrix} = \begin{bmatrix}  m(\ddot z_T(t) + K_{d,z}(\dot z_T(t)-\dot z)+  K_{p,z}(z_T(t) - z) +g) \\\ I_{xx}(\ddot \phi_T(t) + K_{d,\phi}(\dot \phi_T(t)-\dot \phi)+  K_{p,\phi}(\phi_T(t) - \phi)) \\\ -\frac{1}{g} (\ddot y_T(t) + K_{d,y}(\dot y_T(t)-\dot y)+  K_{p,y}(y_T(t) - y))\end{bmatrix} .$$
The resulting controller is as shown below.
{{< figure align=center src="/projects/robotics_specialization/imgs/course1/2d_traj.gif" width="100%" title="2D trajectory">}}


<!-- # 3D Quadrotor Control

The last part involves -->