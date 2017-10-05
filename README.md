# The Fast Fourier Transform

##Introduction
http://www.dtic.mil/dtic/tr/fulltext/u2/a212493.pdf

Jean Baptiste Joseph Fourier is a XIXth century mathematician known for his work about warm propagation. This study leads him to transform any function of a variable in a series of periodic functions (Fourier series). (Théorie analytique de la chaleur
Auteur : 	Jean-Baptiste-Joseph Fourier
Éditeur: 	Paris : F. Didot, 1822.)
Even if his work was uncomplete, this has been a breakthrough and it is just later, thanks to Joseph Louis Lagrange and Peter Gustav Lejeune Dirichlet, that the Fourier Transform became what we currently know.
The Fourier Transform is a powerfull tool in image processing. 
It converts images from their spacial domain to the frequential domain without loss of information. 
The Fast Fourier Transform (FFT) algorithm computes the Discrete Fourier Transform (DFT) which is a mathematical tool revealing periodicity in data. The DFT is used in many domains and is quite slow to compute (http://ieeexplore.ieee.org/document/1162257/?reload=true). The improvement of the FFT is to compute it more efficiently : *N^2* operations in DFT and *nlogn* operations for FFT.


The first algorithm of DFT was written by Gauss who worked on asteroid's orbites. It was then published by Cooley and Tukey, their algorithm is still the most comonly used and called the *Cooley Tukey FFT algorithm*.
In the image processing domain, the FFT can be used to remove unwanted elements or to improve the visualisation for posterior treatments. As it works with periodic signals, it also can be used in sound treatment for instance.

How the FFT algorithms work and what algorithm is the most efficient are the questions this study is based on. 
To answer it, we will use ImageJ as a reference. The base implemented function is not a Direct Fourier Transform but an Harley transform. It does similar operations but without using complex numbers so it is more efficient. As it is not a Fourier Transform, it will not be evocate later. However there are ImageJ *FFT* plugins that we can compare. The comparison will be done using Micro Benchmarking.

##Material and Methods
parler de la fft implémentée de base dans imageJ et de la FHT
FFTJ / Sean Parsons + ref


The principle of FFT is to apply several small DFT. As the Fourier Transform is wildely used, a lot of algorithms have been made to compute it.
The most common algorithm is called the *Cooley Tukey FFT algorithm*. It consists in decomposing the DFT from a N DFT to N=N1*N2. So this algorithm does N1 DFT of length N2. Then, it does N multiplication by what is called a twiddle factor. A Twiddle factor is the complex constant factor used in this agorithm. The last step is to do N2 DFT of length N1.
(http://studylib.net/doc/18577033/fast-fourier-transforms--a-tutorial-review-and-a-state-of...)

The other FFT algorithms are very similar to this one, they respect the same basis. Their individuality consists in the "special cases" that the Cooley Tukey algorithm does not treat.


https://www.dsprelated.com/freebooks/mdft/Bluestein_s_FFT_Algorithm.html :

 We can also name the Bluestein algoritm (chirp z-transform algorithm) and the Rader's FFT algorithm which are used to compute prime-lenght DFTs. (B. Gold and C. M. Rader, Digital Processing of Signals, New York: McGraw-Hill, 1969.) The difference between both is that the Bluestein algorithm also works when the length is not prime, which make it very usefull. It is one of the algorithm described in the result part. Those algorithms work by doing a cyclic convolution.
Similarely, the prime factor algorithm (PFA) only works if the factors of N (N1 and N2) are relatively prime.


-------------------------------------------
The main principle of FFT is to apply several small Discrete Fourier Transforms (DFT). A DFT converts a finite sequence of N equally-spaced samples. In image processing, pixels value along a row or column are generally used. This sequence is then computed into a same length sequence of a complex-valued samples which are function of frequency. The DFT is therefore said to be a frequency domain representation of the original input. From a theoretical point of view, the complexity issue of the discrete Fourier transform has reached a certain maturity. Some work on length-2 DFTs showed the linear multiplicative complexity of DFT. Appart from that, as it deals with a finite amount of data, it is nowadays efficiently implemented in computers numerical FFT algorithms.

###Cooley & Tukey FFT Algorithm (1805/Gauss, Widespread 1965/Cooley&Tukey)
As the Fourier Transform is widely used, a lot of algorithms have been made to compute it. The most common algorithm is called the *Cooley&Tukey FFT Algorithm*. It was first defined by Gauss, but Cooley and Tukey widespread it in 1965, putting it as a strong reference since then. This algorithm consists in a break of the DFT into smaller DFTs, triggering a N1 smaller DFTs of sizes N2 (N=N1N2). Either N1 and N2 can possibly becoe a small factor called radix. If N1 is the radix, the variant computed technique will be called Decimation in Time (DIT). In an other hand, if N2 become the radix, the technique would be a Decimation in Frequency (DIF). We can also differenciate 2 computational differences depending on the N samples. If N is a power of two, The Cooley variant algorithm will be a *Radix-2*, which is mathematically considered as the simpliest and highly optimized algorithm. The algorithm in this case first computes into two indexed inputs for odd and even numbers. Then it recursively combines the two resulted DFTs parts to produce the DFT in one whole sequence. A similar process is done when N isn't a power of two, with a technique called *Mixed Radix* : The N DFT is computable using two N/2 DFTs, and can have a recursive application using a twiddle factor. A Twiddle Factor is any of the trigonometric constant coefficient multiplied by the input data during the algorithm. They are used to recursively combine smaller DFT.

###Prime Factor Algorithm (PFA)
The PFA is also called *Good-Thomas Algorithm*.
Algorithm that is derived from the ancient chinese remainder theorem.
It is more efficient when factors of N are mutually prime, and is ideally combined with a mixed radix algorithm. Actually, the PFA can be confused with the radix Cooley&Tukey algorithm. But PFA can only work with relatively prime factors and requires a more complex re-indexing of the input, making it less used. However PFA doesn't need any twiddle factor during the computation process.

###Cyclic Convolutionnal Algorithms
Determined in late XXth century, They're also based on Cooley&Tukey Algorithm. We can distinguish 2 main algorithm. First *Rader's algorithm* (1968/Charles M. Rader), which is based on prime sized DFTs as a cyclic convolution. It only depends upon the periodicity of DFT Kernel. But a factor of 2 can be applied, saving the real data. One other is *Bluestein's Algorithm*, or *Chirp-z Transform*, wich computes a cyclic convolution for prime sized DFTs. The specialty of this algorithm is that it could compute the DFT, real DFT and zoom DFT. Bluestein FFT algorithm can be used to compute a contiguous subset of DFT frequency samples, and research are still made to improve is efficiency.

###Bruun's Algorithm (1978)
It is based on a recursive polynomial factorization. It involve only a real coefficient until last computational stage. Furthermore, it may intrisically be less accurate than Cooley&Tukey. It is a good FFT based approach on real data though.

###Hexagonal FFT
Hexagonal FFT (HFFT) has first been developed to use the advantages of hexagonal sampling wich provides good digital methods to process signals. It computes a FFT with a separable Fourier kernel utilizing Array Set Addressing (ASA) scheme and an Hexagonal Fast Fourier Transform (HDFT ). An hexagonal grid improves the efficiency because of its higher symmetry and consistent connectivity. Despite that, its application is limited because of its inability to compute orthogonal rows and columns, as a Cooley&Tukey algorithm.

All of those methods are available to compute FFT. The question now is to know which of those methods and which implementation of it will be the more efficient. To answer it, we will use Benchmarking, more precisely Micro-Benchmarking. The Benchmark consists in knowing the relative performance of a program while running it. Factors such as the used resources like the memory and CPU will be mesured. The values obtained are then compared to other programs with the same purpose. It allow the developper to see if his algorithm and implementation are an improvement compared to the references. Micro Benchmarking is a specific Benchmark. The difference is that Micro Benchmark just mesure the performances of a little piece of code, here we compare plugins and not softwares for instance.

In this study, running time and used memory will be mesured and will allow us to compare plugins using different algorithms.
In theory we are also supposed to compare the use of CPU

##Results
parler de la config du pc utilisé
parler plus du benchmark
parler du protocol utilisé pour comp les img
donner les résult 
plugins : 
-> parallel fftj (https://sites.google.com/site/piotrwendykier/software/parallelfftj) -> cooley à check
-> beat (http://imagej.net/Xlib#.27Xlib.27_plugins) -> beistein
-> fft (http://fly.mpi-cbg.de/~preibisch/software.html) -> cooley
parler du facteur 10

The family of algorithm described in Mat&Met section will not be represented here. Indeed, as the *Cooley Tukey FFT algorithm* is mainly used, there is not a plugin for each of the other algorithms. 
Here will be compared three FFT algorithm : two *Cooley Tukey FFT algorithm* and one *Bluestein FFT algorithm*. 
One of the *Cooley Tukey FFT algorithm* is from the website of Preibisch Laboratory, the Berlin Institute of Medical Systems Biology (BIMSB). This website contains the *FFT Transform Plugin (2D/3D)* which display the phase spectrum and the power spectrum.
The second *Cooley Tukey FFT algorithm* plugin is from the website of the PhD Piotr Wendykier. It is an improved version of the *FFTJ* plugin written by Nick Linnebrügger. For this plugin, RGB images are not supported.
The last algorithm, using *Bluestein FFT algorithm*, is from the *Xlib* plugins and can process *FFT* 2D and 3D images of arbitrary size and give the real and imaginary part of the *FFT*. It also does the reverse *FFT*.
As one of our algorithm only computes grey-scale images, two comparison will be done : one with the three plugins on grey scaled images and one with the two plugins working with colored images. 
The Benchmark using bellow is a JavaScript program we have written comparing the time of execution and the memory used. We have run this test 10 000 times for each plugin to have enough data. Forthermore, there is a warm-up phase of 100 iteration.
We would have prefered to compare the algorithms without the whole plugin. Unfortunately, we don't have access to all the source codes of our plugins. Therefore, it is not possible to implement Benchmark directly in the plugins. 


![Alt text](https://github.com/Nine-s/Biologie_Structurale/blob/master/Images/embryos.jpg "Reference Image")
*Figure 1: Image from samples of ImageJ, used to run the FFT plugins*


![Alt text](https://github.com/Nine-s/Biologie_Structurale/blob/master/Images/beat_FFT%202D_imag_no%20scal.jpg "Reference Image")
*Figure 1: Image from samples of ImageJ, used to run the FFT plugins*


![Alt text](https://github.com/Nine-s/Biologie_Structurale/blob/master/Images/beat_FFT2D_real_no%20scal.jpg "Reference Image")
*Figure 1: Image from samples of ImageJ, used to run the FFT plugins*


![Alt text](https://github.com/Nine-s/Biologie_Structurale/blob/master/Images/fft_Phase_of_img.jpg "Reference Image")
*Figure 1: Image from samples of ImageJ, used to run the FFT plugins*

![Alt text](https://github.com/Nine-s/Biologie_Structurale/blob/master/Images/fft_Power_of_img.jpg "Reference Image")
*Figure 1: Image from samples of ImageJ, used to run the FFT plugins*

/!\ à faire -> numéroter les pages, revoir le nb d'iter warmup+test, rajouter des trucs dans l'intro, ajouter les images, 


##Discussion
remettre les résulats d'efficacité dans leur contexte (plus ou moins d'options proposées, date de sortie des plug in, parler du fait qu'on utilise que java etc.)

##Conclusion
récap résult+Discussion
en ouv parler de la suite du projet

##References

[^PIC1988]: Piccini F. Technical Memorandum: The Fast Hartley Transform as an alternative to the Fast Fourier Transform. Surevillance Research Laboratory.1988 Feb
[^HEI1984]: Heideman MT, Johnson DC, Sidney Burrus C. Gauss and the History of the Fast Fourier Transform. IEEE ASSP Magazine. 1984 Oct; 1(4):14-21
[^DUH1999]: Duhamel P, Vetterli M. Fast Fourier Transform: A Tutorial Review and a State of the Art. Signal Processing. 1999;19:259-299
[^AMA2015]: Amannah CI, Bakpo FS. Simplified Bluestein numerical Fast Fourier Transforms Algorithm for DSP and ASP. International Journal of Research Granthaalayah. 2015 Nov
[^WEE1984]: Weed JC, Polge RJ. An efficient implementation of a Hexagonal FFT. Acoustics, Speech and Signal Processing IEEE Conference. 1984 Material

