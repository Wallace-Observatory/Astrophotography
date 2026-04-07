# Astrophotography
_Basic AP tools, scripts and sample data to get you started._

We recommend starting with Siril, as it is free and works on PC, Mac and Linux OS's.  You can find it here: https://siril.org/

Current version as of April 2026 is _1.4.2_

### Note, you do not _need_ to digest all of the following information to get a nice image.  Though, if you were wondering, "Why are going through all this trouble?", then see the two images below. Otherwise, skip to Install Siril below.  



## Planning
At this point, you have probably planned your observation, submitted the request and waited patiently for the clouds to part so that the automated telescope images your target.  By submitting target coordinates in RA & DEC, you have effectively decided on the composition of the image.  You may have submitted the listed coordinates for the centroid of the targets or perhaps you offset those coordinates in order to frame the object.  (Hopefully you chose the J2000 Epoch for those coordinates, otherwise you may be sorely disappointed!) It's also possible you made a thoughtful assessment of your subject and used the _rule of thirds_: (https://www.dpreview.com/articles/7143352326/the-rule-of-thirds-a-simple-way-to-improve-your-images) or _leading lines_: (https://digital-photography-school.com/how-to-use-leading-lines-for-better-compositions/).  
There's no "right" way, but unless you plan on using a single image for your final submission, we do not recommend _cropping_ your image(s) until after you are finished with **stacking** your images.

## The "Stacking" Algorithm: Harvesting Photons, Rejecting Noise
_Why stack? What is stacking?_ 

Think of each 2-minute exposure as a "noisy" data sample. Your goal is to extract a weak, constant signal (a nebula) from a high-variance background (various noise sources and satellite interference).
## Signal-to-Noise Ratio (SNR) vs. Number of Frames (N)
The graph below illustrates the Square Root Rule. Because noise is random (stochastic), it cancels out at a rate of $\sqrt{N}$. Note the "knee" in the curve: you get a massive boost early on, but eventually, you hit diminishing returns where you must quadruple your effort just to double your clarity.

<img width="857" height="567" alt="image" src="https://github.com/user-attachments/assets/a3d6e525-9093-4389-aec3-d99c82e546ef" />

### Strategy in a Script
There is no "right way" to processing astrophotos.  In our scripts, we will use Winsorized Sigma Clipping with Additive Scaling. In plain English: we equalize the baseline of every image, filter out the statistical "outliers," and then perform a weighted average.
- Normalization : Before comparing frames, we use -norm=addscale. This ensures that even if light pollution or transparency shifted during your session, every frame is adjusted to have the same "zero point" (background) and signal intensity.
- The Winsorized Filter: With only 10 frames, a single satellite trail is a massive outlier. Winsorized Clipping "tames" these extremes by replacing them with neighboring values before calculating the standard deviation. This reduces a satellite from skewing the mean. These rejection values can be adjusted, default is sigma=3.
- Weighted Averaging: We don't treat all data equally. By using -weight=wfwhm, the algorithm gives a higher "vote" to frames with the sharpest focus and best atmospheric stability.
- 
**The Result:** You transform 10 grainy, artifact-filled snapshots into one high-fidelity, 32-bit .fit file ready for histogram stretching.



## Install Siril
1. Download the calibration files (10 x bias and 10 x flats) and script from Dropbox: [https://www.dropbox.com/scl/fo/zj7r6fl8tdwes736rpdof/ABrtz7S_3aVtvt2fSBU-Zn4?rlkey=70gqd1hir5gu6d45vugabollb&st=hgiurlml&dl=0](https://www.dropbox.com/scl/fo/zj7r6fl8tdwes736rpdof/ABrtz7S_3aVtvt2fSBU-Zn4?rlkey=yr5oqav0zu7od5rbvt193d2d5&st=rb7ltgi4&dl=0)
2. Unzip this directory structure to a temp folder on your harddrive.
3. Your instructor will provide a DropBox link to your data folder.  Copy those .fits files into the 'lights' subfolder.
4. After you open Siril for the first time, it may give you a tour of the main buttons in the app (worth reading!).
## Set Home directory and Check Images
5. It is always a good idea to check your images before running scripts blindly.
6. You will notice the Home button (it looks like a house) at the top left.  Click Home and redirect Siril to the parent directory that contains all of your subfolders.  After that you can use the Open button to open any of your images for inspection.  Play around with the non-destructive stretch buttons at bottom right.  By default it is set to _linear_, now try _Autostretch_ to give you a preview of stretching without corrupting your raw fits files.  
   <img width="725" height="520" alt="image" src="https://github.com/user-attachments/assets/97070c3f-87bb-4500-b90e-20d91bcff41b" />


## Run Script
7. We need to point Siril to the custom script, 'WAO_OSC_Mercury.ssf'.  Click 'Scripts' button, select Script Editor and --> \File\Open, find the directories you just unzipped and then open the script.
8. Run the script from the Script Editor --> \Script\Run
9. If you keep an eye on the console window to the right, you will see all of the steps stream by.  If you have a fast computer, this might only take a couple minutes.

## Post Processing
10. When the script is done, you should have ~4 images in your 'Home' directory.  'result.fit' is just the stacked image.  'result_post.fit' has all of the corrections (remove green noise, gradient and sky subtraction, calibrated photometric correction).  'result_autostretch.fit' applies a brute force histogram stretch, similar to the button we showed you earlier.  Finally, there's a TIF file that is ready for final edits if you like what you see from the autostretch version.

<img width="630" height="420" alt="image" src="https://github.com/user-attachments/assets/16defd29-e67c-41ed-aefd-0dd92649f4ca" />

### result.fit

<img width="630" height="420" alt="image" src="https://github.com/user-attachments/assets/71fd0766-3b65-4e28-bba9-14a0ff62c365" />

### result_post.fit

<img width="630" height="420" alt="image" src="https://github.com/user-attachments/assets/0a2a60e6-9ce2-4ab5-bbc3-44cdee863c13" />

### result_autostretch.fit



11. There's a couple more basic steps we recommend doing in Siril.  Let's assume we like result_post.fit the best and use that as our starting point for final edits.
12. Cropping - In Siril, draw a box with the mouse for the portion of the image you want to keep.  Right click, Crop.
13. RGB Align - Often the color looks shifted or misaligned.  Rigth click, RGB Align, 2-Pass.
14. The rest is up to you.  You can play around with various Image Processing and Tools in Siril.  Try various stretches in Siril or you could leave those edits to your favorite image editing app.
15. Finally, we need to save the image.  In the top bar, to the right, look for what looks like a download symbol.  If you're done, save the image as a .png and send to your instructor.  If you want to try printing out your image or further edits in PS or GIMP, you will want to save as a 32-bit TIF file as that is uncompressed and intact.

## Troubleshooting
16. Sometimes we put things in the wrong place or the script gets interupted and ERRORS happen.  Try deleting the fit/tif files in the directory and the cache & process subfolders.  Make sure you are in the right directory, that you copied the lights to the lights folder and run script again.
17. Feel free to contact Tim, he's happy to help!

