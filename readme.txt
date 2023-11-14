----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Background
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Running
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
* Create an input and output folder and adjust the paths in the first code chunk
* Put the .ab1 files in the input folder, and also include a metadata .xlsx file

This metadata file needs to contain the following information:

Sheet "sangerseqno_dnasample" 
	with column "SangerSeqNo" containing the name of the ab1 files, without the extention
	with column "DNASample"  containg the name of the sample / individual
	with column "PrimerName" containing the name of the primer
	with column "Locus" containing the name of the locus. This will be used as grouping variable, so adding a different value here will prevent combining the data in the summarized output.
Sheet "DSB"
	with column "Flank_A_end" - currently not in use
	with column "distance_search_term_to_DSB" - currently not in use
Sheet "reference"
	with column "Name" containing the name of the reference sequence
	with column "Sequence" containing the reference sequence. This should be the sequence of the amplicon that you sequenced
Sheet "primers"
	with column "PrimerName containing the name of the primers. should be the same as in the first sheet
	with column "PrimerSeq" containing the sequence of the primer
	with column "Orientation" containing the orientation of the primer relative to the reference sequence
Sheet "dnasample_parents"
	with column "T2" containing the name of the T2 progenitor
	with column "F1" containing the name of the F1 progenitor
	with column "F2" containing the name of the F2 progenitor
	with column "DNASample" containing the name of the individual / sample. should be the same as in the first sheet.
	with column "Genotype" containing the genotype of the individual
Sheet "gt_sites"
	with column "Name" containing the name of the site in which the repair template differs from the reference
	with column "Location" containing the position of the (first base of the) site relative to the start of the reference
	with column "GT" containing the sequence of the site (may contain more than just the differential bases)
	with column "Insertion" containing an integer of how many more bases the repair template contains at this site (use 0 for substitutions) 
Sheet "filter"
	this contains information about certain site - primer combinations that you don't wish to analyse
	with column "Name" containing the name of a site
	with column "PrimerName" containing the name of a primer

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Output
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
An xlsx file will be created with three tabs "Data_detailed", "Data_compact", and "Data_summarized". "Data_detailed contains information per site on each row. "Data_compact" contains information per ab1 file on each row. And "Data_summarized" contains summarized information per DNA sample per locus on each row.

Data_detailed

"Seq1" is the sequence of the primary peaks (matching the reference if possible) at the site in question. "not found" indicates that either the whole sequence is of poor quality, or the 20bp right prior to the site in question is deviating from the reference too mutch.
"Seq2" is the sequence of the secondary peaks (the highest peaks above a certain cutoff (GT_peak_ratio) that can be set in the second code chunk.
"WT_Amp" is the amplitude of the peaks at the site that match the reference. Only positions where the repair template differs from the reference are included.
"GT_Amp" is the same as "WT_Amp", but for peaks matching the repair template.
"AmpRatio" is GT_Amp / (WT_Amp+GT_Amp)
"WTseq" is the reference sequence at the site
"GTseq" is the repair template sequence at the site
"SangerSeqNo" is the name of the ab1 file without the extention
"Prim_Seq" is the full sequence of the ab1 file, primary peaks only.
"Sec_Seq" is the full sequence of the ab1 file, secondary peaks only.
"Seq1_genotype" is the interpretation of "Seq1". Basically "wt" means Seq1 == WTSeq, "GT" means Seq1 == "GTSeq", and "mut" means there is a different sequence. If there are any degenerate bases, then the program combines Seq1 and Seq2 and then tries to find WTSeq and GTSeq. It is "undetermined" in the case "Seq1" == "not found".
"Seq2_genotype" is similar to "Seq1_genotype", but about "Seq2".
"Genotype" is a combination of "Seq1_genotype" and "Seq2_genotype". Can be "wt", "GT/wt", "GT", "mut", and "undetermined. Note that "wt" or "GT" could mean homozygosity for wt or GT alleles, but note that large deletions or insertions prevented the amplitifaction of the second allele.
"Strict" indicates whether the program was run on strict mode or not.
"DNASample" shows the name of the DNA sample, taken from the metadata input file.
"PrimerName" shows the name of the primer that was used for sequencing, taken from the metadata input file.
"Locus" shows the name of the locus/amplicon, taken from the metadata input file.

Data_compact

contains elements in Data_detailed, but also contains parental information, taken from the metadata input file.

Data_summarized

Contains summarized interpretations of the data. "mut" trumps "undetermined"; "mut" can be the real situation, while "undetermined" is caused by a lack of information. "wt", "GT/wt", and "GT" trump "mut" and "undetermined; this is done because "undetermined" but also sometimes "mut" outcomes are due to bad sequencing quality. "GT/wt" trumps "wt" and "GT"; this is done because sequencing from one side may only "see" one allele. 
