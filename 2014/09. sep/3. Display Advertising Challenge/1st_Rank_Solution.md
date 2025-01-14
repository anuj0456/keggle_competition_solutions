# Winning approach by 3 Idiots

Team Members - [@Yuchin Juan](https://github.com/ycjuan), mandora, mengmengyong

REF: https://www.kaggle.com/competitions/criteo-display-ad-challenge/discussion/10555

## LIBFFM

This most important model used in this solution is called ``field-aware
factorization machines.'' If you want to use this model, please download LIBFFM
from:

    http://www.csie.ntu.edu.tw/~r01922136/libffm

## System Requirement

- 64-bit Unix-like operating system (We tested our code on Ubuntu 13.10)

- Python3

- g++ (with C++11 and OpenMP support)

- at least 40GB memory and 100GB disk space

## Get The Dataset

Criteo released the data set after the end of the competition. However, the
format of the released data set is different from the one used in the
competition; the format used in the comptition is in csv format, while the
released one is in text format.

Our scripts require csv format. If you already have the dataset used in the
competition, please skip to the next section. Otherwise, we provide a script to
convert the files in text format to csv format. Please follow these steps:

1. Download the data set from Criteo.

   http://labs.criteo.com/2014/02/kaggle-display-advertising-challenge-dataset/

2. Decompress and make sure the files are correct.

   $ md5sum dac.tar.gz
   df9b1b3766d9ff91d5ca3eb3d23bed27 dac.tar.gz

   $ tar -xzf dac.tar.gz

   $ md5sum train.txt test.txt
   4dcfe6c4b7783585d4ae3c714994f26a train.txt
   94ccf2787a67fd3d6e78a62129af0ed9 test.txt

3. Use ``txt2csv.py'' to convert training and test data to csv format.

   $ converters/txt2csv.py tr train.txt train.csv

   $ converters/txt2csv.py te test.txt test_without_label.csv

4. Add dummy labels to test.csv to make the format be consistent with train.csv.

   $ utils/add_dummy_label.py test_without_label.csv test.csv

5. Calculate the checksum of produced file.

   $ md5sum train.csv test.csv
   ebf87fe3daa7d729e5c9302947050f41 train.csv
   cd6ac893856ab5aa904ec1ad5ef9218b test.csv

Now you can follow the steps in ``Step-by-step'' section to run our experiments.

## Step-by-step

First, compile executables:

    $ make

This section is divided into two parts. The first part introduces how to run
our code with a toy example. After you finish this part, please follow the
second part to run with the whole dataset.

## Tiny Example

1. Create two symbolic links.

   $ ln -s train.tiny.csv tr.csv
   $ ln -s test.tiny.csv te.csv

2. Generate the prediction file "submission.csv".

   $ run.py

3. Generate the checksum for submission.csv.

   $ md5sum submission.csv
   19a913d1577c3d419f62e38c34305341 submission.csv

## The Whole Dataset

1. Copy or link the dataset generated in the section ``Get The Dataset'' to
   this directory. Their names should be "tr.csv" and "te.csv", respectively.

2. Generate the prediction file "submission.csv". (You may want to use more
   threads. Please see "Miscellaneous 1".)

   $ run.py

## Miscellaneous

1. By default we use only one thread, so it may take a long time to train the
   model. If you have multi-core CPUs, you may want to set NR_THREAD in run.py
   to use more cores.

2. Our algorithms is non-deterministic when multiple threads are used. That
   is, the results can be slightly different when you run the script two or
   more times. In our experience, the variances of logloss generally do not
   exceed 0.0001.

3. This script generates a prediction with around 0.44510 / 0.44500 on
   public / private leaderboards, which are slightly worse than what we had
   during the competition. The difference is due to some minor changes.

   If you want to reproduce our best results, please do:

   $ git checkout v1.0

   For detailed instructions, please follow README in that version.

Solution Code: [Code](https://github.com/ycjuan/kaggle-2014-criteo)
