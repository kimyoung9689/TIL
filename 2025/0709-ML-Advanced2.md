### F1 score

정밀도와 재현율의 조화평균

데이터가 한쪽으로 치우쳐져 있을 때 

정확도만으로는 모델 성능을 제대로 평가하기 어려울 때 유용하게 쓰이는 지표


### TPR과 FPR

● TPR (True Positive Rate): 진짜 중에서 진짜로 예측한 비율
● FPR (False Positive Rate): 가짜 중에서 진짜로 예측한 비율

### ROC 곡선 ( ROC curve )

모델의 성능을 여러 기준점에서 한눈에 보여주는 그래프

 FPR과 TPR을 각각 x,y축으로 가지는 그래프

좌상단에 치우칠수록 높은 성능  

= TPR은 높고 FPR은 낮을수록 모델의 성능이 더 좋다는 의미


### AUC (Area Under the ROC Curve)

ROC 곡선 아래의 면적
이 값은 모델의 성능을 **하나의 수치로 요약해서 보여주는 기준**

1에 가까울수록 그래프가 좌상단에 근접하므로 좋은 성능


### IoU (Intersection over Union)

**객체 탐지** 모델의 성능을 평가하는 방법 중 하나

모델이 예측한 바운딩 박스(초록색)와 실제 정답 바운딩 박스(빨간색)가 

**얼마나 잘 겹치는지**를 수치로 나타냄 1에 가까울수록 가장 높은 점수



threshold를 기준으로

IoU ≥ threshold일 경우 예측이 맞았다고 구분
IoU < threshold일 경우 예측이 틀렸다고 구분
## PR Curve(Precision-Recall Curve)

어떤 물체를 잘 찾아냈는지(Recall)

얼마나 정확하게 찾아냈는지(Precision) 그래프로 보여주는 것

'confidence score'라는 기준점을 바꿔가면서 recall과 precision 값이 어떻게 변하는지 보여준다.


### AP (Average Precision)

PR Curve(이전에 설명한 그 그래프) 아래쪽 면적을 계산한 값

이 면적이 넓을수록 모델 성능이 좋다는 뜻

PR Curve는 들쑥날쑥할 수 있어서, 그래프 아래 면적을 계산해 하나의 숫자로 모델 성능을 평가하는 방법


MAP (Mean Average Precision)

모델이 여러 종류의 물체(고양이, 개 등)를 찾아낸다면, 

각 물체마다 AP 값이 다르다.
mAP는 이렇게 **모든 종류의 물체에 대한 AP 값을 평균 낸 것**

모델이 모든 종류의 물체를 얼마나 잘 찾아내는지 종합적으로 평가할 때 쓰는 지표


## Segmentation task의 성능 평가에 사용되는 평가 지표

### PA(Pixel Accuracy)

**전체 픽셀 중에서 네가 예측한 픽셀이 얼마나 정답이랑 똑같냐**를 비율로 나타낸 것

예)그림에 픽셀이 100개 있고 그중에서 예측이 90개 픽셀에서 맞았다면 PA는 90%


### MPA(Mean Pixel Accuracy)

각 클래스를 얼마나 정확하게 맞췄는지의 평균

특정 클래스에만 잘하는 게 아니라 모든 클래스에 대해 골고루 잘하는지를 보여주는 지표

PA는 아까 설명했듯이 '전체 픽셀 중에 정답 픽셀 비율'

**여러 종류의 물체(클래스)를 구분하는 상황일 때** 배경, 자동차, 사람 이렇게 세 가지를 구분한다고 하면

- **배경에 대한 PA** (배경 픽셀 중 맞춘 비율)
- **자동차에 대한 PA** (자동차 픽셀 중 맞춘 비율)
- **사람에 대한 PA** (사람 픽셀 중 맞춘 비율)

이렇게 각 클래스마다 PA를 따로 계산할 수 있다.

**MPA는 이렇게 각 클래스별로 계산한 PA들을 전부 더해서 클래스 개수로 나눈 평균값**

### IoU


1. **네가 예측한 영역** (마스크)
2. **실제 정답 영역** (마스크)

이 두 마스크가 **얼마나 겹치는지**를 숫자로 나타낸 것

계산 방법

- **겹치는 부분 (Intersection):** 네가 예측한 마스크랑 실제 정답 마스크가 **겹치는 픽셀의 개수**
- **합쳐진 부분 (Union):** 네가 예측한 마스크랑 실제 정답 마스크를 **모두 합쳤을 때의 픽셀 개수** (겹치는 부분은 한 번만 세고)

IoU = (겹치는 부분 픽셀 수) / (합쳐진 부분 픽셀 수)

이 값이 1에 가까울수록 예측이 정답이랑 거의 같다는 뜻

0에 가까울수록 많이 다르다는 뜻

IoU가 높을수록 모델이 픽셀 단위로 정확하게 영역을 잘 잡았다고 볼 수 있다.

### Dice Coefficient

 IoU와 유사하지만, 두 영역(합집합, 교집합)의 조화평균을 사용하여 계산하는 방법


왜 IoU 말고 Dice Coefficient를 쓸까?

가장 중요한 이유 중 하나는 **데이터가 불균형(imbalance)할 때 더 정확한 지표**가 될 수 있다는 점
예를 들어, 배경 영역은 엄청 넓고 우리가 찾아야 할 객체 영역은 아주 작을 때 같은 경우에 Dice Coefficient가 더 유용할 수 있다.

두 영역의 조화 평균을 사용해서 계산하는데, 이게 불균형한 데이터에서 특정 클래스에 치우치지 않고 성능을 더 잘 반영한다고 알려져 있다.

특히 영역 크기가 많이 다를 때 IoU보다 좀 더 신뢰할 수 있는 결과를 줄 수 있다.

