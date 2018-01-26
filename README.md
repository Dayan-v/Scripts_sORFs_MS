# A collection of python scripts (v3.5) used for [P.patens sORF discovery work](https://www.biorxiv.org/content/early/2017/11/03/213736). 
The corresponding MS is currently under revision and is available here: https://www.biorxiv.org/content/early/2017/11/03/213736
#### GffParser.py 
__Description__: script to find introns in P.patens gff3 file.

#### protein_Ka_Ks_codeml.pys
__Description__: script to calculate dn/ds value. Basically, it takes two fasta files: proteins corresponding to sORF, transcript nucleotide sequences from other species. Usage: see  --help. Example:  `python3 protein_Ka_Ks_codeml.py translated_FINAL_sORF_SELECTED.fa Zmays_284_Ensembl-18_2010-01-MaizeSequence.transcript.fa  --blast T --threads 10 --makedb T`

   Dependencies:

  * perl, python, biopython
  * pal2nal perl script, available here https://github.com/HajkD/orthologr/tree/master/inst/pal2nal/pal2nal.v14.
    !!! set `pal2nal` variable to run pal2nal script e.g. `pal2nal=r"perl ./pal2nal.v14/pal2nal.pl"` (default)

  * blast. Please, if it is required, change `blast` variable in class e.g.  `blast=r"tblastn"` (default)
  * codeml. .ctl file can be found in this directory
  * clustalo
  * codeml Parser module (it is located at [codemlParser directory](https://github.com/Kirovez/Scripts_sORFs_MS/tree/master/codemlParser))

   Change variable `query_seq_ind` to the path to the file with sORF nucleotide sequences
e.g. (default) `query_seq_ind = SeqIO.index("/home/ilia/sORF/sORFfinder2/moss/FINAL_sORF_SELECTED.fa", "fasta")`

#### oopBLASTv_forsORF.py__ script. Available in this repository

!!! NOTE !!!It requires some variables have to be manually changed. See info in the script files for details.  One of the variables corresponds to the fasta file containing nucleotide sequences of sORFs 
(Important! id of the protein and nucleotide sORF sequences MUST be identical). 	

Scripts required for protein_Ka_Ks_codeml.py:
   * 2.1 codemlParser folder contains module to run and parser Codeml program
   * 2.2 oopBLASTv_forsORF.py - script to run blast and parse the results.
Usage example:
class takes query and reference fasta files
Bl = BlastParser(r"/home/ilia/sORF/sORFfinder2/moss/FINAL_sORF_SELECTED.fa", r"/home/ilia/SOLID_moss/Ppatens_318_v3_index.fa", DB_build=False)

# run BLAST
Bl.runblast()

# the function takes xml file. Default name for this file: <query file name>_vs_<hit file name>. 
Bl.parseBlastXml() 
It can also be used to parse external xml file. E.g. 

parseBlastXml(file_exo="extranal_xml_blast.xml")
 
It returns .bed file with following columns:
	1. Query id
	2. Hit id
	3. Blank column (0 value)
	4. Start HSP in hit (start position of hit sequence involved in alignment)
	5. Stop HSP in hit (stop position of hit sequence involved in alignment)
	6. E-value,  
	7. hit HSP sequence, 
	8. query HSP sequence
	9. length of hit HSP
	10. hit strand
	11. Start HSP in query (start position of query sequence involved in alignment)
	 12. Stop HSP in query (stop position of query  sequence involved in alignment)

3. KaKsloop.py - example script which can be used to run protein_Ka_Ks_codeml.py for set of reference sequences
4. sORF_completeness_v2.0.py - script to estimate changes in homologous sORF length between different species. It takes genome fasta file, fasta file protein translated sORFs and bed table created by oopBLASTv_forsORF.py script         return table with 18 column (actually the first 13 columns are identical to the input bed file):
	1. Query id
	2. Hit id
	3. Blank column (0 value)
	4. Start HSP in hit (start position of hit sequence involved in alignment)
	5. Stop HSP in hit (stop position of hit sequence involved in alignment)
	6. E-value,  
	7. hit HSP sequence, 
	8. query HSP sequence
	9. length of hit HSP
	10. hit strand
	11. Blank column
	12. Start HSP in query (start position of query sequence involved in alignment)
	13. Stop HSP in query (stop position of query  sequence involved in alignment)
	 14. Predicted start Codon coordinates of homologous sORF
	 15. Predicted stop Codon coordinates of homologous sORF 
	 16. Premature Stop Codon (- no PSC found)
	 17. Predicted length  of homologous sORF (if 0 – premature stop codon found before (upstream) HSP start), aa
	 18. sORF query length, aa
5. sORFfastaToBed2.py 
	Parse sORFfinder output file to generate bed file

	positional arguments:
	  infile          name of sORF fasta file generated by sORFfinder
	  genomFile       name of target genome fasta file used for sORFfinder
	  outputfileName  number of threads

	optional arguments:
	  -h, --help      show this help message and exit

	It writes a table with following columns:
	1.	Chromosome name
	2.	Start position
	3.	End position
	4.	Strand
	5.	Coding index
	6.	sORF name

