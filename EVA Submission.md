# Submitting the variant data to the EVA
The code below is for submitting the variant data (i.e., vcf file) to the European Variation Archive ([EVA](https://www.ebi.ac.uk/eva/))

I first made sure that the sample name in the VCF file matched the BioSample ID (**_Asap-001_**), and that the mitochondrial contig (NC_014690.1) was removed. The code for this can be found in [Alignment and Variant Calling](./Alignment%20and%20Variant%20Calling.md). Next, I had to remove the following line (was offending to the eva-submit software):

```bash
# Unzip the vcf file
gunzip Asap001.filtered.ann.noMito.vcf.gz

# Used vim to delete the following line and saved
##ALT=<ID=*,Description="Represents allele(s) other than observed.">

# Recompressed the vcf
bgzip Asap001.filtered.ann.noMito.vcf
```

Next, I installed the [eva v0.4.7](https://github.com/EBIvariation/eva-sub-cli) submission software using conda, validated the VCF, then submitted it to the EVA.

```
# Install eva-sub-cli
conda create \
   -n eva \
   -c conda-forge \
   -c bioconda \
   eva-sub-cli
conda activate eva

# Validate via eva-sub-cli
eva-sub-cli.py \
   --metadata_xlsx \
   Asap-001_EVA_Submission.V2.0.1.xlsx \
   --submission_dir . \
   --tasks VALIDATE \
   --debug

# Submit via eva-sub-cli
eva-sub-cli.py \
   --metadata_xlsx Asap-001_EVA_Submission.V2.0.1.xlsx \
   --submission_dir . \
   --tasks SUBMIT \
   --debug \
   --username Webin-70646 \
   --password '##########'
```

