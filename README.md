# Proteogenomics_Salmon
	Identification of proteogenomic events
	Search LC-MS/MS spectra against the reference protein database (e.g., GCF_000233375.1_ICSASG_v2_protein.faa) using MS-GF+1 or a preferred database search algorithm.
	Retain spectra that do not pass a 1% PSM-level FDR (i.e., “unidentified spectra”) for a multi-stage-search FDR strategy2.
	Search unidentified spectra against a Splice graph database (SpliceDB)3-4 using MS-GF+1 or another preferred database search algorithm.
	The construction of a Splice graph database (SpliceDB) requires RNA-seq reads aligned to the reference genome in SAM format and the genomic sequences of chromosomes in the assembly in FASTA format.
	Run Enosi5-8 to identify proteogenomic events.
	Enosi requires i) the SpliceDB used in Step 1c, ii) identifications (PSMs) from the SpliceDB search in Step 1c, iii) MS-GF+ PepQValues for un-modified peptide sequences, and iv) the reference genome annotation (e.g., GCF_000233375.1_ICSASG_v2_genomic_with_utrs.gff).

	Collection of hints for Augustus9 (gene prediction software)
	Reference peptide identifications from database search in Step 1a. 
OR
Reference and Ensembl peptide identifications from an additional database search against the reference protein database appended with proteogenomic event-supported Ensembl protein sequences.
	To determine the genomic coordinates of peptide identifications, a straight-forward approach is to first perform exact-matching of the non-modified peptide sequence to protein sequences in the reference database. As the peptide sequence is a sub-sequence of the matched protein sequence, the genomic coordinates of the peptide can be inferred from the genomic coordinates of the coding sequence (CDS) features of the parent gene as listed in the reference genome annotation file.
	Proteogenomic event genomic coordinates (Step 1d. Enosi output, event.txt).
	Reference coding sequence (CDS) features within a reasonable distance (e.g., ±60,000 bp) from a proteogenomic event.
	Ensembl CDS features within a reasonable distance from a proteogenomic event.
	Blastx High-scoring Segment Pairs (HSPs; e-value ≤ 1e-10). 
	Submit sub-sequences of the reference genome, defined as the nucleotides spanning the genomic coordinates of identified proteogenomic events and 350bp of flanking sequences, to Blastx for a six-frame translation and comparison against protein sequences in the non-redundant protein sequences (nr) database.

	Augustus9 gene/transcript prediction
	Generate a hints.gff file for each proteogenomic event that includes all hints within a reasonable distance (e.g., ±60,000 bp) from the start and end genomic coordinates of the proteogenomic event. Refer to Augustus8 documentation for best practices.
	Run Augustus.
	Example command:
 
	Collect comparative genomics evidence (Blastp).
	For predicted genes and transcripts, submit the corresponding protein sequences to Blastp for comparison against protein sequences in the nr database. Blastp hits with significant alignments (e.g., e-value ≤ 1e-10, alignment ≥10 amino acids) may be considered as orthologs in support of the predicted gene structure.
	Select a final list of gene/transcript predictions.
	Augustus may return hundreds of gene/transcript predictions. Determining if a prediction supports an Ensembl annotation, correction to a reference annotation, or represents a novel gene is a manual task that requires careful inspection of evidence.

	Creation of normalized PSK expression matrix
	Search spectra against an enhanced database containing reference proteins, Augustus predictions, and proteogenomic event-supported Ensembl proteins.
	Identify proteins at a 1% protein-level FDR.
	To compute a protein-level FDR, proteins are scored based on precursors (combination of peptide sequence and charge) passing a 1% level FDR, where the score of a precursor is defined as the SpecEValue of its best scoring PSM. Specifically, the score of a protein is initialized as the sum of the negative log of the SpecEValue of its precursors. After choosing the highest scoring protein, precursors to that protein are removed and protein scores are recalculated. This process is iterated until no precursors are left. A protein false discovery rate is then computed as the number of decoy proteins/target proteins.


	Compute normalized PSK expression values.
	Raw spectral counts, Sp, for identified proteins (1% protein-level FDR) in each sample are based on spectra passing a 1% PSM-level and 1% precursor-level FDR, where cp is the number of PSMs representing peptide, p, mapping to a protein, P, and N(p) is the number of proteins mapping to a peptide p. 
