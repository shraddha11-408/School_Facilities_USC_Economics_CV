# School_Facilities_USC_Economics_CV
Assignment for the RA position available for the School Facilities project by Prof. David Schönholzer

## Overview
This code snippet develops a reproducible workflow for analyzing 25 school campuses using NAIP aerial imagery. School coordinates were validated, imagery was acquired, and campus attributes were extracted using a Qwen vision model and manual review. The final outputs include structured measurements, attribute-level confidence scores, validation metrics, and a historical comparison of one attribute at two different imagery dates for some schools in the dataset.

## Workflow
<img width="655" height="706" alt="image" src="https://github.com/user-attachments/assets/4f59f09d-5b1c-4c89-bc24-e6aac9309652" />

## Tools Used
1. Google Colab
2. OpenStreetMap
3. ESRI GIS
4. NAIP (National Agriculture Imagery Program)
5. Microsoft Planetary Computer and STAC API
6. Groq for Qwen
7. Google Maps and official school websites for reference validation
8. Google Earth Pro (Historical Imagery feature)

## Setup and Dependencies (HOW TO REPRODUCE)
The notebooks in this repo were run on Google Colab. Here is how you can reproduce it:-

1. imagery_acquisition.ipynb
   Upload school.csv to the notebook's sample_data folder before running it. Here, we also use three data and imagery services : OpenStreetMap, ESRI and NAIP. The first two are to verify the co-ordinates of the schools. Once corrected (if necessary) and verified, we use NAIP to download the aerial images. The metadata is stored in your drive as naip_master_metadata.csv

2. Qwen_predictions.ipynb
   We first need to create a token on Groq to enable us to use the Qwen model. This API key must be added as a secret on Colab as GROQ_API_KEY. Ensure you run this on the free T4 GPU. Bear in mind that in the free tier of Groq, your daily limit might run out so this must done in 3-4 passes.

3. Run measurements_csv_generation.ipynb

4. Run Validation.ipynb

Required Python packages are installed by the setup cells inside each notebook. But you can find these in the requirements.txt file too. Never upload or commit the Groq API key to GitHub.


## Results

Overall, two methodologies were used:-
1. Hand-labeling (attribute values and confidence scores)
2. Qwen model predictions (attribute values)

The measurements.csv contains the values of the attributes after hand-labeling from the aerial images obtained in NAIP.

To validate the methods, I picked a set of 8 random schools and established ground truth values for them. This was done manually using Google Maps and official school websites. I then calculated the error rate for both my methods for each field against the ground truth.

### Error-Rate Calculation

For each attribute, the exact-match error rate was calculated as:

**Error rate (%) = (Number of predictions different from the ground-truth values / Total number of ground-truth predictions) × 100**

For example, if 3 out of 8 predictions differ from the values:

**Error rate = (3 / 8) × 100 = 37.5%**

Confidence scores were reported separately and were not included in the error-rate calculations.

Historical Google Earth imagery was reviewed for the eight selected schools to compare sports-field counts between observations from 2006 and 2016. Both observation dates and counts were recorded, including cases with no detected change. The results are provided in `ground_truth-measurements.csv`.

<img width="806" height="190" alt="image" src="https://github.com/user-attachments/assets/304f3ec5-22af-4d4b-b436-b98efaccbd92" />


### Observations

The hand-labeled measurements had slightly lower error rates than the Qwen predictions when compared with the verified reference values. Human reviewers could use the wider campus context to distinguish visually similar features, while Qwen relied primarily on the visible image details.

Running tracks and full-sized sports fields had relatively low error rates because their large size, regular shapes, and distinctive markings made them easy to identify from aerial imagery. Hard courts were generally detected, but their counts sometimes varied because individual courts were small, closely grouped, faded, or low-contrast. In several cases, Qwen failed to identify hard courts that occupied only a small portion of the image.

Fencing presence, type, and perimeter could not be determined reliably from the aerial imagery. Fence lines are narrow relative to the image resolution and may also be obscured by vegetation, buildings, or shadows. Solar-panel presence could sometimes be identified, but estimating the exact panel area was unreliable because of image resolution and unclear roof boundaries.

Almost all schools had no visible portable classrooms.



