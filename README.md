# Entrez-Tutorial
Last updated: December 15th, 2019

This is a tutorial for searching and downloading nucleotide sequences off of NCBI with Entrez.
You need to know the organism searching for, and the name of the gene.


## Esearch
First get the ncbi taxon ID number. This can be done by searching the latin name in the nucleotide database through the website gui    
For example, Toxocara canis number is txid6265    

The first command uses esearch, which searches the database and returns the number of records for that search    
Instructions for installation can be found at: https://www.ncbi.nlm.nih.gov/books/NBK179288/    
-db and -query are both required flags. -db specifies the database to search in, and the -query is a specified string    

`>esearch -db nucleotide -query "txid6265"`

This returns:    

`<ENTREZ_DIRECT>
  <Db>nucleotide</Db>
  <WebEnv>NCID_1_40401709_130.14.22.215_9001_1545260591_330183727_0MetA0_S_MegaStore</WebEnv>
  <QueryKey>1</QueryKey>
  <Count>2779</Count>
  <Step>1</Step>
</ENTREZ_DIRECT>`

Where the number of records is 2779. This can be corroborated by searching for Toxocara canis on the ncbi nucleotide database online, which also returns 2779 records    

## Efetch

Esearch is most powerful when piped to efetch, which actually grabs the records from the database and either outputs to screen or can redirect to a file    

For example,    

`>esearch -db nucleotide -query "txid6265" | efetch -format fasta > output.fasta`

This will take all sequences and send them to a file called output.fasta, in fasta format.     

## Parse the file with seqtk
(Seqtk can be downloaded at: https://github.com/lh3/seqtk)    
Unfortunately, the sequences will be on multiple lines, making it harder to parse the sequences and search/extract with grep or other programs. To remedy this, use seqtk to convert from multi-line fasta file to single line:    

`>seqtk seq -10 output.fasta > single.fasta`

## Search for target with grep 

Then, to search specific gene, search the single-line fasta file for the name of the locus you're looking for    
The -A option flag will return the number of lines indicated AFTER the match which, in our case, is the sequence    

`>grep -A 1 "ITS2" single.fasta > Toxocara_canis_ITS2.fasta`

Returns:    

`>LC328970.1 Toxocara canis P3 genes for 5.8S rRNA, ITS2, partial sequence
>LC328968.1 Toxocara canis P1 genes for 5.8S rRNA, ITS2, partial sequence
>LC133352.1 Toxocara canis genes for 5.8S rRNA, ITS2, partial sequence, clone: N1, N2, N3
>AB819330.1 Toxocara canis gene for ITS2, partial sequence, isolate: AhvTCAN4
>AB819329.1 Toxocara canis gene for ITS2, partial sequence, isolate: AhvTCAN3
>AB819328.1 Toxocara canis gene for ITS2, partial sequence, isolate: AhvTCAN2
>AB819327.1 Toxocara canis gene for ITS2, partial sequence, isolate: AhvTCAN1
>AB743617.1 Toxocara canis ITS2 gene for antigen protein, partial sequence, isolate: KHuzTcan2
>AB743616.1 Toxocara canis ITS2 gene for antigen protein, partial sequence, isolate: KHuzTcan2
>AB743615.1 Toxocara canis ITS2 gene for antigen protein, partial sequence, isolate: KHuzTcan2
>AB743614.1 Toxocara canis ITS2 gene for antigen protein, partial sequence, isolate: KHuzTcan1`

That's it! :)    



