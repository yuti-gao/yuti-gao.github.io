Latt week I attended a workshop "Fixing Your Files data management", I had a strong motivation to be there because I've realized how messy all my data is. 

I worked with genomics data, but I also have other dataset that are dental, cranial and dietary measurements. 

(In this post, I will start from some general rules covered in the workshop, then talk about my own practice. )

In this post, I will talk about my practice (after combining what covered in workshop) in different locations. 

	1) HPC Cluster (High-Performance Computing Cluster)
	One day the folder that we share as a lab is out of space, so we need to delete some files. I saw how organized my labmate's cluster folder is. 
	于是我痛改前非。 This is how my overall folder looks like now.  
	
	[yuga3894@login-ci5 y_gao_data]$ ls                                                                                    
	HumanLHC  Kraken2_Standard  Kraken2_Standard_16  Monkey
	
	Human LHC: side project. 
	Monkey: dissertation. 
	Kraken2_Standard  Kraken2_Standard_16 是两个项目都会用到的所以放在平行的位置。
 
	[yuga3894@login-ci5 Monkey]$ ls                                                                                        
	0_fastq_gz       2_fq_after_fastp      4_Kraken2_filtered_outputs  4_MetaPhlan4_nofilter  macaca_fascicularis  scripts 
	1_demultiplex    3_fq_after_bwa_EB_LB  4_Kraken2_outputs           5_check                out_files                    
	1_fq_after_demx  3_refgenome           4_MetaPhlan4_filtered       aMeta                  readme.md   
	
