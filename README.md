# Cosmological Dynamic Systems
Two academics, Ahlo and Amendola, derived sets of differential equations from the same fundemental cosmological field equation but achieved very different results. These models have a differing number of critical points despite being derived from the same equation, which is a mathematic impossibility.

![alt text](https://github.com/BillGriffin98/CosmologicalDynamicSystems/blob/main/Ahlo.png)

![alt text](https://github.com/BillGriffin98/CosmologicalDynamicSystems/blob/main/amendola.png)

My project investigated this discrepency by constructing a coordinate transformation between the two sets of variables and ultimately concluded that Amendola's choice of variables allowed for division by zero and hence produced contradictory results. 

## Simulating on Matlab    

While we can study the behaviour of dynamical systems using mathematical tools, it is often helpful to simulate the models and visualise their behaviour. To do this, I generated an initial value and then follow and plotted its trejectory according to the model's governing differential equations. This is an example of an *initial-value problem* and while there are many algorithms that can solve it, I chose the Runge-Kutta method for its speed and ease of implementation.

### The Runge-Kutta Method:
To solve the intial value problem for $\frac{dy}{dt} = f(y, t)$ with the initial condition $y(t_0) = y_0$ is given by the following equations:

$$\begin{align*}
k_1 &= h f(t_n, y_n), \\
k_2 &= h f(t_n + \frac{h}{2}, y_n + \frac{k_1}{2}), \\
k_3 &= h f(t_n + \frac{h}{2}, y_n + \frac{k_2}{2}), \\
k_4 &= h f(t_n + h, y_n + k_3),
\end{align*}$$

where $h$ is the step size, $t_n$ is the current time, and $y_n$ is the current approximation to $y(t_n)$. Then, the next approximation $y_{n+1}$ is calculated as:

$$
y_{n+1} = y_n + \frac{1}{6}(k_1 + 2k_2 + 2k_3 + k_4)
$$

Hence, to simulate each dynamical system in MatLab. I did the following:
1. Define a time range $t$, typically $t = 0$ to $t = 1$.
2. Generate a random pair of initial values $(x_0,y_0)$ for each of our variables.
3. Initilise arrays for values of $x, y, t$, define the governing differential equations, and input step-size $h$.
4. Compute the Runge-Kutta method $1/h$ times and store each value in the array
5. Plot the arrays against eachother and label the axises accordingly

### Difficulties in Implementation
Implementing this method took a lot of trial and error. It was hard to determine exactly how large the step size should be for each model as there was a large varience in computation speed between tests, and hence some models took far too long to plot if the step size was too small. To solve this, I guaged how well each program ran on large step-sizes and then calibrated from there. If a program did take long in order to generate a sufficently-filled diagram, I left the program running in the background.

In addition to this, I had to fine tune the inititalisation and Runge-Kutta step as often the calculations would blow-up to extremely large values - dwarfing the other results and resulting in unusable diagrams. To solve this, I ensured that my intital values fell within the range permitted by the model's choice of variables. This was a difficult processes as it required careful analysis of the choice of variables within the context of cosmology. However, I was able to carefully derive bounds for each variables and inplemented checks at the beginnings of each program to ensure that all initial values fell within their bounds. 

Lastly, the definition for the Runge-Kutta method listed above is for a *1-dimensional* system. As the cosmological dynamic systems featuring in this paper are 2-dimensional, I had to expand the method to account for this. To do this, I applied the method to both the $x$ direction and $y$ direction and plotted the values against each other.

Now we will get into the dynamical systems that I investigated in my paper.

## Csomological Preliminaries
In 1915 Einstein introduced his field equations 

$$\begin{equation}
    G_{\mu \nu} \equiv R_{\mu \nu} - \frac{1}{2}g_{\mu \nu}R =8\pi GT_{\mu \nu}
\end{equation}$$

where $G_{\mu \nu}$ is the Einstein tensor, $R_{\mu \nu}$ is the Ricci tensor, and $R$ is the Ricci scalar. After making some key assumptions and simplifications, we can derive one of the FLRW Fields Equations

$$3H^2 = \kappa^2\rho_m + \kappa^2\rho_{rad} + \Lambda$$ 

then we divide through by $3H^2$ to get our dimensionless variables 

$$\begin{equation}1 = \frac{\kappa^2\rho_m}{3H^2} +\frac{\kappa^2\rho_{rad}}{3H^2} + \frac{\Lambda}{3H^2}\end{equation}$$

$$\begin{align}
    x &= \frac{\kappa^2\rho_m}{3H^2} \\
    y &= \frac{\kappa^2\rho_{rad}}{3H^2} \\
    \Omega_{\Lambda} &= \frac{\Lambda}{3H^2} = \frac{\kappa^2\rho_{\Lambda}}{3H^2}
\end{align}$$

where $\rho_{\Lambda} = \Lambda / \kappa^2$. Now our field equation has become $$1 = x+ y + \Omega_{\Lambda} $$  From this equation we see that we're working in a 2D system with the bounds 

$$x,y \geq 0$$


$$x+y \leq 1$$

and so our state space forms a triangle.  To construct our first cosmological dynamical system we differentiate our variables with respect to conformal time.

$$\begin{align*}
    x' &= x(3x+4y-3)\\
    y' &= y(3x+4y-4)
\end{align*}$$

This system has three critical points: 

$$\begin{align}
R &= (0,1),\\
O &= (0,0),\\
M &= (1,0).
\end{align}$$

We can see the phase portrait generated by Matlab of this system. The code for the program can be found in orginaltriangle.m

![alt text](https://github.com/BillGriffin98/CosmologicalDynamicSystems/blob/main/orginaltrianglepicture.png)

## $f(R)$ Gravity

The observation of a Type 1a supernova in 1998 provided sufficient evidence to deduce that the expansion of the universe is accelerating, this is one of many puzzling observations made in the last 100 years that have called for an extension to Einstein's theory of General Relativity (GR).  One such extension comes from generalising the Einstein-Hilbert action found at the foundation of GR 

$$\begin{align}
    S &= \frac{1}{2\kappa ^2}\int (f(R) + \mathcal{L}_m + \mathcal{L}_r)\sqrt{-\text{det } g}d^4x \\
\end{align}$$

where $\mathcal{L}_m, \mathcal{L}_r$ are the Lagrangian matter and radiation densities respectively.  For the remainder of this paper we will only be interested in vacuum solutions $(\mathcal{L}_m =  \mathcal{L}_r= 0 $) and hence our action reduces to 

$$\begin{align}
    S &= \frac{1}{2\kappa ^2}\int f(R)\sqrt{-\text{det } g}d^4x \\
\end{align}$$

As we can see the generalisation comes from substituting $R$ in the action for a more general $f(R)$, as a result this class of theories are called $f(R)$ theories.

We will be discussing two separate choices of variables and their resulting dynamical systems.  Both choices of variables are vacuum solutions of the FLRW universe and are derived from the same fundamental equations 

$$\begin{align}
H^2 &= \frac{R}{6} - \frac{f}{6F} - \frac{H\dot{F}}{F}\\
-2F\dot{H} &= \ddot{F} - \dot{F}H\\
R &= 6(2H^2 + \dot{H})
\end{align}$$

where $$F := \frac{\text{d}f}{\text{d}R}$$


## Amendola's method

Amendola begins by taking our field equation and dividing by $H^2$ 

$$1 =  \frac{R}{6H^2} - \frac{f}{6FH^2} - \frac{\dot{F}}{FH}$$

We can now introduce our dimensionless variables for our Amendola's model

$$\begin{align*} 
       x_{1} &= -\frac{\dot{F}}{FH} \\
       x_{2} &= -\frac{f}{6FH^2} \\
       x_{3} &= \frac{R}{6H^2} \\
\end{align*}$$

We can now rewrite our field equation in terms of these variables 

$$\begin{equation}
    1 = x_{1} + x_{2} + x_{3}\end{equation}$$

If we differentiate by conformal time and apply some key identities, we get the following 2D system:

$$\begin{align}
    x_1' &= -4+2x_3+3x_1+x_1^2-x_1x_3 \\
    x_3' &= -x_1x_3^2 - 2x_3(x_3 - 2)
\end{align}$$

We can set these equations to 0 to obtain the following 3 critical points:

$$P_1: (0,2)$$
 
$$P_2: (1,0)$$
 
$$P_3: (-4,0)$$

We then simulate the model in MatLab and plot the results. The code for the program can be found in amendola.m
![alt text](https://github.com/BillGriffin98/CosmologicalDynamicSystems/blob/main/amendola.png)

## Ahlo's Method
Beginning with the same field equation, Ahlo derives the following dimensionless variables

$$\begin{align}
    T &= \sqrt{\frac{3}{\alpha}}(\alpha \dot{R} + \alpha HR + \frac{3}{2}H) \\
    X &= \sqrt{\frac{3}{\alpha}}(\alpha \dot{R} + \alpha HR - \frac{1}{2}H) \\
    R &= \sqrt{\frac{12}{\alpha}}(\alpha H\dot{R} + \alpha H^2R + \frac{1}{2}H^2)^{\frac{1}{2}} \\
\end{align}$$

We also introduce the variable

$$\begin{equation} \bar{T} = \frac{1}{1+2\alpha T}\end{equation}$$ 

with bound $0< \bar{T} < 1$. Once differentiating and rearranging, we get the unconstrained system 

$$\begin{align*}
    \frac{\text{d}\bar{T}}{\text{d}\bar{t}} &= \bar{T}(1-\bar{T})[\bar{T}\sin(\theta) + (1-\bar{T})(1-\cos(\theta))^2] \\
    \frac{\text{d}\theta}{\text{d}\bar{t}} &= -\bar{T}(3+\cos(\theta))-(1-\bar{T})(1-\cos(\theta))\sin(\theta)
\end{align*}$$

where $\bar{t} = 2\sqrt{12\alpha}\bar{T}$.

This final system gives us two critical points of the form $(\bar{T},\theta)$ 

$$P_A: (0,2n\pi )$$ 

$$P_B: (0,\pi + 2n\pi)$$ 

We can produce the phase portrait for this dynamical system 
![alt text](https://github.com/BillGriffin98/CosmologicalDynamicSystems/blob/main/Ahlo.png)

At this point, we can notice a discrepency in the number of critical points between Ahlo's and Amendola's dynamical systems. This is troubling as both systems were derived from the same fundemental equations, and hence should have an equal amount of critical points. 



## Coordinate Transformations

To investigate the discrepency in the number of critical points, I suspected I'd need to create a coordinate transformation between them and study its properties. 

Firstly, I investigated a regular transformation by applying a scaling by a factor of 2 in the $x$-axis to our triangle phase portrait:

![alt text](https://github.com/BillGriffin98/CosmologicalDynamicSystems/blob/main/transformedtrianglepicture.png)

Next I investigated the following transformation transformation

$$T = \begin{pmatrix}
\frac{1}{1-y+\epsilon}& 0 \\
0 & \frac{1}{1-y+\epsilon}
\end{pmatrix}$$ 

which will scale our triangle by a factor of $\frac{1}{1+\epsilon}$ in the $x$ direction and a factor of $\frac{1}{\epsilon}$ in the $y$ direction. This transformation is non-regular when $\epsilon= 0$, which we can demonstrate by showing the phase portrait for increasingly smaller values of $\epsilon$

![alt text](https://github.com/BillGriffin98/CosmologicalDynamicSystems/blob/main/ChangesInEpsilon.png)

We can see the issues with this transformation in our phase portraits where our triangle appears to be tending towards a rectangle. This demonstrates that a non-regular transformation will lose critical points been transforming between dynamical systems.

Later in my investigation, I found that the coordinate transformation between Ahlo and Amendola's choice of variables is non-regular and therefore explains why we have a discrepency in the number of critical points. I conclude that Amendola was careless when choosing his variables and allowed for division by 0, hence making his model subject to paradoxes.

