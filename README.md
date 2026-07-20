# PTB-XL ECG Signal Exploration

## Project Overview

This project explores real electrocardiogram (ECG) waveform data from the PTB-XL dataset.

Unlike my UCI Heart Disease project, which uses tabular clinical features for machine learning classification, this project focuses on time-series ECG waveform data.

The main goals are to understand the structure of 12-lead ECG data, visualize ECG signals, explore R-peak detection, and estimate heart rate from ECG recordings.

This project focuses on ECG data exploration and basic signal processing. No machine learning or deep learning classification model was trained in this notebook.

---

## Dataset

This project uses the PTB-XL ECG dataset available through PhysioNet.

PTB-XL contains clinical 12-lead ECG recordings.

An ECG records the electrical activity of the heart over time.

Each recording contains 12 leads:

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

Each lead observes the heart's electrical activity from a different direction.

---

## Loading ECG Data

ECG records were accessed using the WFDB Python package.

An example record was loaded using:

`wfdb.rdrecord()`

The ECG signal can be accessed through:

`record.p_signal`

Additional information, such as the sampling rate and lead names, can be accessed through the record metadata.

---

## Understanding ECG Data Structure

The ECG recording used in this exploration contains multiple sampling points for each of the 12 leads.

A sampling point represents one measurement of the ECG signal at a specific moment in time.

A sampling point is not the same as one heartbeat.

For example, if an ECG is sampled at 100 Hz:

- 100 samples = 1 second
- 1000 samples = 10 seconds

The recording duration can be calculated as:

`Number of samples / Sampling rate`

Understanding the sampling rate is important because it allows sample positions to be converted into actual time in seconds.

---

## 12-Lead ECG Visualization

<img width="4496" height="5306" alt="all_12_leads (1)" src="https://github.com/user-attachments/assets/a02a5160-55b7-481d-9a2e-3f80683c90f7" />

All 12 ECG leads were plotted to compare their waveform patterns.

Different leads show different amplitudes and morphologies because they observe the same electrical activity of the heart from different anatomical directions.

The 12-lead visualization demonstrates that ECG data is multidimensional and that different leads provide complementary information.

---

## Selected Lead Comparison

<img width="4170" height="2366" alt="compare_leadII_V1_V6 (1)" src="https://github.com/user-attachments/assets/3052fbfe-32a2-47a1-b578-f966bd68ba15" />

Selected leads, including Lead II, V1, and V6, were plotted separately for easier comparison.

The waveform shapes differ among the leads even though they represent the same cardiac cycles.

These differences occur because each lead observes the electrical activity of the heart from a different direction.

---

## ECG Waveform Components

A typical ECG waveform contains several important components:

### P Wave

The P wave represents atrial depolarization.

### QRS Complex

The QRS complex represents ventricular depolarization.

The R wave is usually one of the most prominent peaks in the ECG signal.

### T Wave

The T wave represents ventricular repolarization.

Understanding these waveform components is important for interpreting ECG signals and developing future ECG analysis methods.

---

## R-Peak Detection

<img width="3038" height="1176" alt="leadII_rpeaks (1)" src="https://github.com/user-attachments/assets/aff3304c-a322-4974-9a11-7835cb40eb73" />

R peaks were detected from Lead II using `scipy.signal.find_peaks()`.

Two main parameters were used:

- `height` — sets a minimum amplitude for detected peaks
- `distance` — sets a minimum distance between consecutive detected peaks

The detected peaks were plotted on top of the ECG waveform to visually check whether they aligned with the expected R waves.

The selected parameters worked for the example recording, but they may not work equally well for all patients, ECG leads, or noisy signals.

---

## Heart Rate Estimation

Heart rate was estimated using two methods.

### Method 1: Peak Count

The number of detected R peaks was counted over the recording duration.

Heart rate was estimated using:

`Heart Rate = Number of R Peaks × 60 / Recording Duration`

### Method 2: Median R-R Interval

The R-R interval represents the time between consecutive R peaks.

The intervals were calculated using the detected peak positions and the ECG sampling rate.

Heart rate was then estimated using:

`Heart Rate = 60 / Median R-R Interval`

The two methods produced similar heart-rate estimates for the example ECG recording.

The median R-R interval provides another way to estimate heart rate without relying only on the total number of peaks counted during the recording.

---

## Visualization

The project includes visualizations of:

- All 12 ECG leads
- Individual ECG waveforms
- Selected lead comparisons
- R-peak detection
- ECG signals displayed using time in seconds

The figures help connect the raw numerical ECG data with recognizable cardiac waveform patterns.

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

These patterns can potentially support classification tasks involving cardiac abnormalities.

However, this notebook does not train an AI classification model.

Instead, it provides a foundation for future work involving ECG preprocessing, feature extraction, classification, and deep learning.

---

## Key Learning Points

Through this project, I learned how to:

- Access real ECG data using WFDB
- Understand the structure of 12-lead ECG recordings
- Understand the difference between sampling points and heartbeats
- Use sampling rate to convert samples into time
- Visualize all 12 ECG leads
- Compare ECG signals across different leads
- Identify basic P, QRS, and T waveform components
- Detect R peaks using signal-processing methods
- Calculate R-R intervals
- Estimate heart rate using two methods
- Understand how ECG waveform data differs from tabular clinical data

---

## Limitations

This project is an introductory ECG exploration rather than a clinical diagnostic system.

Important limitations include:

- Only a limited number of ECG examples were explored.
- R-peak detection parameters were manually selected.
- Noise and abnormal ECG morphology may affect peak detection.
- Heart-rate estimation depends on accurate R-peak detection.
- No ECG classification model was trained or validated.
- The results should not be interpreted as clinical diagnoses.

A more advanced project could apply preprocessing to a larger number of PTB-XL records and train a machine learning or deep learning model for ECG classification.

---

## Conclusion

This project provided an introduction to working with real ECG waveform data.

I learned how ECG signals are stored as time-series data, how the 12 leads provide different views of cardiac electrical activity, and how R peaks can be used to estimate heart rate.

Compared with my UCI Heart Disease project, this project helped me understand an important difference between two types of medical data: tabular clinical data and physiological waveform data.

This ECG exploration provides a foundation for future work in ECG preprocessing, feature extraction, and AI-based ECG classification.
