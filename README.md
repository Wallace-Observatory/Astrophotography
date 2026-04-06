# Astrophotography
_Basic AP tools, scripts and sample data to get you started._

We recommend starting with Siril, as it is free and works on PC, Mac and Linux OS's.  You can find it here: https://siril.org/
Current version as of April 2026 is _1.4.2_

### Note, you do not need to digest all of the following information to get a nice image.  Though, if you were wondering, "Why are going through all this trouble?", then see the two images below.



## Planning
At this point, you have probably planned your observation, submitted the request and waited patiently for the clouds to part so that the automated telescope images your target.  By submitting target coordinates in RA & DEC, you have effectively decided on the composition of the image.  
You may have just went with the default coordinates for the centroid of the target.  Hopefully you chose the J2000 Epoch for those coordinates, otherwise you may be sorely disappointed!  Maybe you made a slight adjustment to that centroid in an attempt to frame the object to your taste.  
It's also possible you made a thoughtful assessment of your subject and used the rule of thirds (https://www.dpreview.com/articles/7143352326/the-rule-of-thirds-a-simple-way-to-improve-your-images) or leading lines (https://digital-photography-school.com/how-to-use-leading-lines-for-better-compositions/).  
There's no "right" way, but unless you plan on using a single image for your final submission, we do not recommend _cropping_ your image(s) until after you are finished with **stacking** your images.

## The "Stacking" Algorithm: Harvesting Photons, Rejecting Chaos
_Why stack? What is stacking?_ 

Think of each 2-minute exposure as a "noisy" data sample. Your goal is to extract a weak, constant signal (a nebula) from a high-variance background (electronic noise and satellite interference).

### The Strategy:
We use Winsorized Sigma Clipping with Additive Scaling. In plain English: we equalize the baseline of every image, filter out the statistical "outliers," and then perform a weighted average.
- Normalization (The Baseline): Before comparing frames, we use -norm=addscale. This ensures that even if light pollution or transparency shifted during your session, every frame is adjusted to have the same "zero point" (background) and "gain" (signal intensity).
- The Winsorized Filter: With only 10 frames, a single satellite trail is a massive outlier. Winsorized Clipping "tames" these extremes by replacing them with neighboring values before calculating the standard deviation. This prevents a satellite from "poisoning" the mean.
- Weighted Averaging: We don't treat all data equally. By using -weight=wfwhm, the algorithm gives a higher "vote" to frames with the sharpest focus and best atmospheric stability.
**The Result:** You transform 10 grainy, artifact-filled snapshots into one high-fidelity, 32-bit master file ready for histogram stretching.

## Signal-to-Noise Ratio (SNR) vs. Number of Frames (N)
The graph below illustrates the Square Root Rule. Because noise is random (stochastic), it cancels out at a rate of $\sqrt{N}$. Note the "knee" in the curve: you get a massive boost early on, but eventually, you hit diminishing returns where you must quadruple your effort just to double your clarity.

<img width="857" height="567" alt="image" src="https://github.com/user-attachments/assets/a3d6e525-9093-4389-aec3-d99c82e546ef" />

## Install Siril
1. Download the calibration files (10 x bias and 10 x flats) and script from Dropbox: https://www.dropbox.com/scl/fo/zj7r6fl8tdwes736rpdof/ABrtz7S_3aVtvt2fSBU-Zn4?rlkey=70gqd1hir5gu6d45vugabollb&st=hgiurlml&dl=0
2. Unzip this directory structure to a temp folder on your harddrive.
3. Your instructor will provide a link to your target images.  Copy those .fits files into the 'lights' subfolder.
4. After you open Siril for the first time, it may give you a tour of the main buttons in the app (worth reading!).
## Set Home directory and Check Images
5. It is always a good idea to check your images before running scripts blindly.
6. <img width="725" height="520" alt="image" src="https://github.com/user-attachments/assets/97070c3f-87bb-4500-b90e-20d91bcff41b" />
    _You will notice the Home button (it looks like a house) at the top left.  Click that and you will direct Siril to the directory that contains all of your subfolders._
7. We need to point Siril to the custom script, 'WAO_OSC_Mercury.ssf'.  Click 'Scripts' button, select Script Editor and --> \File\Open, find the directories you just unzipped and then open the script.

## Run Script
6. Run the script from the Script Editor --> \Script\Run
7. If you keep an eye on the console window to the right, you will see all of the steps stream by.  If you have a fast computer, this 
   

