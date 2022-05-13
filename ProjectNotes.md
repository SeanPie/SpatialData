#Try displaying the rendered blob

    cd gen811

#Created a virtual Conda environment for the project:

    conda create -n spatial
    conda activate spatial

#Installed important packages to virtual environment:

    conda install -c conda-forge parallel
    conda install -c bioconda sra-tools

#Downloaded fastq data from NCBI SRA database using parallel

    parallel wget https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR156548{}/SRR156548{} ::: 44 45 46 47 48

#Downloaded read data is from 5 samples from the paper, 1 control mouse and 4 experimental mice

#Make directories for each tissue sample and move each fastq run to the right folder

    mkdir {4hours,12hours,2days,6weeks,sham}
    mv SRR15654844 sham     #for each tissue sample 

#Convert files from SRA data to fastq files

    fastq-dump --split-files /SRR15654844     #For each tissue sample

    mkdir Images
    cd Images

#Downloaded images for kidney tissue for each mouse

    wget -O 2day_he.tiff https://www.rebuildingakidney.org/hatrac/resources/gene_expression/processed_images/2021/09/17-E9NR/2days_158_HE-s0-z0-c0.ome.tif:BMRMW7ONOS6HY2WZAOMLH5NMEM?uinit=1&cid=record #2 days tissue sample
    
    wget -O 12hour_he.tiff https://www.rebuildingakidney.org/hatrac/resources/gene_expression/processed_images/2021/09/17-E9NG/12hr_140_HE-s0-z0-c0.ome.tif:REW2KTSUSG73ZS6CIL2LPL5PWE?uinit=1&cid=record #12 hours tissue sample

    wget -O 4hour_he.tiff https://www.rebuildingakidney.org/hatrac/resources/gene_expression/processed_images/2021/09/17-E9NC/4hr_115_HE-s0-z0-c0.ome.tif:YH2T2SY2YDJ2EIPUBPSFIVVTMI?uinit=1&cid=record #4 hours tissue sample

    wget -O sham_he.tiff https://www.rebuildingakidney.org/hatrac/resources/gene_expression/processed_images/2021/09/17-E9NA/sham_137_HE-s0-z0-c0.ome.tif:2AKLPIUY2TPQ7AFYC6KLN3GH6Q?uinit=1&cid=record #sham control tissue sample
    
    wget -O 6week_he.tiff https://www.rebuildingakidney.org/hatrac/resources/gene_expression/processed_images/2021/09/17-E9NY/6wks_110_HE-s0-z0-c0.ome.tif:6WDVGHH36H7FC36HZB34NWLXLU?uinit=1&cid=record #6 weeks tissue sample

    mkdir ../Spaceranger
    cd ../Spaceranger
    mkdir Output    #Folder for SpaceRanger output files

#Download and install (unpack) SpaceRanger (1.3.1)

    wget -O spaceranger-1.3.1.tar.gz "https://cf.10xgenomics.com/releases/spatial-exp/spaceranger-1.3.1.tar.gz?Expires=1652485053&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9jZi4xMHhnZW5vbWljcy5jb20vcmVsZWFzZXMvc3BhdGlhbC1leHAvc3BhY2VyYW5nZXItMS4zLjEudGFyLmd6IiwiQ29uZGl0aW9uIjp7IkRhdGVMZXNzVGhhbiI6eyJBV1M6RXBvY2hUaW1lIjoxNjUyNDg1MDUzfX19XX0_&Signature=KhWzVM4mRkVp74-2fUYpO2QJzch3OrHG9onZJhOVYjJNHrUluskOQb3zuAe5ZN6d6oYSfJl1u8izmjjCqpvd5-UV8QG1e0fPC416Zd3rVJWk6e4awT0WLvvGbiyi0vZ2BXSJk8UuAjKLhy8jEcDExtHiF1A85OWQA8y~j~fWLFBOKGqrwtjd567GNcoEFV3M32xGkslV17D74~mG0T9gn5VgdYQwBL72Sl1-qIVtbk67JNOOCF08Vknnh3cOXk~RkXYSvTCu1-pQZc6mbRzwblOFDZ4xENu6rV2k5LZI9AN0Nm0LyLOl1xBU9nsYARLIqsw0hKbGJzfId6S5AlZmwQ__&Key-Pair-Id=APKAI7S6A5RYOXBWRPDA"

    tar -xzvf spaceranger-1.3.1.tar.gz   #untar folder

#Download and unpack reference data

    wget https://cf.10xgenomics.com/supp/spatial-exp/refdata-gex-GRCh38-2020-A.tar.gz

    tar -xzvf refdata-gex-GRCh38-2020-A.tar.gz

#Bash script to run SpaceRanger count function on each tissue sample

    #!/bin/bash
    spaceranger-1.3.1/spaceranger count --id=2day_processed \ #Output directory
                   --transcriptome=/refdata/GRCh38-2020-A \ #Path to Reference
                   --fastqs=../gen811/sham/ \ #Path to FASTQs
                   --sample=SRR15654844 \ #Sample name from FASTQ filename
                   --image=/gen811/Images/sham_he.tiff \ #Path to brightfield image 
                   --unknown-slide #Default spot positions

#Repeated command for each tissue sample

#Move spaceranger output to Spaceranger/Output/

    mv 2day_processed Output     #Repeat for each sample

    cd /Output/2day_processed/outs

#Downloaded "cloupe.cloupe" file from outs folder with cyberduck
#Downloaded Loupe Browser from 10X genomics (https://support.10xgenomics.com/single-cell-gene-expression/software/downloads/latest#loupe) to view cloupe file 

#View of the 2days_processed kidney file inside the Loupe Browser with premade clusters based on SpaceRangers analyses
![](/811_1.png)

#View of the barcoded spots in the kidney file clustered into a t-SNE plot (Cluster colors should be the same)
![](/811_T.png)

#View of spots clustered into a UMAP plot
![](/811_U.png)
