---
layout: post
title: Image_processing
tags: 
  - python
  - work
  - A.I
categories: Python
---
# 이미지 처리
1. 두 개의 사진 파일을 불러온 뒤 흑백사진으로 바꿈
2. 두 개의 사진을 합치는 작업


```yaml
from PIL import Image
import matplotlib.pyplot as plt
import numpy as py # 행렬 계산 자료형 (Numerical Python)
import cv2 #영상처리 라이브러리
# PC에 있는 파일을 colab으로 업로드
from google.colab import files
uploaded = files.upload()


# 로컬에서 jobs.jpg라는 파일을 읽어오라.
img = plt.imread('jobs.jpg') #받아 들일 때 컬러였다.
print("최초의 컬러 이미지 모양", img.shape)
# 컬러 이미지 img를 흑백으로 변환 dst에 넣기
dst = cv2.cvtColor(img, cv2.COLOR_RGB2GRAY)
print("흑백이미지로 변환 후 모양", dst.shape)
plt.imshow(dst)

# dst 모양의 앞쪽 두개의 차원을 가져와서 width, height애 넣음
width, height = dst.shape[:2]
print("width=", width, "height=", height)

# 이미지를 원하는 크기로 줄여보기
resizeFactor = 0.5  # 스케일 factor
reWidth = int(width * resizeFactor) # 원래 차원을 절반으로 줄임
reHeight = int(height * resizeFactor) # 자연수 int casting
print("reWidth=", reWidth, "reHeight=", reHeight)

# resize를 조절하되, 비어있게 되는 부분은 보간(interpolation)해서 채워 넣어라.
dst = cv2.resize(dst, (reHeight, reWidth), interpolation = cv2.INTER_CUBIC)
print("resize shape", dst.shape)

plt.imshow(dst)

# image의 모든 요소들을 순회하여 인덱싱한다.
for i in range(reWidth):
    for j in range(reHeight):
        # 밝기 값이 120보다 작은 픽셀이 존재한다면 
        if dst[i][j] < 120:
            dst[i][j] = 0
            

plt.imshow(dst)

img2 = plt.imread("apple.jpg") #두 번째 이미지를 가져온다.
dst2 = cv2.cvtColor(img2, cv2.COLOR_RGB2GRAY)
print(dst2.shape)
dst2 = cv2.resize(dst2, (reHeight, reWidth), interpolation = cv2.INTER_CUBIC)

# image의 모든 요소들을 순회하여 인덱싱한다.
for i in range(reWidth):
    for j in range(reHeight):
        # 밝기 값이 120보다 작은 픽셀이 존재한다면 
        if dst2[i][j] > 200:
            dst2[i][j] = 0
        else: 
            dst2[i][j] = 255

plt.imshow(dst2)

# 두 개의 이미지 차원이 똑같은지 확인
print(dst.shape)
print(dst2.shape)

# 블렌딩 coefficient, 합칠때 가중치
alpha = 0.89 # 0에서 1사이의 값을 가지도록 지정

# 두 영상의 정보를 블렌딩 해보기
for i in range(reWidth):
    for j in range(reHeight):
        # jobs 영상에 가중치를 크게 주려면 alpha값을 크게 주면 됨
        dst[i][j] = alpha*dst[i][j] + (1-alpha)*dst2[i][j] # 가중평균 가능

plt.imshow(dst)
```
