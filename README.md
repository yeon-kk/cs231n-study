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
