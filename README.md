RedEdge Data Process
  =======

 --------------------------------------------------


This is the python implementation of the RedEdge Bulk processing & semi realtime mornitoring code.
This repository based on MicaSense RedEdge and Altum Image Processing(https://github.com/micasense/imageprocessing) algorithm


## 1. Installation

### 1.1 Dependencies

```
  SOFTWARE          REQUIRED FOR   URL
  python            all            http://www.python.org/
  numpy             all            https://pypi.python.org/pypi/numpy/
  scipy             all            https://pypi.python.org/pypi/scipy
  Scikit-learn
  opencv-contrib-python 
  matplotlib
  pysolar
  exiftool + pyexiftool
  zbar + pyzbar
  gdal              GSW            https://pypi.python.org/pypi/GDAL
  micasense-imageprocessing
```


**NOTE**: support for python2 is being discontinued. It is highly recommended to use python3.

**NOTE**: The supported operating system is Windows10 

If you are using anaconda, see the script install-anaconda-deps.sh which help
installing all required dependencies in a dedicated environment (recommended method).


## 2. 데이터 

```
####################################################################################################
RedEdge-MX 밴드 처리를 위한 파일 셋팅
1. Irradiance(복사조도) = 바닥에 놓인 패널촬영, 단위 : (W/m2/nm)
2. (option) Sky radiance = 태양에서부터 135도(방위) 각도로 찍은 하늘, 단위 : (W/m2/sr/nm) 
3. Total radiance = 레드엣지 촬영 폴더 선택 , 단위 :  (w/m2/sr/nm) 
####################################################################################################
```

```
####################################################################################################
# 파일 저장 구상
data folder
      └ 0000SET RedEdge(전원을 켤때마다 새로운폴더 생성)
          └ 000 (250장 촬영마다 새로운 폴더 생성)
          └ Matching 
          └ Result
              ├ TIFF  (5개 밴드영상이 저장된 폴더, 밴드순서 : Blur, Green, Red, RedEdge,NIR)         
                      (Using "RE_pre_pro" Algorith code)
              └ IMG   (RGB 영상과, CIR영상, NDVI 영상이 저장되는 폴더)
                      (Using "RE_pre_pro" Algorith code)
                          ----- 아래는 추후 작업 필요 -----
###################################################################################################
              └ BRI   (Band Registered image; 모폴로지 알고리즘을 이용해 정렬된 영상)
                      (Using "RE_post_pro" Algorith code)
###################################################################################################
```


```
###################################################################################################
영상처리 순서도 기술
1.지상에서 패널촬영후 영상에서 Irradiance Value를 추출하여 뒤에 영상에 적용
***실시간으로 처리되는 영상을 가져오고, 처리된 영상과 처리가 되지 않은 영상 분할하기 필요***
(Option)하늘을 촬영한 영상을 통하여 Sky radiance 값을 가지고 오는 코드
2.촬영 셋트 내의 다른 영상 가지고 오기
3.다른 기하를 가지고 촬영된 영상을 정렬하여 크롭된 영상 제작(5개 밴드 매칭; 기준밴드 : NIR(4번))
4.RedEdge의 기본 알고리즘을 이용하여 영상전처리 DN -> Radiance -> Reflectance or Rrs
5.Reflectance 또는 Rrs 영상을 저장하기(tiff file format)
-------------
6.밴드 정합을 통해서 RGB, CIR, NDVI 영상 만들기
7.각 영상 저장 및 실시간 플롯 시스템 구축
###################################################################################################
```

## 3. 사용법
```
process.bat 에서 아나콘다 환경 변경 call conda activate #####

1. panel 영상 선택(미선택시 radiance 영상 제작)
2. 영상 1개 선택(영상1개를 선택시, 해당폴더의 모든 영상에 대한 작업이 시작됨
3. 반사도 영상 또는 Radiance 영상이 TIFF로 제작됨
4. RGB, CIR, NDVI 영상이 각 폴더에 저장됨
5. opencv창을 통해 처리되는 영상이 지속적으로 디스플레이됨
```

## 4. 기타

```
1.영상정합기법을 사용하지 않고 카메라 제공 Matrix만 사용중
2.Rrs를 제작하기 위한 Sky radiance부분이 빠져잇음
3.물에서의 밴드정렬을 위한 모폴로지 부분이 빠져있음
4.밴드 10개(MX + Blue)에 대한 코드 수정 필요
```

