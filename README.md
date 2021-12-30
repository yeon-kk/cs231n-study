# cs231n-lecture study

cifar-10

L1 distance, L2 distance 
test image - training image (단순 픽셀위치에서의 절댓값 차이)  L1의 경우 test할 때, test set 수에 비례해서 선형적으로 시간이 증가  (<->CNN은 test시 동일)  
k-NN : k-Nearest Neighbor. k개중 가장 가까운 이미지를 다수결로.( k가 있을 때 더 부드러운 결과). 즉 k개의 이미지를 선정(배 사진 + 오분류된 말 ..)해서 다수결로 정해진 것을 정답이라 말함  
{L1, L2.. 왜?, k-NN,NN 왜?}

train이미지를 다시 검증하면?  
NN :  정확도는 100%. 자기 자신과의 비교
k-NN : 경우에 따라 다름. 다수결이기 때문에 나머지가 다른 클래스로 예측한다면 바뀌게 됨

즉, distance(픽셀 차) & k value  
hyper parameter : 문제에 따라 다름. 실험한 것 중 best를 채택 ( 그렇다면 하이퍼파라미터를 튜닝하기 위해 어떤 지표를 사용할 것인가? : validation data)  
training data 수가 적은 경우, k-fold(cross  validation)  
{{왜 }}

Image Captioning 이미지 캡셔닝 : 이미지를 문장으로(text) 정확하게 표현(묘사) CNN(classifier) + RNN(문장구성)

parameter(x: 이미지, W:가중치)  
Linear classifier : f(x,W) = Wx -> 10*1 1*3072 = 10*3072  : 행렬 연산으로 인해 해당 곱셈이 성립  
가중치 x 이미지 + 편향 : 이 순서로 행렬곱을 해줘야 하는데  
x는 이미지  
가중치에 해당하는 행렬은 ( 이미지 열 개수, 클래스 개수 ) 를 맞춰주어야 한다.  
== 가중치를 곱한 이미지에 편향을 더한 것. *뿌옇게 보이는 영상을 획득  

색상도 영향을 미침. 색상이 달라지면 잘 못 판별할 가능성이 큼. grayscale의 경우 색상없이 texture 등으로 판별하게 되는데 정확도 떨어질 가능성 큼  

이미지를 score로 만들었고, score을 loss 로 만드는 과정이 필요  
W(클래스 수,픽셀 수) * x(픽셀 수,1) + bias(클래스 수,1) = (클래스 수,1)  

- SVM loss  
- Softmax loss  

SVM loss는 정답 클래스와 그 외의 클래스의 차에 마진을 구하는 것  
Softmax loss는 -log(해당 클래스의 확률) 한 것

softmax와 softmax loss 는 다르다.  
softmax로는 해당 클래스에 대한 확률을 구하는 것  
softmax loss로는 -log(해당 클래스 확률)  
여기서 log값을 취하는 이유는 log가 더 빠르게 수렴하기 때문이며, 음수를 곱하는 이유는 loss는 분류기 성능이 얼마나 "나쁜지"를 판단해야 하기 때문에 나쁜 것을 최대화 하기 위해서는 음수를 곱해서 양수(최댓값)으로 만들어야 하기 때문  
스코어와 확률은 또 다른 것.  
W*x + bias한 것이 스코어, 이 스코어를 지수로하고 e를 밑으로 하는 새로운 값들을 모두 더한 것을 전체로 해서 정규화를 한 것이 확률임  
분류기는 해당 클래스의 스코어를 양의 무한대 또는 음의 무한대로 분류하는 방향으로 진행하기 때문에 각각의 확률은 1 또는 0에 "가까워" 지는 것. (1 또는 0이 될수는 없다)  

Linear classifier: parametric classifier의 한 종류=> train 데이터 정보가 W로 요약됨  
	이미지를 입력 받으면 하나의 긴 벡터로 폄  
	W*x = [스코어_0, 스코어_1,  …  , 스코어_9] : 10개 클래스를 각각 스코어로 표현, 각 스코어 중 큰 값을 해당 클래스로 분류  
	W 행렬(클래스 수, 이미지 픽셀 수) ==각 이미지 픽셀에 대응되는 요소. 해당 픽셀이 클래스를 결정하는데 얼마나 중요한 역할을 하는지를 의미  
	픽셀 강도 값에 해당하는 고차원 공간에서 픽셀 간 선형 결정 경계를 학습  
손실 함수: W가 좋은지 나쁜지 알 수 있는 방법 (최적화 과정이란 가장 덜 나쁜 W를 찾는 것)  
	최종 Loss는 데이터 셋에서 각 N개의 샘플들의 Loss의 평균  
	multi-class SVM loss(다중분류, 이진분류의 일반화) <->binary SVM(positive, negative)  
	올바르게 분류한 카테고리의 스코어가 그렇지 않은 카테고리에 마진을 더한 값 이상이라면 loss는 0이 됨(예시의 마진은 1이지만 다르게 정해주어도 됨)  
	즉, y_i 카테고리에 대한 loss는 자기 자신의 카테고리를 제외하고 ∑▒〖max(0,s_j – s_y_i +1)〗   
	max(0, quantity) 형태의 손실함수: Hinge loss => x축: s_y_i , y축: loss)  
	y_i(정답) 클래스의 스코어가 다른 스코어와 마진을 더한 것보다 충분히 높아야 Loss가 줄어듦. 따라서 가장 높은 스코어를 조금 조절하더라도 마진으로 인해 loss는 0  
	W 스케일이 변하더라도 loss는 변하지 않음. 마진 또한 리스케일 되기 때문  
	일정 마진을 넘기면 성능 개선을 하지 않음  
Regularization: train set에 맞는 것 완화(패널티)=> 모델이 좀 더 단순한(일반적인)W선택하게 함  
	L1 Regularization: W 요소 대부분이 0이 되게 하는 것을 선호, L2 Regularization: W 요소가 전체적으로 퍼져 있을 때 덜 복잡하다고 생각  
	Elastic net Regularization: L1+L2, Max norm Regularization: L1, L2 대신 max norm 사용  
	Dropout  
	Batch normalization, stochastic depth  
	Softmax Classifier (스코어에 확률이란 의미를 부여하여 분포 계산)  
	e의 지수에 스코어를 취하고(전체는 양의 값이 됨) 그 합들로 나누어 정규화 한 것  
	loss 함수는 얼마나 좋은지가 아니라 얼마나 나쁜지를 측정하는 것. 값이 높을수록 안 좋은 것을 의미하기 때문에 음수 값을 붙임 => -log(정답 스코어에 해당하는 확률 값)  
	확률 1을 얻으려면 스코어는 양의 무한대, 0을 얻으려면 음의 무한대 (단, 최대 또는 최솟값에 도달하진 못함)  
	SVM loss와 달리 성능을 더 높이려 함  

L1과 L2 distance..? 이미지를 한점에 표현.. h는 양수만?  
