# cs231n-study

cifar-10

L1 distance
test image - training image (단순 픽셀위치에서의 절댓값 차이)  L1의 경우 test할 때, test set 수에 비례해서 선형적으로 시간이 증가  (<->CNN은 test시 동일)  
k-NN : k-Nearest Neighbor. k개중 가장 가까운 이미지를 다수결로.( k가 있을 때 더 부드러운 결과). 즉 k개의 이미지를 선정(배 사진 + 오분류된 말 ..)해서 다수결로 정해진 것을 정답이라 말함  

train이미지를 다시 검증하면?  
NN :  정확도는 100%. 자기 자신과의 비교
k-NN : 경우에 따라 다름. 다수결이기 때문에 나머지가 다른 클래스로 예측한다면 바뀌게 됨
