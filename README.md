# Examining StyleGAN as a Utility-Preserving Face De-identification Method
This repository is a part of my paper: Examining StyleGAN as a Utility-Preserving Face De-identification Method.
You can download the code and data from here: https://drive.google.com/drive/folders/13vA3sBsBWrthZpbJIU3IjMsaA-mDLznh?usp=sharing

The codes were successfully run on the following GPU and Linux OS. There is no guarantee that they are executable on other OS with different GPU types:
 
 
 Valence VWS-1542881-DPN - Deep Learning DevBox (X299, 1x i7-7820X , 16GBx4 DDR4, 1x 1TB OS SSD, 1x 8TB HDD, Dual 10GBase-T Onboard, 2x RTX 2080 TI 11GB GPU).
 
 
Step 1: Latent code generation
The codes are obtained and modified from this Jupiter Notebook file: https://colab.research.google.com/drive/1jrSki9OXahtnS2Okcf7_ubvLkPnboJey#scrollTo=JaAu5s2Z5Ag2

You need to generate latent codes first. Then, you need to install the libraries.
To install the libraries and run them, you need Anaconda.
After installing Anacoda, to avoid problems with installing libraries, run the following commands on Terminal or Anaconda Command Prompt:

conda config --add channels conda-forge 

conda config --set channel_priority strict


For the latent code generation, build latent environment with this command:

 
conda env create -f latent.yml

 
After finishing the installation, activate the environment and go to the latent folder:

 
conda activate latent
cd latent
 
 
Then align the images of the dataset. You can change the size to 128, 512, or 1024:
 
 
python align_images.py CelebA/ aligned0/ --output_size=256
 
 
Run the following codes sequentially to get generated0 and latent0 folders and files:
 
 
python align_images.py CelebA aligned_images --output_size=256
 
 
python encode_images.py --optimizer=adam --lr=0.02 --decay_rate=0.95 --decay_steps=6 --use_l1_penalty=0.3 --face_mask=True --iterations=400 --early_stopping=True --early_stopping_threshold=0.05 --average_best_loss=0.5 --use_lpips_loss=0 --use_discriminator_loss=0 --output_video=False aligned0 generated0  latent0
 
 
Step 2: StyleGAN generated photos:

 Build the following environment:
 
conda env create -f StyleGANCelebA08.yml

 
Activate it and go to the corresponding folder:

 
conda activate StyleGANCelebA08

cd StyleGANCelebA08

 
Make this folder stylegan_results and copy and paste these ready folders from latent section to it: aligned0, generated0, and latent0. (I already pasted them there.)

 
Now run the following command:

 
python generate08.py

 
The generated faces for StyleGAN0-0 to StyleGAN0-8 will be available in data0 folder.
If the code is interrupted, you can resume it by just running the above code.

 
You can change generate08.py file to choose different auxiliary photos (They are t1 and v1 now) or different StyleGANs by commenting or uncommenting the corresponding lines. The generated pictures will have the original file name plus the auxiliary code in their names which can help later to distinguish them. 

CelebA is sampled from the original CelebA dataset. You can see, download, and read the annotations and labels from their website. 
 
 
 

