# U-Net_Convolutional_Networks_for_Biomedical

## 1. Introduction

### 1.1 Introduction

U-Net은 생물학적인 영상에서 Segmetation을 위해 제안된 네트워크

### 1.2 Segmentation

<img src='https://user-images.githubusercontent.com/102225200/201017805-8b22114c-5393-479e-9d45-ce9e00669e9b.png' width=800>

<sub>이미지 출처 Fig.1. : https://light-tree.tistory.com/75  Fig.2. : https://www.jeremyjordan.me/semantic-segmentation/

CNN의 일반적인 사용은 Fig.1. 처럼 Classification이며 이미지에 대한 출력은 단일 클래스 여기서 더 나아가면 Localization이 추가되어 바운딩박스를 통해 위치확인 가능

하지만 생물학 이미지 처리에서 원하는 것은 Segmentation으로 Fig.2.처럼 픽셀마다 라벨(클래스)이 포함되어야 함

## 2. U-Net architecture

### 2.1 U-Net의 구성

<img src='https://user-images.githubusercontent.com/102225200/201018530-6183dc77-cf78-4d7f-8a67-7f39529df194.png' width=800>

U자형으로 생긴 구조로 크게 **Context(이미지의 문맥, 클래스 정보)** 정보를 위한 Contracting path, 정확한 **Localization(위치 정보)**을 위해 Expanding path로 구성

### 2.2 Contracting path

<img src='https://user-images.githubusercontent.com/102225200/201019013-18b13be8-7a35-41d8-b6c1-279c415339e2.png' width=800>

Convolution과 Relu 그리고 Maxpooling의 반복을 거치면서 Context 추출

### 2.3 Expanding path

<img src='https://user-images.githubusercontent.com/102225200/201019601-662e01ef-e3d3-4e29-934c-aba52549f339.png' width=800>

Convolution과 Relu 그리고 Up- Convolution 을 거치면서 원본 이미지와 비슷한 크기로 복원

### 2.4 U-Net 구조 특징

<img src='https://user-images.githubusercontent.com/102225200/201019862-7425d91f-6ca8-4721-a640-e10d6ad2640d.png' width=800>

분류모델과 다르게 Fully Connected Layer가 아닌 Fully Convolutional Network로 구성

따라서 Contracting path에서 나온 Context와 Expanding path에서 나온 Localization이 결합하여 성능이 우수


## 3. Overlap-tile strategy 


<img src='https://user-images.githubusercontent.com/102225200/201020137-a220b993-a192-4601-87ef-9c1c86e84537.png' width=800>

위 이미지처럼 노란색 영역의 Segementation을 위해 파란색 영역만큼의 입력 이미지의 비어있는 부분은 미러링을 하여 외삽하는데
이런 이유는 해상도가 큰 이미지에 대해 매끄럽게 분할이 가능하며 GPU 메모리에 의해 해상도가 제한되기 때문



## 4. Training

### 4.1 Objective function


<img src='https://user-images.githubusercontent.com/102225200/201020437-9ea28045-24a7-4b0e-b542-3e280c9a587c.png' width=800>


Segmentation은 픽셀단위로 라벨링을 해야 되기 때문에 픽셀 단위로 Softmax를 사용하고 
학습을 위해 Cross entorpy를 사용하는데 학습이 더 잘되도록 추가적으로 가중치 함수를 사용

### 4.2 Weight

<img src='https://user-images.githubusercontent.com/102225200/201022746-f5bbf6b6-e355-451f-a8d4-3a44abdfadc3.png' width=800>

가중치함수의 식을 통해 d1, d2 거리가 가까울수록 가중치가 증가 

위 그림을 가중치를 매핑한 이미지인데 이를 통해 가까운 셀의 경계는 가중치가 더 큰 것을 확인 할 수 있음

### 4.3 Data Agumentation


<img src='https://user-images.githubusercontent.com/102225200/201023445-a28166bb-dac6-4980-8622-ca638ab8e152.png' width=800>

데이터가 부족할 땐 이미지 증강이 필수적

여기선 왼쪽 이미지처럼 일반적으로 사용하는 데이터 증강 외에 오른쪽 이미지처럼 elastic deformation을 사용


## 5. Experiments

<img src='https://user-images.githubusercontent.com/102225200/201023717-e9bd8a16-91d2-458a-918b-436655ec6aef.png' width=800>

EM segmentation challenge에선 U-Net은 데이터 전처리나 사후처리없이 Warping Error 기준으로 좋은 성능

ISBI cell tracking challenge에서는 두번째 좋은 모델과의 성능이 차이가 많이 남


## 6. Conclusion

### 6.1 Conclusion

- U-Net은 생물의학 segmentation에서 매우 좋은 성능을 보여줌

- Elastic deformation 덕분에 라벨링 된 데이터가 많이 필요 없었으며 합리적인 tarin 시간

- U-Net architecture가 더 많은 작업에 쉽게 적용 될 것이라고 확신

### 6.2 Review

- 논문 제목처럼 Biomedical 분야에서는 많이 활용되고 있을 것 

- 다른 분야도 생각해보면 위성사진에서 밀집 되어있는 주택가에서 주택의 경계선, 얽혀 있는 도로를 구분 등 사용이 가능할 것으로 보여짐






