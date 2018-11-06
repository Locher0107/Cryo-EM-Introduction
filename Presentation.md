# Cryo-EM Introduction

Since 2017, Cryo-EM innovators win the Nobel prize in Chemistry, Cryo-EM has entered the public's eyes. Actually from 2012, Cryo-EM area is rapidly growing due to the improvement of equipments, computation tools for image processing and acquisition of better quality data. Several near-atomic resolution reconstructions of protein structures are obtained, which are amazing. More and more scientists turn their interest into this area including those who try to apply statistical models. For instance, Sjors has developed RELION software which turn the recontruction problem into the optimization of a single target function by bayesian approach and EM algorithm. Punjani A, et al have developed stochastic gradient descent method on this optimization problem which is implemented in cryoSPARC software. Both show good performance of algorithm speed, high resolution and accuracy when calculating the 3D structures.

To introduce the background, we start with the typical procedure of Cryo-EM imaging setup. Many randomly oriented macro-molecules specimen of the same kind are frozen ice and put on a holely carbon film (Fig.1, credit to Kazuyoshi and Matthias). This carbon film is placed under the electron microscope. When electrons at the speed of light pass through the sample, electrons would be recorded onto a CCD( Charge-coupled device) to form a projection image (Fig.2, credit to UCSF). More details of this step can be refered to this youtube video https://www.youtube.com/watch?v=Qq8DO-4BnIY&t=19s.

<p align="center">
  <b>Fig.1</b><br>
  <img src="https://github.com/Locher0107/Cryo-EM-Introduction/blob/master/cryo_Em_procedure.jpg" width="300" height="300">
</p>

<p align="center">
  <b>Fig.2</b><br>
  <img src="https://github.com/Locher0107/Cryo-EM-Introduction/blob/master/electrons.jpg" width="300" height="300">
<p>

Essentially, the structure of macro-molecule is represented as electric potential function <a href="https://www.codecogs.com/eqnedit.php?latex=\phi(x)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\phi(x)" title="\phi(x)" /></a>, where <a href="https://www.codecogs.com/eqnedit.php?latex=x\in\mathbb{R}^3" target="_blank"><img src="https://latex.codecogs.com/gif.latex?x\in\mathbb{R}^3" title="x\in\mathbb{R}^3" /></a>. From Fig.3 we can see a complete 3D structure of a protein GroEL, which is a large ribosomal protein mainly involved in proper protein folding and is popular for experiments and simulations. We show GroEL 3D structure by using UCSF Chimera, a software usually used for representing 3D Cryo-Em structure. And this GroEL dataset is downloaded as .MRCS form from PDB (Protein Databank). The PDB code is 4HEL (https://www.rcsb.org/structure/4HEL). 

<p align="center">
  <b>Fig.3</b><br>
  <img src="https://github.com/Locher0107/Cryo-EM-Introduction/blob/master/4hel_3.png" width="300" height="220">
  <img src="https://github.com/Locher0107/Cryo-EM-Introduction/blob/master/4hel_2.png" width="300" height="220">
<p>
 
 In the Cryo-EM experiment, if we put the frozen GroEL protein from Fig.3 in the elctron macroscope from Fig.2, the electrons would pass through the specimens and then we would have a projection image called micrograph. In the experiment, we would do hundreds of times to obtain enough micrographs for 3D reconstruction (Fig.4). As we can see, this micrograph is pretty noisy. In such a micrograph, it includes hundreds of particles. Recall that in a specimen, we have many macro-molecules of the same kind but in different orientations. They are all projected into 2D images in this large micrograph. The red square in Fig.4 picked one particle in a unknow orientation. When moving to the next stage of 3D reconstruction, we need to pick hundreds of particles from each micrograph. This procedure is called particle picking, which is usually done automatically by algorithm or manually by expert. During the process, some deficient particles would be taken out. Finally we have N particles or N 2D projections in spatial domain like the examples in rows in Fig.5.
 
 Mathematically, those N particles can represented as <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\int^\infty_\infty{\phi(R^{-1}.(x_1,x_2,x_3))dx_3}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\int^\infty_\infty{\phi(R^{-1}.(x_1,x_2,x_3))dx_3}" title="\int^\infty_\infty{\phi(R^{-1}.(x_1,x_2,x_3))dx_3}" /></a>, where <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;R" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;R" title="R" /></a> is the rotation matrix in <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\mathbb{R}^3" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\mathbb{R}^3" title="\mathbb{R}^3" /></a>. That is because during the projection through electrons, the image producing procedure is essentially doing the integration of the electric potential function, or 3D density function, going in the electrons passing direction. Furthermore, those particles are also widely called experiment image <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;X_i" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;X_i" title="X_i" /></a>.
 
 
 <p align="center">
  <b>Fig.4</b><br>
  <img src="https://github.com/Locher0107/Cryo-EM-Introduction/blob/master/2Dmicrograph_1.jpg" width="400" height="300">
<p>
  <p align="center">
  <b>Fig.5</b><br>
  <img src="https://github.com/Locher0107/Cryo-EM-Introduction/blob/master/2dparticles.jpg" width="400" height="200">
<p>
  
 However, the Cryo-EM images are not truely projections of the macro-molecules with noise because they are affected by the CTFs (contrast transfer functions). Theoretically, CTFs are defined as a function in the Fourier domain depending on the magnitude of frequency, phase and defocus value.<br>
 <br>
<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;CTF(\vec{s})&=-\omega_1sin(\gamma(\vec{s}))-\omega_2cos(\gamma(\vec{s}))\\" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;CTF(\vec{s})&=-\omega_1sin(\gamma(\vec{s}))-\omega_2cos(\gamma(\vec{s}))\\" title="CTF(\vec{s})&=-\omega_1sin(\gamma(\vec{s}))-\omega_2cos(\gamma(\vec{s}))\\" /></a><br>
<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\gamma(\vec{s})=\gamma(s,\theta)=-\frac{\pi}{2}C_s\lambda^3s^4&plus;\pi\lambda.z(\theta)s^2" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\gamma(\vec{s})=\gamma(s,\theta)=-\frac{\pi}{2}C_s\lambda^3s^4&plus;\pi\lambda.z(\theta)s^2" title="\gamma(\vec{s})=\gamma(s,\theta)=-\frac{\pi}{2}C_s\lambda^3s^4+\pi\lambda.z(\theta)s^2" /></a><br>
<br>
Accordding to the provided parameters from electron microscope, we can compute the CTFs. Fig.6 is the example of a CTF image in Fourier domain. With the above knowledge, we can move to the forward-projection, which is 3D reconstruction, based on projection-slice theorem. Theoretically, the 2D Fourier transform of projection image is equal to
one central slice of 3D Fourier transform of the rotated object and this 2D projection should in a direction perpendicular to the central slice. In other words, this theorem states that if we take the 3D Fourier transform of object A as FA, at the same time we have its one projection B from a specific direction D. Then we take the 2D Fourier transform of B as FB, which FB would be equal to a central slice in FA. This central slice position is fixed and it is perpendicular to direction D. Fig.7 (credit to Lanhui Wang, et al) shows details of how projection-slice theorem works when taking macro-molecule 50s ribosomal as an example. If we have enough central slices estimated and calculated from the experiment images, we could reconstruct 3D Fourier structure. After converting by 3D inverse Fourier transform, we obtain the estimated 3D structure of macro-molecules. As matter of a fact, this is what most Cryo-EM algorithms are based on.
 <p align="center">
  <b>Fig.6</b><br>
  <img src="https://github.com/Locher0107/Cryo-EM-Introduction/blob/master/ctf_plot_3.jpg" width="400" height="300">
<p>
  
   <p align="center">
  <b>Fig.7</b><br>
  <img src="https://github.com/Locher0107/Cryo-EM-Introduction/blob/master/fourier_slice_theorem.jpg" width="600" height="354">
<p>
  
 
