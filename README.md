# Video_Analytics

### Overview & real-life application

This project introduces a vision-based sensor that was implemented on multiple long-span bridges in NYC using live traffic camera 
feed. Methods for noise filters, camera motion compensation and target detection are introduced to 
optimize it for practical use and to evaluate a bridge's global and localized behavior in real-time. 
To see an example of how the code was applied when the Verrazano-Narrows was shaking earlier this year to estimate how much it was moving just from a youtube video, check the paper in the repo!

https://user-images.githubusercontent.com/79415699/122861697-dc4c4480-d2ed-11eb-8b71-d122e1749453.mp4


## How it works:
The code defines a function that uses Computer Vision trackers in Python's OpenCV library to extract coordinates of the centroid of a user-defined bounding box. 

In each frame i, the bounding box is defined as:

(𝑥𝑏𝑖; 𝑦𝑏𝑖; 𝑑𝑥𝑏𝑖; 𝑑𝑦𝑏𝑖)

The box’s centroid coordinates in each frame are:

(𝑥𝑐𝑖; 𝑦𝑐𝑖) = (𝑥𝑏𝑖 + 𝑑𝑥𝑏𝑖/2; 𝑦𝑏𝑖 +𝑑𝑦𝑏𝑖/2)

The code produces a movemeent trace by assigning coordinates in each frame to a list across n frames:

[𝑋]𝑇𝑎𝑟𝑔𝑒𝑡 = [𝑥𝑐𝑖, 𝑥𝑐𝑖+1, … , 𝑥𝑐𝑛]

[𝑌]𝑇𝑎𝑟𝑔𝑒𝑡 = [𝑦𝑐𝑖, 𝑦𝑐𝑖+1, … , 𝑦𝑐𝑛

# User-Guide
(video taken for demo purposes)

## 1. Start by selecting a target and running the video 
![image](https://user-images.githubusercontent.com/79415699/122855608-78bd1980-d2e3-11eb-9e72-ccb9ff3d87a9.png)

https://user-images.githubusercontent.com/79415699/122856969-bcb11e00-d2e5-11eb-970d-b73f4cb369fe.mp4


## 2. Scaling

To get a better sense of the movement measured, we can manually define a 
bounding box on the first frame of the video to determine the size of the object in pixels f and convert the axis to inches or feet. 

In this example, f = 50 pixels the real dimension is 3" (measured), so the scaling factor is: 𝑆𝐹 = 𝑑/𝑓 in/pixel.

![image](https://user-images.githubusercontent.com/79415699/122858001-85437100-d2e7-11eb-988f-8be32031254a.png)


![image](https://user-images.githubusercontent.com/79415699/122860678-2502fe00-d2ec-11eb-8879-879d1152292d.png)

## 3. Compensating for Camera Movement/Shake

As the video is recorded, the camera might not be 100% stable, which would introduce noise to the signal. To remove camera shake, repeat step 1 but choose a real-life stationary target like the book sitting on the shelf for example. 

![image](https://user-images.githubusercontent.com/79415699/122860797-5ed40480-d2ec-11eb-9c9f-b040da1218bb.png)

We get the target's real displacement by subtracting the two displacements:

[𝑋]𝐶𝑜𝑚𝑝𝑒𝑛𝑠𝑎𝑡𝑒𝑑 = [𝑋]𝑇𝑎𝑟𝑔𝑒𝑡 − [𝑋]𝐹𝑖𝑥𝑒𝑑
[𝑌]𝐶𝑜𝑚𝑝𝑒𝑛𝑠𝑎𝑡𝑒𝑑 = [𝑌]𝑇𝑎𝑟𝑔𝑒𝑡 − [𝑌]𝐹𝑖𝑥𝑒𝑑

![image](https://user-images.githubusercontent.com/79415699/122860869-7ad7a600-d2ec-11eb-8364-1ea05919b205.png)


## 4. Signal Processing and filtering

To clean higher unwanted frequencies (noise), a low-pass filter is applied: 

![image](https://user-images.githubusercontent.com/79415699/122860984-afe3f880-d2ec-11eb-88f1-e167f6d744fe.png)





