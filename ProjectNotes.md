Created a virtual Conda environment for the project:
    conda create -n spatial
    conda activate spatial
Installed GNU Parallel for simultaneous downloading:
    conda install -c conda-forge parallel
Downloaded fastq data from NCBI SRA database using parallel
    Website:
        https://trace.ncbi.nlm.nih.gov/Traces/sra/?run=SRR16946491
    Command:
        parallel wget https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR169464{}/SRR169464{} ::: 88 89 90 91
            Downloads read data from 4 samples from the paper, 2 control mice and 2 experimental mice
Downloaded images for lung tissue for each mouse
    