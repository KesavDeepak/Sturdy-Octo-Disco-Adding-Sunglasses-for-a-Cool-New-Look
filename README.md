# Sturdy-Octo-Disco-Adding-Sunglasses-for-a-Cool-New-Look

Sturdy Octo Disco is a fun project that adds sunglasses to photos using image processing.

Welcome to Sturdy Octo Disco, a fun and creative project designed to overlay sunglasses on individual passport photos! This repository demonstrates how to use image processing techniques to create a playful transformation, making ordinary photos look extraordinary. Whether you're a beginner exploring computer vision or just looking for a quirky project to try, this is for you!

## Features:
- Detects the face in an image.
- Places a stylish sunglass overlay perfectly on the face.
- Works seamlessly with individual passport-size photos.
- Customizable for different sunglasses styles or photo types.

## Technologies Used:
- Python
- OpenCV for image processing
- Numpy for array manipulations

## How to Use:
1. Clone this repository.
2. Add your passport-sized photo to the `images` folder.
3. Run the script to see your "cool" transformation!

## Applications:
- Learning basic image processing techniques.
- Adding flair to your photos for fun.
- Practicing computer vision workflows.

Feel free to fork, contribute, or customize this project for your creative needs!

## Program:
```
import cv2
import numpy as np
import matplotlib.pyplot as plt

face = cv2.imread("/content/23002011.png")
sunglass = cv2.imread("/content/download.jpg", cv2.IMREAD_UNCHANGED)

# Haar Cascade for eyes
eye_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + "haarcascade_eye.xml")

gray = cv2.cvtColor(face, cv2.COLOR_BGR2GRAY)
eyes = eye_cascade.detectMultiScale(gray, 1.3, 5)

if len(eyes) >= 2:
    eyes = sorted(eyes, key=lambda x: x[0])[:2]
    (x1, y1, w1, h1) = eyes[0]
    (x2, y2, w2, h2) = eyes[1]

    ex = min(x1, x2)
    ey = min(y1, y2)
    ew = max(x1 + w1, x2 + w2) - ex
    eh = max(h1, h2)

    sg_w = int(ew * 2.0)  
    sg_h = int(eh * 1.2)  
    sunglass_resized = cv2.resize(sunglass, (sg_w, sg_h))

    x = ex - int((sg_w - ew) / 2)
    y = ey - int((sg_h - eh) / 2)

    sg_h, sg_w = sunglass_resized.shape[:2]

    if y+sg_h <= face.shape[0] and x+sg_w <= face.shape[1] and x >= 0 and y >= 0:
        roi = face[y:y+sg_h, x:x+sg_w]

        if sunglass_resized.shape[2] == 4:  
            b, g, r, a = cv2.split(sunglass_resized)
            sunglass_rgb = cv2.merge((b, g, r))
            mask = a / 255.0
        else:
            sunglass_rgb = sunglass_resized
            mask = np.ones((sg_h, sg_w), dtype=float)

        for c in range(3):
            roi[:,:,c] = roi[:,:,c] * (1 - mask) + sunglass_rgb[:,:,c] * mask

        face[y:y+sg_h, x:x+sg_w] = roi


plt.figure(figsize=(10,10))
plt.imshow(cv2.cvtColor(face, cv2.COLOR_BGR2RGB))
plt.axis("off")
plt.title("Face with Sunglasses (Aligned to Eyes)")
plt.show()

```
## Output:
<img width="311" height="435" alt="image" src="https://github.com/user-attachments/assets/dd024c23-ff04-41c0-9ace-069f026036a1" />

<img width="425" height="435" alt="image" src="https://github.com/user-attachments/assets/cbec7553-524b-4c16-b721-149035205941" />

<img width="1597" height="749" alt="image" src="https://github.com/user-attachments/assets/067a8c3d-5555-407d-8f97-f2879c3b3db4" />

<img width="533" height="812" alt="image" src="https://github.com/user-attachments/assets/ee71e96b-77b8-42fa-b35a-998d92d01ee7" />
