# In class assignment for workshop-11

For this week's workshop, we'll be doing the first step in any sequencing pipeline (assuming you're starting from scratch) - making fastq files from Illumina BCL files!

---
### **DO NOT SKIP THIS PORTION.** You MUST have already completed the prerequesites for this workshop:
1. Created an PMACS HPC account and already gone through the tutorial (or are experienced working in UNIX-based environments). You will also need to install iTerm.
2. Git tutorial video
3. Illumina tutorial video
4. Conda
---

## First, let's talk a little about what we learned...
1. What is Conda? And why do we use it?
2. What is Git? And why do we use it?
3. How does Illumina sequencing by synthesis work? How did your BCL files come to existence?

## Next, let's identify a sequencing set that you will be working on...
1. Identify your project of interest (already sequenced). If you don't have one, let me know and I'll provide you one!

## Alright let's begin!
1. Log in to the HPC - make sure you're logging into the address with `consign` and not `mercury`.
2. Enter an interactive session.
3. Check which directory you're in. If you're not in your home directory, `cd` in your home directory.
4. Make a new directory called `pkg` and enter into it.
5. We're going to be installing the mamba wrapper of conda (which is much speedier compared to conda).
    - Go to https://github.com/conda-forge/miniforge.
    - Download the installation script: `curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"`
      - What does `curl` do?
      - What do the two flags mean that we're using in this command?
      - Take a look at the installation script and see what's in there. Remember that someone could write nefarious code which you might execute in the next step. **Always take a look at scripts before you execute them.**
    - Run the installation script: `bash Miniforge3-$(uname)-$(uname -m).sh`
    - Let's make sure it installed correctly and doesn't have any errors: `mamba --version`
6. We are now going to create a new environment. This will allow us to install packages with relative ease and keep track of which versions are installed. For our purposes, this will be the environment for your single-cell processing pipeline.
    - `mamba create -n your-env-name` will create a new mamba environment for you. Make sure to follow the prompts to create this environment.
    - Note that when we create this environment, we don't actually enter it automatically. You will now need to enter `mamba activate your-env-name` to be running from this environment. You'll notice that your command line prompt will change to reflect this.
    - Because we're doing mostly bioinformatic stuff, we will need to add some channels so that commonly usesd packages are available.
    ```{bash}
    conda config --add channels defaults
    conda config --add channels bioconda
    conda config --add channels conda-forge
    conda config --set channel_priority strict
    ```
7. We won't actually be installing any programs today since the only program we need today is already pre-installed on the HPC (and we won't have to compile from source and deal with that today...). Normally, I recommend using mamba to install packages as opposed to loading from the HPC. If you type `module load bcl2fastq2/v2.20.0.422`, this will make bcl2fastq2 accessible to you. Typing `bcl2fastq --version` will demonstrate that this is available to you (note that the program invocation does not have the number 2 at the end).
8. We will be using bcl2fastq2 to make fastq files for any modality that we can't use cellranger. You will need to be familiar with both cellranger and bcl2fastq2. Depending on your assay/dataset, go to 10X Genomics and install the correct cellranger package into your `~/pkg/` directory. Cellranger will also require bcl2fastq2 to be installed to make fastqs as well.
9. When you're done, take a breather! Now, we have to deal with sample sheets.

## Make your samplesheet
1. On the HPC, make a new directory labeled for this particular experiment. You will likely reorganize your directory as you become more comfortable with your setup.
2. For cellranger-arc, cellranger-atac, or cellranger mkfastq - you will need a simple samplesheet. Use your library indexing spreadsheet to construct a `simple samplesheet` as detailed in the 10X Genomics documentation. Either upload your simple samplesheet to your new directory using `scp` or use `Vim` to construct it in the HPC.
3. Using the documentation for your particular flavor of cellranger, write a sample bash script to execute your mkfastq script. Do NOT actually run your bash script yet.
    - I will provide you the path for the raw BCL files.
    - Your output folder for fastq files will be here: `/project/bettslab/students/<your-name>/<project-name>/<sequencing-date>/fastq`
4. After your script has been validated, you will be submitting a job using the LSF job submission. Use the LSF documentation to initiate the following requirements:
    - Use 8 cores
    - Use 64GB of RAM
    - Save the stdout into a file.
    - Save the stderr into a file.
    - To trigger an email notification, add these flags `-N -u <your-email-address>`
5. Now we will try making fastq files using Illumina's bc2lfastq. This step is needed for libraries that are not directly compatible with cellranger mkfastq. This typically includes the ADT/HTO libraries. More information will be provided in class depending on time.

## Summary
By the end of today, we will have accomplished the following: 
1. Understand why/how we use Conda/Mamba.
2. Install Mamba and set up a new Mamba environment.
3. Install cellranger
4. Convert bcl to fastq using cellranger mkfastq.
5. Convert bcl to fastq using Illumina bcl2fastq (depending on time).
6. Understanding what/how BCLs are made.

We will also have practiced:
1. Operating in Unix environment
2. Using Vim
3. Submitting jobs using the LSF job submission system