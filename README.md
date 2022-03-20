
# Introduction

A simple method to identify motion in a live camera feed or a normal video and generate signals



## Programming Language

Python
OpenCV

## Usage
 
 With a camera module and a microcontroller this implementation can be used as a security mechanism. Genarated signal can be used to sound a alarm.




## Deployment

Install OpenCV

1.In Pycharm


2.Windows

3.Ubuntu



```bash
  npm run deploy
```


## Explanation

For testing purpose we weill use a uploaded video and detect motion. As a output, detected motions will be drawn and the signal will also drawn on the video.

So for a live video feed from a camera, above output video which marks motion can be used to display or store for security purposes. Signal can be used to sound a real time alarm.

1. Import libraries

``` 
from imutils.video import VideoStream
import datetime
import imutils
import time
import cv2
import argparse
``` 
2. Open the video and grab first frame from it

```
vs = cv2.VideoCapture("venv/images/vid.mp4")
ref, frame = vs.read()
time.sleep(2.0)
frame_width = int(vs.get(3))
frame_height = int(vs.get(4))
```

3. Create a video writer object to save the output

```
out = cv2.VideoWriter('venv/images/vid1.avi', cv2.VideoWriter_fourcc('M', 'J', 'P', 'G'), 10, (frame_width, frame_height))
```

4. While loop will run until there are no frames to grab from the 
input video

break point
```
if frame is None:
		break
```
5. First the frames are GRAY scaled and Blured that helps to identify differences

```
gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
gray = cv2.GaussianBlur(gray, (21, 21), 0)
```
6. Get the **absolute Difference** between two frames and then get the threshold

```
frameDelta = cv2.absdiff(gray0, gray)
thresh = cv2.threshold(frameDelta, 25, 255, cv2.THRESH_BINARY)[1]
```

7. Dilate the thresholded image to fill in holes, then find contours on thresholded image

```
cnts = cv2.findContours(thresh.copy(), cv2.RETR_EXTERNAL,
cv2.CHAIN_APPROX_SIMPLE)
cnts = imutils.grab_contours(cnts)

```
8. If there are contours that means there is a difference between two frames. Contours
 are drawn to identify them. change the value of mached are to change sensitivity 

 ```
 for c in cnts:
		# if the contour is too small, ignore it
		if cv2.contourArea(c) <5000:
			continue
		# compute the bounding box for the contour, draw it on the frame,
		# and update the text
		(x, y, w, h) = cv2.boundingRect(c)
		cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)
		text = "Moving"

 ```
 

 <p align="center"> <img src=/images/22.png></p>
 <p align="center"> Movemet is identified in threshold</p>
 <p align="center"> <img src=/images/11.png></p>
 <p align="center"> Movemet is identified and drawn </p>

 9. Status and date/time text are displayed on frames

 ```
 cv2.putText(frame, "Status: {}".format(text), (10, 20),
				cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 255, 255), 2)
	cv2.putText(frame, datetime.datetime.now().strftime("%A %d %B %Y %I:%M:%S%p"),
				(10, frame.shape[0] - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.35, (0, 0, 255), 1)

 ```

 ## For live videos from a camera

1. input video from device

```
vs = VideoStream(src=0).start()
vs = cv2.VideoCapture(0)
time.sleep(2.0)

```
## Further implementations

1. Use above text = "Stable"/"Unstable" signals to triger a alarm

<p align="center"> <img src=/images/33.jpg></p>
<p align="center"> Status is shown on the output</p>
<p align="center"> <img src=/images/44.png></p>
<p align="center"> Status Signal on console</p>



2. To check the stablility for some period of time -

count if text signal is not changed 


## Optimizations

To change the sensitivity (To identify smaller or larger motions ) of the motion detection change the value of the area ( 490 ) 

```
if cv2.contourArea(c) <490:
```			
