# MNIST-dataset-classification

먼저 mnist의 기본예제를 통해 기본 구조를 익히고 넘어가자.
출처: https://www.kaggle.com/uysimty/get-start-image-classification

![image](https://user-images.githubusercontent.com/81463668/113806532-68de7080-979d-11eb-8e23-2118c2e23aa8.png)

연산을 위해 numpy를 가져오고 pandas를 이용해서 dataframe만들기위해 import를 해준다.
keras의 이미지 전처리 기능에서 함수들을 가져오고 사이킷런을 import해와서 train과 test를 나누어서 set로 설정해주기 위한 것을 알 수 있다.
matplot library를 import해 나중에 결과나 이미지를 나타내려고 하는 것을 알 수 있다.
parameter들을 초기화해주기 위해 random을 가져왔고 os를 import해서 어느 부분의 디렉토리로 이동, 파일을 가져오거나 수정할 수 있게 했다.

![image](https://user-images.githubusercontent.com/81463668/113806545-6ed45180-979d-11eb-93c0-ddd3f828f40e.png)

model에 대한 설정이다. training을 할 때 반복을 100번하고, mini batch사이즈를 32로 설정, FASTRUN 모드일 때는 학습을 한번만 하게 한다.

train과 test에 대한 data를 csv파일에서 pandas를 사용해서 가져온다.

![image](https://user-images.githubusercontent.com/81463668/113806552-74ca3280-979d-11eb-90f2-e52f52ee979d.png)

가져온 train data의 형식을 보면 라벨과 image의 pixel들로 구성되어있는 것을 확인할 수 있다. test data도 같은 형식을 가질 것이다.

![image](https://user-images.githubusercontent.com/81463668/113806560-798ee680-979d-11eb-8775-1515d5187fd7.png)

show image 함수를 통해 matplot으로 이미지를 표현해내는 코드이다.
train_data에 임시적으로 sample image를 넣어서 출력하려고 하는 코드이다.
출력하게되면...

![image](https://user-images.githubusercontent.com/81463668/113806577-827fb800-979d-11eb-92a4-a5b27971f1c9.png)

show 함수가 기능을 잘 하는 것을 확인.

이제 진짜 데이터를 가져와서 학습하기 전에 전처리를 할 것이다.

![image](https://user-images.githubusercontent.com/81463668/113806595-8a3f5c80-979d-11eb-84db-50a7e644917b.png)

좀 전에 나왔던 import가 한번 더 여기서 나왔는데 확실하게 import해주기 위해서 가져온 것같다. train data를 reshape를 시켜줘서 dimension을 변결하고 train data안에 들어있는 label이라는 string을 모두 지운다. 그 후 x에 대입.
to_categorical 함수를 사용해서 train data의 label부분에서 index를 가져와 index부분은 1 나머지는 0인 tensor를 갖게하고 y에 대입힌다.
그 다음에. 사이킷런을 사용해서 train과 test로 데이터셋을 나누어준다.

![image](https://user-images.githubusercontent.com/81463668/113806601-90cdd400-979d-11eb-9a24-6db3f5ea8d0f.png)

이미지 전처리에 관한 코드이다. 
이미지를 얼마만큼 돌리고(rotation) 사이즈를 변경하고 확대할지 등등.. 이미지에 약간씩의 변화를 줘서 augmentation하는 과정이라고 보면된다. train과 test 모두 적용시키는 코드를 볼 수 있다.

![image](https://user-images.githubusercontent.com/81463668/113806613-962b1e80-979d-11eb-8617-078db5b248e8.png)

keras에서 models.Sequential을 가져와 신경망의 층을 만들어 Sequential에 쌓아 한번에 처리하기 편하게 하려고 한다.
미리 만들어진 keras.layers안의 여러 가지 층을 만들어주는 함수를 가져온다.
model이라는 Sequential 객체를 만들어주고 model에 convolutional층을 통과 시킨다. 활성화함수는 모두 relu를 썼고 처음 convolutional 층은 64개, 두 번째 층은 32개를 사용한다.
그 이후 maxpooling을 이용해 대푯값을 추려주고 이번 마지막의 신경망에서 FC층이 존재하므로 1차원으로 만들어주기 위해 Flatten함수를 사용한다. 그리고 Dense함수를 이용해 Fully connected한 신경망을 만들고 노드의 개수는 10개 활성화함수는 softmax로 설정해준다.

![image](https://user-images.githubusercontent.com/81463668/113806636-9fb48680-979d-11eb-8799-c00039aa1468.png)

model을 학습시키기전에 optimizer 알고리즘은 rmsprop로 정해주고 loss function은 categorical_crossentropy 그리고 정량적인 지표는 정확도를 나타내도록 한다.

![image](https://user-images.githubusercontent.com/81463668/113806651-a6db9480-979d-11eb-9b25-a9e34210b31c.png)

만약 training 도중에 런타임이 끊긴다거나 오류가 발생할 수 있기 때문에 학습중간까지 학습된 정보들을 저장하고 가져와줄 수 있는 tool을 마련한다.
early stopping의 본래기능은 너무 train set에 많이 학습되면 cost가 낮아지다가 overfitting 되면서 cost가 되려 높아지는데 이렇게 높아지기 전에 학습을 끝내버린다.

![image](https://user-images.githubusercontent.com/81463668/113806660-ac38df00-979d-11eb-8e8a-4b6124c1f3bf.png)

이제 모든 설정이 끝났으므로 model에 미리정해둔 batch와 epoch으로 학습을 진행시킨다.

![image](https://user-images.githubusercontent.com/81463668/113806671-b0fd9300-979d-11eb-83f6-e40fc8800c72.png)

이런식으로 학습이 진행되면서 loss와 accuracy가 표시된다.
여기서 아까 구성한 tool에 의해서 epoch이 진행되도 validation set에서 큰 loss가 줄어들지 않으면 improve하지 않았다고 뜨게할 수 있다.
![image](https://user-images.githubusercontent.com/81463668/113806682-b529b080-979d-11eb-9ef1-822bd2698e0e.png)

이제 모든 학습이 끝났으니 model에 test값을 넣어서 실제로 예측값이 얼마나 label과 비슷한지 확인해본다.

![image](https://user-images.githubusercontent.com/81463668/113806692-b955ce00-979d-11eb-9217-fbef3b48db24.png)

test도 동일하게 rescale을 진행해주고 keras의 evaluate 함수에 test값과 test의 ground-truth값인 y값을 넣어준다. 그렇게 되면 score값이 나오게 되고 이 score값은 accuracy와 loss값으로 이루어져 있다.
학습이 매우 잘되서 정확도가 굉장히 높은 것을 확인할 수 있다.


