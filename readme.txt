----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Background
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Running
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
* Create an input and output folder and adjust the paths in the first code chunk
* Add the .ab1 files to the input folder, and also a metadata .xlsx file

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
