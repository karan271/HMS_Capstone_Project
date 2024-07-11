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

### Project organization

We must build a classifier that is able to assign for a given EEG time series the correct class based on the detection and recognition of one of 6 patterns:
* seizure (SZ)
* generalized periodic discharges (GPD)
* lateralized periodic discharges (LPD)
* lateralized rhythmic delta activity (LRDA)
* generalized rhythmic delta activity (GRDA)
* “other” for all signals that do not fit inside above categories.

So after the EDA it’s time to recognize the patterns and start fitting the models to get the overview oof the error and evaluation metrics.

### Recognition

To understand the pattern, we analyzed a sample of eeg with the eeg_id and recorded its shape by reading the .parquet sample files.

After that we analyzed the different designations allotted to different electrical activity as per the eeg graph. We got to know,
We have 20 signals, 19 of which correspond to EEG measurements, which are voltage fluctuations recorded by electrodes put on the scalp, and one that corresponds to EKG, which is the patient's ECG.
EEG measurements reveal the electrical activity of the brain. The various signal designations (Fp1, F3,...) relate to conventional places where electrodes will be placed. The sites are often connected with a certain region of the brain. The letters represent certain lobes, such as pre-frontal (Fp), frontal (F), temporal (T), parietal (P), occipital (O), and central (C). Even numbers in electrode names imply that they are located on the right side of the head. An odd number indicates that it is positioned on the left side. Z (for zero) indicates that the electrode is in the middle plane and acts as a reference point.

After that we focused on eeg electrodes, starting from the FP1 type.
Further, zooming in according to the seconds and in the end, plotting the whole set of electrodes. But it was not very insightful, signals are very noisy. A possible cause of the noise in our signal is the existence of an interfering signal that add to the true signal. Things can be intepreted with the use of EKG or ECG records as with the denser graph we can filter down different amplitudes. Further processing can be interpretable and meaningful one. So, we will come back to more graphical interpretations with EKG.

Considering "seizure_vote" as the target variable to identify the seizures, let's fir the models. We split the data into training and validation sets and used 80% of the data for training and 20% for validation.

Then, we generated error measures for each model, including Mean Absolute Error (MAE), Mean Squared Error (MSE), Root Mean Squared Error (RMSE), and R-squared.
Linear regression has high error metrics and a low R² value, indicating poor data fit. The assumption of a linear connection may not be valid for this dataset.

The Decision Tree Model improves significantly with reduced error metrics and a higher R² value, indicating a better capture of data structure.

The Random Forest Model has the best performance, with the lowest error metrics and the highest R² value. The ensemble strategy, which involves averaging many trees, prevents overfitting while improving prediction performance.

According to the assessment metrics, the Random Forest Regression model fits the data best. However, feature engineering, hyperparameter adjustment, and cross-validation would be considered for further development.

The following data modelling was not an accurate for prediction as the target variable was wrong in this case. So, we target the expert consensus now and do a bit more EDA.

## Train the model

The model training starts with the scaling and splitting the data in training and validation sets.
80% and 20% respectively.
The random forest classification to the clustered data with initializing and fitting the data. Further, making predictions based on accuracy, classification, and confusion matrix.


With which we get 99.95% accuracy and perfect precision and recall. But that’s too good to be true so, we adjust the model by checking the selected features of the test csv file.

Re-training of data gave us good accuracy but the ultimate result for seizure detection based on expert consensus gave the wrong output with duplications and repetitions.

## Prediction on Test Data with Selected Features and concatenating the files with ids

We matched and referenced the train and test files with their respective .parquet files to get a bigger picture of all the target variables,

'seizure_vote', 'lpd_vote', 'gpd_vote', 'lrda_vote', 'grda_vote', 'other_vote'

Now, these target variables have been identified for prediction. Performing the same model selection and prediction based on the merged files. Adding f1 score for better understanding.

Here, we do get a good accuracy, averaging up to 83% for all the seizure votes and making it better to get a prediction overview.


After aligning the train and test files with their respective.parquet files, we were able to generate a comprehensive dataset that included all target variables:'seizure_vote', 'lpd_vote', 'gpd_vote', 'lrda_vote', 'grda_vote', and 'other_vote'. 

Using this consolidated dataset, we performed model selection and prediction, adding the F1 score to provide a more accurate evaluation of model performance. The results demonstrate promising accuracy, with an average of 83% over all seizure votes, showing strong predictive capability. 

This method improves the overall knowledge and effectiveness of the prediction model for the stated target variables through Machine Learning.

But, can we try another model or maybe method like neural networks with time series and image classification?

Yes, we can.





## Wavenet Neural Network - A deeplearning time series model

WaveNet is a feedforward neural network also known as a deep convolutional neural network (CNN). In WaveNet, the CNN accepts a raw signal as input and generates an output one sample at a time.

Grouping the data by 'eeg_id', aggregating and calculating the minimum, maximum, and sum of relevant columns while normalizing target values. It also extracts the 'patient_id' and 'expert_consensus' for each 'eeg_id' and prints the resulting dataframe and its shape.

It would be easy to convert all .parquet files in a folder to one .npy file for both the ids to get easy for us to iterate over the different frequencies.

Combining all the .parquet files in one .npy file for both the ids.

We create a custom PyTorch dataset class EEGDataset for loading and processing EEG and spectrogram data, applying transformations, and preparing it for a WaveNet neural network model.

The data is standardized, log-transformed, and randomly augmented. The class handles data in both training and testing modes, providing inputs and target labels for the neural network.

Now, we create an instance of the EEGDataset, load it into a PyTorch DataLoader with a batch size of 32, and shuffle the data.


Then iterating through the DataLoader, plotting the spectrogram images for each sample in a grid layout. The images are normalized, and each subplot is labeled with the corresponding target values and EEG ID.

## In Conclusion

The notebook effectively sets up the data pipeline for the WaveNet neural network model, including data loading, augmentation, and visualization.

By iterating through batches, it displays spectrograms and their corresponding target labels, providing a clear understanding of the dataset's structure and preparation steps. This visualization aids in verifying the correctness of the data preprocessing pipeline and ensures the model receives accurately processed inputs.

The approach ensures a robust preparation of EEG data for subsequent training and evaluation phases.
