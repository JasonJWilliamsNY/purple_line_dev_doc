|CyVerse logo|_

|Home_Icon|_
`Learning Center Home <http://learning.cyverse.org/>`_


Walkthrough of DNA Subway Purple Line (alpha testing documentation)
---------------------------------------------------------------------
The Purple Line provides the capability for analysis of microbiome and eDNA (
environmental DNA) by implementing a similified version of the `QIIME 2 <https://qiime2.org/>`_ (p
pronounced "chime") workflow. Using Purple Line, you can analyze uploaded
highthroughput sequencing reads to get details on microbial populations or
species identification from enviornmental DNA samples.

Metabarcoding uses next generation sequencing to analyze hundreds of thousands
of DNA barcodes from complex mixtures of DNA-representing microbes (microbiomes)
or environmental DNA (eDNA). In a typical experiment, DNA is isolated from
sterile swabs or material taken from different environmental locations. PCR is
used to amplify a variable region, such as COI, or 12S or 16S ribosomal RNA
genes, and NGS reads identify the variety and abundance of microbial species
from different locations. The analysis requires specialized software, such as
QIIME 2.

The Purple Line supports QIIME 2 analyses of microbiomes with 16S rRNA barcodes
and eDNA analysis of fish from water samples. For each, the Purple Line manages
data input from Cyverse's Discovery Environment, metadata upload, demultiplexing
of samples, quality control, and taxonomic identification and quantitation.
Once sequences are analyzed, the results can be visualized to allow comparisons
between samples and different conditions summarized in the metadata.



**Some things to remember about the platform**

- You must be a registered user to use Purple Line. (register for a CyVerse account at `user.cyverse.org <https://user.cyverse.org/>`_)
- The Purple line was designed to make microbiome/eDNA data analysis "simple".
  However, we ask that users very carefully and thoughtfully decide what "jobs"
  they want to submit.
- A single Purple Line project may take a XXXX [TIME ESTIMATE]to process since
  HPC computing is subject to queues which hundreds of other jobs may be staging
  for. Additionally these systems undergo regular maintenance and are subject to
  periodic disruption.
- DNA Subway implements the `QIIME2`_ software. This software is in continual
  development. Our version may not be the most current, and our documentation
  and explination is not meant to replece the full `QIIME2 documentation <https://docs.qiime2.org/2018.2/>`_
- We have made design decisions to create a straight-foward classroom-freindly
  workflow. While this Subway does not have all possible features of QIIME, we
  belive the important concepts behind microbiome and eDNA analysis are
  embodied in the current Subway architecture.


----

 .. admonition:: Sample Data
     In this guide, we will use sample data collected from Montana [MORE INFO]
     Where appropriate, a note in the instructions will indicate which options
     to select so that you can work with this test data.


*DNA Subway Green Line - Metadata file and Sequencing Prerequisites*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*DNA Subway Green Line - Create an RNA-Seq Project to Examine Differential Expression*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**A. Create a project in Subway**

  1. Log-in to `DNA Subway <https://dnasubway.cyverse.org/>`_ - unregistered
     users may NOT use Purple Line. (register for a CyVerse account at `user.cyverse.org`_)

  2. For 'Select Project Type' select either Single End Reads or Paired End
     Reads

     .. note::
        For sample data in this guide select **Single-end**


  3. For 'Select File  Format' select the format the corresponds to your sequence metadata.

     .. tip::
         Typically, microbiome/eDNA will be in the form of multiplexed FastQ
         sequences. We support the following formats:

         - `Illimina Casava 1.8 <https://support.illumina.com/content/dam/illumina-support/documents/myillumina/33d66b02-53b5-4f4d-9d8b-f94237c7e44d/casava_qrg_15011197b.pdf>`_
         - `Earth Microbiome Project <http://www.earthmicrobiome.org/protocols-and-standards/>`_

     .. note::
        For sample data in this guide select **Illimina Casava 1.8**

  4. Enter a project title, and description; click 'Continue'

**B. Upload Read Data to CyVerse Data Store**
The sequence read files used in these experiments are too large to upload using
the Subway internet interface. You must upload your files (either .fastq or .fastq.gz)
directly to the CyVerse Data Store.

  1. Upload your

     - FASTQ sequence reads
     - Sample metadata file (.tsv or .txt formatted according to `QIIME2 Metadata requirments <https://docs.qiime2.org/2018.2/tutorials/metadata/>`_ )

     to the CyVerse Data Store using Cyberduck. See instructions:
     `CyVerse Data Store Guide <https://cyverse-data-store-guide.readthedocs-hosted.com/en/latest/step1.html>`_


----

*DNA Subway Purple Line - Manage Data, Demultiplex, and  Check Quality with FASTQC*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**A. Select files**

  1. Click on the “Manage Data” stop: this opens a Data store window that says
     "Select your FASTQ files from the Data Store" (if you are not logged in to
     CyVerse, it will ask you to do so); click the **add data** link.
  2. Select your metadata file: click on the folder that matches your CyVerse
     username and Navigate to the folder where your sequencing files are located.

     .. note::
        For sample metadata file in this guide select navigate to:
        Shared Data > cyverse_training > platform_guides > dna_subway >
        purple_line > montana_controls > **mappingfile_MT_corrected.tsv**

        Select the **mappingfile_MT_corrected.tsv** and then click "Add selected
        files"

  3. To validate the metadata file, click "validate sample mapping file", header
     columns will be displayed. Next click the "Validate" button.

  4. To add sequence data, click the "add data" link. Click on the folder that
     matches your CyVerse username and Navigate to the folder where your
     sequencing files are located.

     .. note::
        For sample sequence date in this guide select navigate to:
        Shared Data > cyverse_training > platform_guides > dna_subway >
        purple_line > montana_controls

        Select all 11 fastq files (they are compressed and will have the fastq.gz
        file extension). Then click "Add selected files".

  4. Click the "add data" link to add the sequencing data to your project.


  .. warning::
    **Known alpha-version bug**
    After adding data, the next stop (Demultiple reads) will still be blocked.
    Refesh DNA Subway in your browser to unblock this step.

**B. Demultiplex reads**
At this step, reads will be grouped according to the sample metadata provided.

  1. Click **demux reads** to demultiplex your sample reads.

  2. When demultiplexing is complete, you will generate a file (.qzv) click
     this link to view a visualization and statistics on the sequence and
     metadata for this project.




**B. Check sequencing quality with FastQC**
It is important to only work with high quality data. `FastQC <http://www.bioinformatics.babraham.ac.uk/projects/fastqc/>`_ is a popular tool
for determining sequencing quality.

  1. Once files have been loaded, in the 'Manage Data' window, click the 'Run'
     link in the 'QC' column to run FastQC.
  2. One the job is complete, click the 'View' link to view repeat_results

  .. note::

    **QIIME2 Visualizations**

    One of the features of QIIME2 are the variety of visualizations provided
    at several analysis steps. Although this guide will not cover every
    feature of every visualization, here are some important points to note.

    - **QIIME2 View**: DNA Subway uses the QIIME2 View plugin to display
      visualizations. Like the standalone QIIME2 software, you can navigate
      menus, and interact with several visualizations. Importantly, many files
      and visualizations can be directly download for your use outside of
      DNA Subway, including in report generation, or in your custom QIIME2
      analyses. You can view downloaded .qza or .qzv files at `https://view.qiime2.org <https://view.qiime2.org>`_




----

*DNA Subway Green Line - Trim and Filter Reads with FastX Toolkit*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Raw reads are first "quality trimmed" (remove poor quality bases off the end(s)
of a read) and then are "quality filtered" (filter out entire poor quality reads)
prior to aligning to the genome. After trimming and filtering, FastQC is run
on the trimmed/filtered files.

  1. Click “FastX ToolKit” to open the FastX Toolkit panel for all your data.
  2. For each file, under 'Basic', Click 'Run' to filter the reads using default
     parameters or click 'Advanced' to run with desired parameters; repeat this
     process for all the FASTQ files in your dataset.
  3. Once the job completes, click the 'View' link to view a generated FastQC
     report.

     .. tip::

         - Starting at this step, DNA Subway results are labeled with a job ID
           (e.g. fx####). These jobs are available in a 'DNASubway' folder
           in the home directory of your CyVerse Data Store.
         - Starting at this step, you may see 'Basic' and 'Advanced' options
           for analyses. Clicking the 'Advanced' option allows you to set
           parameters. The Parameters shown in the 'Advanced' step are the defaults
           used in the 'Basic' option.
  4. Since you may trim reads multiple times to achieve the desired quality of data
     record the job IDs (e.g. fx####) that you wish to use in the subsequent steps.


----

*DNA Subway Green Line - Map Reads to Genome with TopHat*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
TopHat is the first component of the Tuxedo Protocol. This program aligns
individual RNA-Seq reads to a previously assembled “reference” genome using a
component program called Bowtie. TopHat then uses information from the newly
mapped reads to determine what the intron/exon boundaries are for mapped
transcripts, determining their splice sites.


**TopHat Basic**

  1. For each file, under 'Basic', Click 'Run' to begin the alignment using
     default parameters. (The reads will be aligned to the reference genome
     you selected when you created your project)

  2. Repeat this process for all the FASTQ files in your dataset.


**TopHat Advanced**

  1. Click 'TopHat' to open the TopHat panel for all your data.

  2. Under 'Advanced' Click 'Run'.

  3. Set the parameters as desired; Click 'Submit' to begin the alignment using
     default parameters. (The reads will be aligned to the reference genome
     you selected when you created your project).

  4. Repeat this process for all the FASTQ files in your dataset.

    .. tip::
        We generally recommend selecting the 'No novel junctions' option
        unless you have very high-coverage data (e.g. >100 million reads for
        a ~3Gb genome).

When this step completed you can view the summary mapping statistics, or view
the aligned reads using the Integrated Genome Viewer (IGV).

----

*DNA Subway Green Line - Assemble Transcripts with Cufflinks*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Cufflinks assembles or “links” the RNA-Seq alignments into a set of transcripts
which are best estimates (determined by parsimony) of your sample’s actual
transcripts. In other words, Cufflinks makes hypotheses about how related reads
could be merged into transcripts. Cufflinks also makes estimates about the
relative abundance of each transcript.

  .. note::

    This step is optional, and can be skipped

**Cufflinks Basic**

  1. Click 'Cufflinks' to open the Cufflinks panel for all your data.
  2. For each file, under 'Basic', Click 'Run' to begin the assembly using
     default parameters. (The reads will be assembled using the reference
     genome you selected when you created your project).
  3. Repeat this process for all the FASTQ files in your dataset.

**Cufflinks Advanced**

  1. Click 'Cufflinks' to open the Cufflinks panel for all your data.
  2. Under 'Advanced' Click 'Run'
  3. Set the parameters as desired; Click 'Submit' to begin the assembly using
    default parameters. (The reads will be aligned to the reference genome
    you selected when you created your project).
  4. Repeat this process for all the FASTQ files in your dataset.

*DNA Subway Green Line - Examine Differential Expression with CuffDiff*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Cuffdiff uses the Cufflinks output (and/or or reference genome) to calculate
gene and transcript expression levels in one or more condition and tests them
for significant differences. Depending on how many replicates and conditions you
have, you may ultimately create several Cuffdiff jobs to test your desired
combinations.

  1. Click 'Cuffdiff' to open the Cuffdiff panel for all your data.
  2. Under 'Assign TopHat alignment files to samples and replicates' assign all
     of your samples (e.g. wild type, time point 1, control, etc.) to a
     grouping (e.g. 'Sample 1', 'Sample 2', etc.)
  3. For each sample, select from the drop-down menu the TopHat job
     (previously TopHat mapped reads) and their replicates that belong with
     that sample group. (you may need to review the TopHat job names from
     the TopHat step).
  4. Either click 'Submit' (Basic) to run with default parameters, or
     use the 'Advanced' link to adjust parameters.

For the result you wish to examine, click the graph icon to view a collection of
graphs that illustrate differences in expression between samples. You can also
view a table of the results, including expression levels and comparison for
each annotated gene.


More help and additional information
`````````````````````````````````````

..
    Short description and links to any reading materials

Search for an answer:
    `CyVerse Learning Center <http://learning.cyverse.org>`_ or
    `CyVerse Wiki <https://wiki.cyverse.org>`_

Post your question to the user forum:
    `Ask CyVerse <http://ask.iplantcollaborative.org/questions>`_

----

**Fix or improve this documentation:**

- On Github: `Repo link <https://github.com/CyVerse-learning-materials/dnasubway_guide>`_
- Send feedback: `Tutorials@CyVerse.org <Tutorials@CyVerse.org>`_

----

.. |CyVerse logo| image:: ./img/cyverse_rgb.png
    :width: 500
    :height: 100
.. _CyVerse logo: http://learning.cyverse.org/
.. |Home_Icon| image:: ./img/homeicon.png
    :width: 25
    :height: 25
.. _Home_Icon: http://learning.cyverse.org/
