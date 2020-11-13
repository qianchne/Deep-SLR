# Deep-SLR : Deep Generalization of Structured Low-Rank Algorithms
Structured low-rank (SLR) algorithms, which exploit annihilation relations between the Fourier samples of a signal resulting from different properties, is a powerful image reconstruction framework in several applications. This scheme relies on low-rank matrix completion to estimate the annihilation relations from the measurements. The main challenge with this strategy is the high computational complexity of matrix completion. We introduce a deep learning (DL) approach to
significantly reduce the computational complexity. Specifically, we use a convolutional neural network (CNN)-based filterbank that is trained to estimate the annihilation relations from imperfect (under-sampled and noisy) k-space measurements of Magnetic Resonance Imaging (MRI). The main reason for the computational efficiency is the pre-learning of the parameters of the non-linear CNN from exemplar data, compared to SLR schemes that learn the linear filterbank parameters from the dataset itself. Experimental comparisons show that the proposed scheme can enable calibration-less parallel MRI; it can offer performance similar to SLR schemes while reducing the runtime by around three orders of magnitude. Unlike pre-calibrated and self-calibrated approaches, the proposed uncalibrated approach
is insensitive to motion errors and affords higher acceleration. The proposed scheme also incorporates image domain priors that are complementary, thus significantly improving the performance over that of SLR schemes.

## Relevant Papers
Pramanik, Aniket, Hemant Aggarwal, and Mathews Jacob, "Deep Generalization of Structured Low-Rank Algorithms (Deep-SLR)", IEEE Transactions on Medical Imaging, 2020. https://ieeexplore.ieee.org/document/9159672

Pramanik, Aniket, Hemant Aggarwal, and Mathews Jacob, "Calibrationless Parallel MRI using Model Based Deep Learning (C-MODL)", 2020 IEEE 17th International Symposium on Biomedical Imaging (ISBI). https://ieeexplore.ieee.org/document/9098490

## Recursive H-DSLR network
<img src="hdslr.png"  title="hover text">
The network solves for 

<img src="opt_prb.png"  title="hover text" width="500px"> 

which alternates between

<img src="alt_steps.png"  title="hover text" width="450px">

### Benefits of Deep-SLR for Parallel MRI
1. It is a calibrationless recovery approach which does not require to estimate coil sensitivities.


2. It is insensitive to motion artifacts arising due to mismatch between calibration and main scans.

<img src="proposed_vs_precalibrated.png"  title="hover text">

3. It can achieve higher accleration factor since no fully sampled calibration region required for sensitivity estimation unlike auto-calibrated methods. 

<img src="proposed_vs_selfcalibrated.png"  title="hover text">

4. Deep-SLR is three orders of magnitude faster than Structured Low-rank (SLR) methods.

<img src="computational_complexity.png"  title="hover text" width="750px">

## Results
### Parallel MRI
<img src="brainPMRI.png"  title="hover text">

### Single-channel Sparse MRI
<img src="brain_sparseMRI.png"  title="hover text">

## H-DSLR Code for Parallel MRI and Single-channel Sparse MRI
This is a tensorflow implementation of hybrid Deep-SLR (H-DSLR) network for undersampled Parallel MRI and single-channel Sparse MRI reconstructions. H-DSLR is a model-based deep learning approach that significantly reduces the computational complexity of Structured low-rank (SLR) algorithms. It employs both k-space and spatial domain priors. The codes have been written in python-3.7 using the Tensorflow-v1.15 library.

### Parallel MRI Dataset
The training dataset consists of 12-channel brain MRI images from 4 subjects collected through SSFP acquisition protocol. There are 90 slices per subject which makes it 90 x 4 = 360 slices in total. Each slice is of dimension 256 x 232 x 12. The dataset can be downloaded from the link https://drive.google.com/file/d/1Fml2PtQuECfbXAI86OYqzBb7K_CiQ2tk/view?usp=sharing .\
The testing dataset is uploaded as tst_img.npy which consists of a slice (256 x 232 x 12) from a subject unseen by the network during training.

### Single-channel Sparse MRI Dataset
The datasets used for single channel experiments are single-channel T1-weighted brain MRI from the Calgary-Campinas Public (CCP) dataset in (https://sites.google.com/view/calgary-campinas-dataset). The single-channel complex valued images are generated by coil combination of 12-channel T1-weighted brain MRI. There are 25 subjects for training, 10 for validation and 10 for testing. There are 256 axial slices per subject with a dimension of 256 x 170.\
A testing dataset is uploaded as test_2_img_axial.npy which consists of two axial slices of 256 x 170 from a subject unseen by the network during training. 

### Description of Python scripts
```trn_HDSLR_parallel_mri.py``` : It is the training code. The H-DSLR model is trained on the 360 12-channel brain slices described above.

```tst_HDSLR_parallel_mri.py``` : It is the code for testing a pre-trained model on the test dataset uploaded as tst_img.npy. The pre-trained model is inside the directory 'savedModels'. This is meant for Parallel MRI recovery.

```trn_HDSLR_single_channel_sparse_MRI.py``` : It is the training code for single-channel sparse MRI recovery. The H-DSLR model is trained on 25 subjects with single-channel brain slices described above.

```tst_HDSLR_single_channel_sparse_MRI.py``` : It is the code for testing a pre-trained model on the test dataset uploaded as test_2_img_axial.npy. The pre-trained model is inside the directory 'savedModels'. This is meant for Single-channel Sparse MRI recovery. 

```auxiliaryFunctions.py``` : Parallel MRI dataset preparation related functions are defined in this script.

```data_processing_functions.py``` : Single-channel Sparse MRI dataset preparation related functions are defined in this script.

```HDSLR.py``` : Defines the H-DSLR network architecture to be trained for parallel MRI or single-channel sparse MRI recovery. 


