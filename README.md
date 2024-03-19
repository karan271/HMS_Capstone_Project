# HMS_Capstone_Project

> Harmful Brain Activity Classification and Prediction



## Project Overview

The purpose of this capstone project is to identify and categorize seizures and other forms of harmful brain activities by developing model trained on electroencephalography (EEG) signals recorded from critically ill hospital patients.

### Problem Area

- Currently, EEG monitoring is based primarily on manual analysis by specialist neurologists. While invaluable, this time-consuming operation is a significant bottleneck. Manual evaluation of EEG recordings is not only time-consuming, but also costly, prone to fatigue-related mistakes, and plagued by reliability concerns among diverse reviewers, even when those reviewers are specialists.
- Advancements in this field may enable doctors and brain researchers to identify seizures or other forms of brain injury, allowing for faster and more precise treatment.

### Proposed Vision

- The work in automating EEG analysis will help doctors and brain researchers detect seizures and other types of brain activity that can cause brain damage.

The algorithms to be developed may also help researchers who are working to develop drugs to treat and prevent seizures.
- My work will significantly increase electroencephalography pattern categorization accuracy, resulting in dramatic improvements for neurocritical care, epilepsy, sleep disorders, medication discovery, and in some cases brain tumor.
- Further Modeling and analysis will be based of Pattern recognition, Correlation, Linear and Logistic Regression, and more.

### Dataset

There are two data files, one Train.csv and Test.csv. The train data consists of a lot of overlapping samples. The test data does not so, it will be trained to group the required data as there are not duplicates.

- eeg_id: A unique identifier for the entire EEG recording. Those identifies also the parquet files of the eeg in the train_eegs folder. Note that as each row denominates a particular subsample of an EEG, there can be several rows with same eeg_id.
- eeg_sub_id: An ID for the specific 50 second long subsample this row's labels apply to. eeg_sub_id are assigned in chronological order for each eeg_id beginning with 0. The subsample is shifted from the beginning of the eeg_id record by eeg_label_offset_seconds. Note that even if subsample is 50 seconds long, the annotation is done by looking at the central 10 seconds.
- eeg_label_offset_seconds: The time between the beginning of the consolidated EEG and this subsample. Time shift.
- spectrogram_id: A unique identifier for an entire spectrogram. A spectrogram can regroup several EEGs records. Note that spectrogram_id are name of spectrogram files in folder train_spectrograms. For more information on what is a spectrogram.
- spectrogram_sub_id: An ID for the specific 10-minute subsample this row's labels apply to. Same concept as for eeg_sub_id.
- spectogram_label_offset_seconds: The time between the beginning of the consolidated spectrogram and this subsample. Simple time shift.
- label_id: An ID for this set of labels. Unique for each row.
- patient_id: An ID for the patient who donated the data. An eeg_id or spectrogram_id is associated with a single patient_id but a single patient can be associated with several eegs and spectrograms.
- expert_consensus: The consensus annotator label. Provided for convenience only. Give the final decision for classifying of the 10 seconds central window of a given subsample in one of the 6 available category.
- [seizure/lpd/gpd/lrda/grda/other]_vote: The count of annotator votes for a given brain activity class. The full names of the activity classes are as follows: lpd: lateralized periodic discharges, gpd: generalized periodic discharges, lrd: lateralized rhythmic delta activity, and grda: generalized rhythmic delta activity. Note that total number of annotators can vary for each row, even within the same eeg. Size of cohort of experts range from 1 to 28.


