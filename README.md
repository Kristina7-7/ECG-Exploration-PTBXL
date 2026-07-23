# PTB-XL ECG Signal Exploration

## Project Overview

This project explores real electrocardiogram (ECG) waveform data from the PTB-XL dataset.

Unlike my UCI Heart Disease project, which uses tabular clinical features for machine learning classification, this project focuses on time-series ECG waveform data.

The project began with exploring the structure of a single 12-lead ECG recording, including ECG visualization, waveform components, R-peak detection, and heart-rate estimation.

The analysis was then expanded to multiple ECG records to test whether the same R-peak detection method works consistently across different ECG signals.

The main goals of this project are to:

- Understand the structure of 12-lead ECG data
- Visualize and compare ECG signals
- Understand basic ECG waveform components
- Detect R peaks using basic signal processing
- Estimate heart rate from ECG recordings
- Compare ECG examples with different diagnostic characteristics
- Test the limitations of fixed R-peak detection parameters
- Compare fixed and adaptive threshold methods for R-peak detection

This project focuses on ECG data exploration and basic signal processing. No machine learning or deep learning classification model was trained.

---

## Dataset

This project uses the PTB-XL ECG dataset available through PhysioNet.

PTB-XL contains clinical 12-lead ECG recordings together with metadata and diagnostic information.

Each ECG recording contains 12 leads:

- I
- II
- III
- aVR
- aVL
- aVF
- V1
- V2
- V3
- V4
- V5
- V6

Each lead observes the electrical activity of the heart from a different direction.

The dataset also contains metadata such as ECG IDs, file locations, and SCP codes that describe ECG findings and diagnostic information.

---

## Loading ECG Data

ECG records were accessed using the WFDB Python package.

An ECG record can be loaded using:

`wfdb.rdrecord()`

The waveform data can be accessed through:

`record.p_signal`

Other information can also be obtained from the record, including:

- Sampling rate
- Lead names
- Signal dimensions

The PTB-XL metadata was also explored to connect ECG waveform files with their ECG IDs and SCP codes.

---

## Understanding ECG Data Structure

The ECG recordings used in this project contain multiple sampling points for each of the 12 leads.

A sampling point represents one measurement of the ECG signal at a specific moment in time.

A sampling point is not the same as one heartbeat.

For example, for the 100 Hz ECG recordings used in this exploration:

- 100 samples = 1 second
- 1000 samples = 10 seconds

The recording duration can be calculated as:

`Recording Duration = Number of Samples / Sampling Rate`

Understanding the sampling rate is important because it allows sample positions to be converted into actual time in seconds.

---

## 12-Lead ECG Visualization

<img width="4496" height="5306" alt="all_12_leads (1)" src="https://github.com/user-attachments/assets/a02a5160-55b7-481d-9a2e-3f80683c90f7" />

All 12 ECG leads were plotted to compare their waveform patterns.

Different leads show different amplitudes and morphologies because they observe the same electrical activity of the heart from different anatomical directions.

The 12-lead visualization demonstrates that ECG data is multidimensional and that different leads provide complementary information about cardiac electrical activity.

---

## Selected Lead Comparison

<img width="4170" height="2366" alt="compare_leadII_V1_V6 (1)" src="https://github.com/user-attachments/assets/3052fbfe-32a2-47a1-b578-f966bd68ba15" />

Selected leads, including Lead II, V1, and V6, were plotted separately for easier comparison.

The waveform shapes differ among the leads even though they represent the same cardiac activity.

These differences occur because each lead observes the electrical activity of the heart from a different direction.

This comparison helped demonstrate why ECG analysis cannot treat all leads as identical signals.

---

## ECG Waveform Components

A typical ECG waveform contains several important components.

### P Wave

The P wave represents atrial depolarization.

### QRS Complex

The QRS complex represents ventricular depolarization.

The R wave is often a prominent part of the QRS complex and can be used to help identify individual cardiac cycles.

### T Wave

The T wave represents ventricular repolarization.

Understanding these basic waveform components is important for interpreting ECG signals and developing future ECG analysis methods.

---

## Initial R-Peak Detection

<img width="3038" height="1176" alt="leadII_rpeaks (1)" src="https://github.com/user-attachments/assets/aff3304c-a322-4974-9a11-7835cb40eb73" />

R-peak detection was first explored using Lead II and the `scipy.signal.find_peaks()` function.

Two main parameters were used:

- `height` — sets a minimum amplitude required for a peak
- `distance` — sets a minimum distance between consecutive detected peaks

The detected peaks were plotted on top of the ECG waveform to visually examine whether they aligned with prominent ECG peaks.

The method appeared to work reasonably well for the initial example. However, testing only one ECG cannot show whether the same parameters will work for other patients or abnormal ECG signals.

This led to the next part of the project: testing the same method across multiple ECG records.

---

## Heart Rate Estimation

Heart rate was estimated using two methods.

### Method 1: Peak Count

The number of detected peaks was counted over the recording duration.

Heart rate was estimated using:

`Heart Rate = Number of Detected Peaks × 60 / Recording Duration`

This method estimates the average heart rate based on the total number of detected peaks.

### Method 2: Median R-R Interval

The R-R interval represents the time between consecutive detected R peaks.

The intervals were calculated using the detected peak positions and the ECG sampling rate.

Heart rate was estimated using:

`Heart Rate = 60 / Median R-R Interval`

Using the median R-R interval reduces the influence of individual unusually long or short intervals compared with relying on one interval.

The two methods produced similar results for the initial ECG example, but later testing showed that they can differ when peak detection is inconsistent.

---

## Testing the Method Across Multiple ECG Records

To test whether the same R-peak detection method works across different ECG signals, the analysis was expanded to five PTB-XL ECG records.

Examples were selected using the PTB-XL metadata and SCP codes.

The selected records included:

- ECG 1 — normal ECG example
- ECG 3 — normal ECG example
- ECG 8 — MI-related example
- ECG 22 — ST/T change-related example
- ECG 17 — atrial flutter/atrial fibrillation-related example

All five records contained:

- 1000 sampling points
- 12 ECG leads
- 100 Hz sampling rate
- Approximately 10 seconds of ECG data

The same peak-detection approach was applied to Lead II for all five records.

This allowed the method to be tested under different ECG waveform characteristics instead of evaluating only one example.

---

## R-Peak and Heart Rate Comparison

The detected peaks and heart-rate estimates were compared across the five ECG records.

| ECG ID | Detected Peaks | Peak Count HR | Median R-R HR |
|---|---:|---:|---:|
| 1 | 11 | 66.0 bpm | 64.2 bpm |
| 3 | 11 | 66.0 bpm | 63.8 bpm |
| 8 | 9 | 54.0 bpm | 73.2 bpm |
| 22 | 10 | 60.0 bpm | 77.9 bpm |
| 17 | 12 | 72.0 bpm | 70.6 bpm |

ECG 1, ECG 3, and ECG 17 showed relatively similar heart-rate estimates between the two calculation methods.

However, ECG 8 and ECG 22 showed much larger differences.

This indicated that comparing numerical heart-rate estimates alone was not enough. The detected peaks also needed to be visually inspected on the original ECG waveforms.

---

## R-Peak Detection Across Different ECGs

### Normal ECG Example — ECG 1

<img width="3038" height="1176" alt="ecg_1_rpeak_detection" src="https://github.com/user-attachments/assets/c4edd8fe-ea8e-4ed7-b180-12291219bad8" />

For ECG 1, the detected peaks were relatively consistent and appeared on prominent positive peaks.

The regular spacing of the detected peaks also produced similar heart-rate estimates between the peak-count and median R-R methods.

---

### Failure Case — ECG 8

<img width="3038" height="1176" alt="ecg_8_rpeak_detection" src="https://github.com/user-attachments/assets/76b18f83-aa5d-4cb6-85de-f04e7ad5f46f" />


ECG 8 showed one of the clearest limitations of the fixed peak-detection method.

There was a long interval where possible QRS complexes were not detected by the fixed positive height threshold.

This produced a large difference between the two estimated heart rates:

- Peak-count estimate: 54.0 bpm
- Median R-R estimate: 73.2 bpm

This suggests that changes in waveform amplitude, morphology, or baseline can cause a fixed threshold to miss possible cardiac peaks.

---

### Complex Abnormal ECG Example — ECG 17

<img width="3038" height="1176" alt="ecg_17_rpeak_detection" src="https://github.com/user-attachments/assets/d1be582c-0ba5-423d-b3f0-005e4a14a0fc" />

ECG 17 showed a more complex and rapidly changing waveform.

Although the two calculated heart-rate values were relatively similar, the waveform demonstrates that agreement between two heart-rate calculations does not automatically prove that every detected peak is a true R peak.

Both heart-rate calculations depend on the same detected peak positions.

Therefore, visual inspection and more robust ECG processing methods are important when evaluating peak detection.

---

## Main Observation

The same fixed peak-detection parameters did not perform equally well across all ECG records.

The method worked relatively well for some ECG examples, particularly the normal ECG records, but detection became less consistent for some abnormal or more complex waveforms.

The results demonstrate an important limitation of simple ECG signal-processing methods:

**Parameters that work for one ECG may not generalize to every patient or cardiac condition.**

ECG amplitude, morphology, baseline variation, rhythm, and noise can all affect peak detection.

---

## Fixed vs Adaptive Threshold Experiment

A simple adaptive threshold method was tested to determine whether using a record-specific threshold could improve R-peak detection.

The fixed method used the same threshold for every ECG:

`height = 0.2`

The adaptive threshold was calculated separately for each ECG using:

`Adaptive Threshold = Mean of Signal + 0.5 × Standard Deviation of Signal`

The minimum peak distance was kept the same so that the main change between the two methods was the height threshold.

Both methods were compared using:

- Number of detected peaks
- Peak-count heart rate
- Median R-R interval heart rate
- Difference between the two heart-rate estimates
- Visual inspection of detected peak locations

---

## Fixed vs Adaptive Results

The effect of adaptive thresholding was different across the five ECG records.

| ECG ID | Fixed HR Difference | Adaptive HR Difference | Observation |
|---|---:|---:|---|
| 1 | 1.8 bpm | 1.8 bpm | Little or no change |
| 3 | 2.2 bpm | 2.2 bpm | Little or no change |
| 8 | 19.2 bpm | 19.2 bpm | Large disagreement remained |
| 22 | 17.9 bpm | 0.4 bpm | Large improvement in consistency |
| 17 | 1.4 bpm | 3.5 bpm | Adaptive result became less consistent |

For ECG 1 and ECG 3, the two methods produced similar results.

For ECG 8, a large difference between the two heart-rate estimates remained even after using the adaptive threshold.

ECG 22 showed the largest improvement. The difference between the two heart-rate estimates decreased from 17.9 bpm with the fixed threshold to 0.4 bpm with the adaptive threshold.

For ECG 17, the difference increased from 1.4 bpm to 3.5 bpm. Visual inspection also showed an additional smaller peak detected by the adaptive method that may not represent a true R peak.

Overall, the adaptive threshold improved detection consistency for some ECGs, especially ECG 22, but did not consistently improve every recording.

---

## What I Learned from the Threshold Comparison

This experiment showed that an adaptive threshold is not automatically better than a fixed threshold.

Lowering or changing the threshold may help detect peaks that were missed by a fixed threshold, but it may also detect additional smaller peaks.

ECG signals vary between records, so a simple threshold based only on the mean and standard deviation may not be enough to accurately detect R peaks in every ECG.

The results suggest that R-peak detection should consider more than only signal amplitude.

---

## Visualization

The project includes visualizations of:

- All 12 ECG leads
- Individual ECG waveforms
- Selected Lead II, V1, and V6 comparisons
- Initial R-peak detection
- R-peak detection across multiple ECG records
- Adaptive-threshold peak detection
- ECG signals displayed using time in seconds

These visualizations help connect raw numerical ECG data with recognizable waveform patterns and make it easier to identify limitations in automated signal-processing methods.

---

## Connection to AI and Machine Learning

ECG waveforms can potentially be used as input for machine learning and deep learning models.

Models may learn patterns related to:

- P-wave morphology
- QRS morphology
- T-wave morphology
- Heart rhythm
- Timing relationships
- Differences across ECG leads

However, this exploration also demonstrates an important step that comes before model development: understanding the data and preprocessing methods.

If ECG preprocessing or peak detection is inaccurate, the features provided to a machine learning model may also be inaccurate.

This notebook does not train an AI classification model.

Instead, it provides a foundation for future work involving ECG preprocessing, feature extraction, classification, and deep learning.

---

## Key Learning Points

Through this project, I learned how to:

- Access real ECG waveform data using WFDB
- Explore PTB-XL metadata and SCP codes
- Connect ECG metadata with waveform records
- Understand the structure of 12-lead ECG recordings
- Understand the difference between sampling points and heartbeats
- Use sampling rate to convert samples into time
- Visualize all 12 ECG leads
- Compare ECG signals across different leads
- Identify basic P, QRS, and T waveform components
- Detect peaks using basic signal-processing methods
- Calculate R-R intervals
- Estimate heart rate using peak count and median R-R intervals
- Compare the same analysis method across multiple ECG records
- Visually evaluate whether detected peaks are reasonable
- Recognize the limitations of fixed peak-detection parameters
- Understand how ECG waveform data differs from tabular clinical data
- Compare fixed and adaptive thresholds

---

## Limitations

This project is an introductory ECG exploration and should not be interpreted as a clinical diagnostic system.

Important limitations include:

- Only a small number of ECG records were examined in detail.
- Peak-detection parameters were manually selected.
- The same fixed parameters may not work for all ECG morphologies.
- A simple positive height threshold may miss QRS complexes with different amplitudes or orientations.
- Noise and baseline variation may affect peak detection.
- Heart-rate estimates depend on the accuracy of the detected peaks.
- Detected peaks from the algorithm were not independently confirmed as true R peaks using expert annotations.
- Diagnostic labels were used mainly to select different ECG examples rather than to make clinical predictions.
- No machine learning or deep learning classification model was trained or validated.
- The results should not be interpreted as clinical diagnoses.

Future work could use more advanced preprocessing, adaptive thresholds, dedicated QRS detection algorithms, and larger numbers of ECG records.

---

## Future Work

Possible next steps include:

1. Apply ECG filtering and baseline correction before peak detection.
2. Compare fixed thresholds with adaptive R-peak detection methods.
3. Test the method on a larger number of PTB-XL records.
4. Compare performance across different diagnostic groups.
5. Explore automated ECG feature extraction.
6. Use ECG waveform data as input for machine learning or deep learning classification models.

These steps would extend the project from basic ECG exploration toward a more complete AI-based ECG analysis workflow.

---

## Conclusion

This project provided an introduction to working with real ECG waveform data and showed how the analysis developed from exploring one ECG to comparing signal-processing methods across multiple records.

I learned how ECG signals are stored as time-series data, how the 12 leads provide different views of cardiac electrical activity, and how detected R peaks can be used to estimate heart rate.

The fixed-versus-adaptive threshold experiment showed that the hypothesis was partially supported. The adaptive threshold produced more consistent heart-rate estimates for ECG 22, but it did not improve all ECG records and introduced a questionable additional detection in ECG 17.

Therefore, a simple adaptive threshold is not universally better than a fixed threshold. Different ECG signals may require more robust signal-processing methods for reliable R-peak detection.

Compared with my UCI Heart Disease project, this project also helped me understand an important difference between two types of medical data: tabular clinical data and physiological waveform data.

This ECG exploration provides a foundation for future work in ECG preprocessing, feature extraction, and AI-based ECG classification.
