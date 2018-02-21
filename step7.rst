|CyVerse logo|_

|Home_Icon|_
`Learning Center Home <http://learning.cyverse.org/>`_


Walkthrough of DNA Subway Purple Line (alpha testing documentation)
---------------------------------------------------------------------
The Purple Line provides the capability for analysis of microbiome and eDNA (
environmental DNA) by implementing a similified version of the
`QIIME 2 <https://qiime2.org/>`_ (pronounced "chime") workflow. Using the Purple
Line, you can analyze uploaded high throughput sequencing reads to get details
on species in microbial or environmental DNA samples.

Metabarcoding uses next generation sequencing to analyze hundreds of thousands
of DNA barcodes from complex mixtures of DNA. In a typical experiment, DNA is
isolated from sterile swabs or material taken from different environmental
locations or conditions. PCR is used to amplify a variable region, such as COI,
or 12S or 16S ribosomal RNA genes, and NGS reads identify the variety and
abundance of species from different samples. The analysis requires specialized
software, such as QIIME 2.

The Purple Line manages data input from Cyverse's Discovery Environment,
metadata upload, demultiplexing of samples, quality control, and taxonomic
identification and quantitation. Once sequences are analyzed, the results can be
visualized to allow comparisons between samples and different conditions
summarized in the metadata.



**Some things to remember about the platform**

- You must be a registered user to use Purple Line. (register for a CyVerse
  account at `user.cyverse.org <https://user.cyverse.org/>`_)
- The Purple line was designed to make microbiome/eDNA data analysis "simple".
  However, we ask that users very carefully and thoughtfully decide what "jobs"
  they want to submit.
- A single Purple Line project may take hours to process since
  HPC computing is subject to queues which may support hundreds of other jobs.
  These systems also undergo regular maintenance and are subject to
  periodic disruption.
- DNA Subway implements the `QIIME 2`_ software. This software is in continual
  development. Our version may not be the most current, and our documentation
  and explination is not meant to replece the full
  `QIIME2 documentation <https://docs.qiime2.org/2018.2/>`_
- We have made design decisions to create a straight-foward classroom-friendly
  workflow. While this Subway Line does not have all possible features of QIIME
  2, we believe the important concepts behind microbiome and eDNA analysis are
  embodied in the current architecture.


----

 .. admonition:: A Note on Sample Data

   In this guide, we will use two sample datasets

     - **Microbiome**: collected from Montana [MORE INFO]
     - **eDNA**: collected from [MORE INFO]

     Where appropriate, a note in the instructions will indicate which options
     to select so that you can work with this test data.


*DNA Subway Purple Line - Metadata file and Sequencing Prerequisites*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
For QIIME 2 to run, a valid metadata file is required. This file must conform to
strict guidelines, or analyses will fail. QIIME 2 metadata is stored in a TSV
(tab-separated values) file. These files typically have a .tsv or .txt file
extension, though it doesn't matter to QIIME 2 what file extension is used. TSV
files are simple text files used to store tabular data, and the format is
supported by many types of software, such as editing, importing, and exporting
from spreadsheet programs and databases. Thus, it's usually straightforward to
manipulate QIIME 2 metadata using the software of your choosing. If in doubt,
we recommend using a spreadsheet program such as Microsoft Excel or Google
Sheets to edit and export your metadata files.

.. tip::

  **Leading and trailing whitespace characters**
    If any cell in the metadata contains leading or trailing whitespace
    characters (e.g. spaces, tabs), those characters will be ignored when the
    file is loaded. Thus, leading and trailing whitespace characters are not
    significant, so cells containing the values 'gut' and '  gut  ' are
    equivalent. This rule is applied before any other rules described below

  **ID column**
      The first column MUST be the ID column name (i.e. ID header) and the
      first line of this column should be #SampleID or one of a few alternative.

      - Case-insensitive: id; sampleid; sample id; sample-id; featureid;
        feature id; feature-id.
      - Case-sensitive: #SampleID; #Sample ID; #OTUID; #OTU ID; sample_name

  **Sample IDs**
      For the sample IDs, there are some simple rules so that QIIME 2 does not
      get confused:

      - IDs may consist of any Unicode characters, with the exception that IDs
        must not start with the pound sign (#), as those rows would be
        interpreted as comments and ignored. IDs cannot be empty (i.e. they must
        consist of at least one character).
      - IDs must be unique (exact string matching is performed to detect
        duplicates).
      - At least one ID must be present in the file.
      - IDs cannot use any of the reserved ID column names (the sample ID names,
        above).
      - The ID column can optionally be followed by additional columns defining
        metadata associated with each sample or feature ID. Metadata files are
        not required to have additional metadata columns, so a file containing
        only an ID column is a valid QIIME 2 metadata file.

  **Column names**

      - May consist of any Unicode characters.
      - Cannot be empty (i.e. column names must consist of at least one
        character).
      - Must be unique (exact string matching is performed to detect duplicates)
        .
      - Column names cannot use any of the reserved ID column names.

  **Column values**

      - May consist of any Unicode characters.
      - Empty cells represent missing data. Note that cells consisting solely of
        whitespace characters are also interpreted as missing data.

      QIIME 2 currently supports categorical and numeric metadata columns. By
      default, QIIME 2 will attempt to infer the type of each metadata column:
      if the column consists only of numbers or missing data, the column is
      inferred to be numeric. Otherwise, if the column contains any non-numeric
      values, the column is inferred to be categorical. Missing data (i.e. empty
      cells) are supported in categorical columns as well as numeric columns.
      For more details, and for how to define the nature of the data when needed,
      see the
      `QIIME 2 metadata documentation <https://docs.qiime2.org/2018.2/tutorials/metadata/>`_

**A. Create Metadata file**

  1. Using a spreasheet editor, create a metadata sheet that provides
     descriptions of the sequencing files used in your experiment. Export this
     file as a **.txt** or **.tsv** file. following the `QIIME 2 metadata documentation`_
     recomendations.

     .. tip::

        See an example metadata file used for our sample data here:
        `Sample mapping file <http://datacommons.cyverse.org/browse/iplant/home/shared/cyverse_training/platform_guides/dna_subway/purple_line/eDNAworked/mappingfile.tsv>`_


*DNA Subway Purple Line - Create a Microbiome Analysis Project*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**A. Create a project in Subway**

  1. Log-in to `DNA Subway <https://dnasubway.cyverse.org/>`_ - unregistered
     users may NOT use Purple Line. (register for a CyVerse account at `user.cyverse.org`_)

  2. For 'Select Project Type' select either Single End Reads or Paired End
     Reads

     .. admonition:: Sample Data

        For sample data in this guide select **Paired End Reads**


  3. For 'Select File  Format' select the format the corresponds to your sequence metadata.

     .. tip::
         Typically, microbiome/eDNA will be in the form of multiplexed FastQ
         sequences. We support the following formats:

         - `Illimina Casava 1.8 <https://support.illumina.com/content/dam/illumina-support/documents/myillumina/33d66b02-53b5-4f4d-9d8b-f94237c7e44d/casava_qrg_15011197b.pdf>`_
         - `Earth Microbiome Project <http://www.earthmicrobiome.org/protocols-and-standards/>`_

     .. admonition:: Sample Data

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

*DNA Subway Purple Line - Metadata and QC*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**A. Select files using Manage Data**

  1. Click on the “Manage Data” stop: this opens a Data store window that says
     "Select your FASTQ files from the Data Store" (if you are not logged in to
     CyVerse, it will ask you to do so); click the **add data** link.
  2. Select your metadata file: click on the folder that matches your CyVerse
     username and Navigate to the folder where your sequencing files are located.

     .. admonition:: Sample Data

        For sample metadata file in this guide select navigate to:
        Shared Data > cyverse_training > platform_guides > dna_subway >
        purple_line > eDNAworked

        Select the **mappingfile.tsv** and then click "Add selected
        files"

  3. To validate the metadata file, click "validate sample mapping file", header
     columns will be displayed. Next click the "Validate" button.

  4. To add sequence data, click the "add data" link. Click on the folder that
     matches your CyVerse username and Navigate to the folder where your
     sequencing files are located.

     .. admonition:: Sample Data

        For sample sequence date in this guide select navigate to:
        Shared Data > cyverse_training > platform_guides > dna_subway >
        purple_line > montana_controls

        Select all 11 fastq files (they are compressed and will have the fastq.gz
        file extension). Then click "Add selected files".

  5. Click the "add data" link to add the sequencing data to your project.


  .. warning::
    **Known alpha-version bug**
    After adding data, the next stop (Demultiple reads) will still be blocked.
    Refesh DNA Subway in your browser to unblock this step.

**B. Demultiplex reads**

At this step, reads will be grouped according to the sample metadata provided,
this includes separating reads acording to their sequencing barcodes if this
was not done prior to running the Purple Line.

  1. Click **demux reads** to demultiplex your sample reads.

  2. When demultiplexing is complete, you will generate a file (.qzv) click
     this link to view a visualization and statistics on the sequence and
     metadata for this project.

**C. Check sequencing quality and Trim Reads**

It is important to only work with high quality data. This step will generate a
sequence quality histogram which can be used to determine parameter for trimming.

  1. Click the 'Demultiplex reads' stop, then click the 'demux reads' button.

  2. When the demultiplexing step is complete, Job status will be "done" and a
     link to a result ending in **.qzv** will appear. Click this link to view
     your results.

  3. Click the "Interactive Quality Plot" tab to view a histogram of sequence
     quality. Use this plot to determine a location to trim.

    .. tip::

        **Tips on trimming for sequence quality**

      On the Interactive Quality Plot you are shown an histogram, plotting the
      average quality (X axis)
      `Phred Score <https://en.wikipedia.org/wiki/Phred_quality_score>`_ vs. the
      position on the read (y axis) in base pairs for a **subsample** of reads.

      **Zooming to determine 3' trim location**

      Click and drag your mouse around a collection of base pair positions you
      wish to examine. Clicking on a given histogram bar will also generate a
      text report and metrics in the table below the chart. Using these metrics,
      you can choose a position to trim on the right side (e.g. 3' end of the
      sequence read). The 5' (left trim) is sepecific to your choice of primers
      and sequencing adaptors (e.g. the sum of the adaptor sequence you expect
      to be attached to the 5' end of the read). Poor quality metrics will
      generate a table colored in red, and those base positions will also be
      colored red in the histogram. Double-clicking will return the histogram to
      its original level of zoom.

      **Example plots**

      *Quality drops significantly at base 35* - In this case because [MORE INFO]

      |histogram_poor|

      *Improved quality sequence*

      |histogram_good|


    .. note::

      **QIIME2 Visualizations**

    One of the features of QIIME 2 are the variety of visualizations provided
    at several analysis steps. Although this guide will not cover every
    feature of every visualization, here are some important points to note.

    - **QIIME2 View**: DNA Subway uses the QIIME 2 View plugin to display
      visualizations. Like the standalone QIIME 2 software, you can navigate
      menus, and interact with several visualizations. Importantly, many files
      and visualizations can be directly download for your use outside of
      DNA Subway, including in report generation, or in your custom QIIME 2
      analyses. You can view downloaded .qza or .qzv files at
      `https://view.qiime2.org <https://view.qiime2.org>`_


  4. Click on the 'Trim reads' step. Click the 'run' button and then select
     values for "trimLeft" (the position starting from the left you wish to
     ) and "TruncLen" (This is the position where reads should be trimmed,
     truncating the 3' end of the read. Reads shorter than this length will
     be discarded).

     .. admonition:: Sample Data

        Based on the histogram for our sample, we recommend the following
        parameters:

        - **trimLeft: 17** (this is specific to primers and adaptors in this
          experiment)
        - **TruncLen: 120** (this is where low quality sequence begins, in this case
          because our sequence length is lower than the expected read length)

----

*DNA Subway Purple Line - Cluster Sequences*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

At this step, you can visualize summaries of the data. A feature table will
generate summary statistics, including how many sequencies are associated with
each sample.

  1. Click 'Feature table' and then the "Build feature table" link. When
     processed, you will get a link to a visualization file (.qzv). Open this
     file to examine your results. The QIIME 2 view window will also have a link
     to download a FASTA file of your sequences.

     .. tip::

       The feature table contains two columms output by DADA2. DADA2 (Divisive
       Amplicon Denoising Algorithm 2) determines what sequences are in the
       samples. DADA2 filters the sequences and identifies probable
       amplification or sequencing errors, filters out chimeric reads, and can
       pair forward and reverse reads to create the best representation of the
       sequences actually found in the samples and eliminating erroneous
       sequences.

       - **Feature ID**: A unique identifier [OTU? MORE INFO]
       - **Sequence**: A DNA Sequence associated with that identifier [MORE INFO]

       Clicking on any given sequence will initiate at BLAST search on the NCBI
       website. Click "View report" on the BLAST search that opens in a new
       web browser tab to obtain your results. Keep in mind that if your
       sequences are short (due to read length or trimming) many BLAST searches
       may not return significant results.

  2. Click on 'Phylogenetic diversity' and then click the "Build phylogenetic
     diversity". This will not generate a visualization, but the data will be
     passed on to the next steps.

----

*DNA Subway Purple Line - Calculate Alpha and Beta Diversity*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
At this stop, you will examine *Alpha Diversity* (the diversity of species/taxa
present within a single sample) and *Beta Diversity* (a comparison of
species/taxa diversity between two or more samples). Alpha diversity answers the
question - "how many species are in a sample?"; beta diversity answer the
question - "what are the differences in species between samples?".


**A. Calculate core matrix**

  1. Click on 'Core matrix' and then click the "run" link. Choose a sampling
     depth and classifier - see comments in the tip below. Finally, click '
     Submit job'.

       .. tip::

         **Choosing Core matrix parameters**

         *Sampling Depth*
         This parameter indicates how may random subsamples of counts will be
         taken from each sample to calculate diversity. This parameter has a
         large impact on your results so it is important to choose carefully.
         You can choose by examining the table generated at the
         **Demultiplex reads**
         step. Choose a number that is at least as high as the minimum
         Demultiplex sequence counts number (keeping in mind samples with counts
         lower than this number will be dropped). The computational costs/time
         goes up as sampling depth increases so keeping this number under 2000
         is recommended. [SEVERAL GUESSES HERE]

         *Classifier*
         Choose a classifier pertaining to your experiment type. For
         **Microbiome** choose **Grenegenes (16s rRNA)** classifier. For an
         **eDNA** experiment chosse **Custom 12s rRNA, take 3** or if you are
         specifically looking for marine fishes you may elect to choose the
         **Mitofish JO** classifier.

    .. admonition:: Sample DNA

      We recomend the following parameters for the **eDNAworked** dataset:

       - **Sampling Depth**: 1000
       - **Classifier**: Custom 12s rRNA, take 3

**B. Calculate Alpha diversity**

----



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
.. |histogram_poor| image:: ./img/dna_subway/histogram_poor.png
   :width: 400
   :height: 250
.. |histogram_good| image:: ./img/dna_subway/histogram_good.png
   :width: 400
   :height: 250
