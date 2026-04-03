# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 김도현
- 리뷰어 : 송세미

# PRT(Peer Review Template)
- [X]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - CIFAR-10 데이터셋을 활용하여 ResNet-34, Plain-34, ResNet-50 모델 빌드/ 학습을 완료. 특히 Skip Connection 유무에 따른 정확도 차이를 시각화하여 프로젝트의 목표인 Ablation Study 결과를 명확히 만들었음. plt.plot을 통해 생성된 ResNet vs Plain 성능 비교 그래프가 포함되어 있음.
      !<img width="1034" height="653" alt="1-1" src="https://github.com/user-attachments/assets/37ac63ee-ea71-4895-8243-35fcc1160add" />

     !<img width="932" height="658" alt="1-2" src="https://github.com/user-attachments/assets/67da1584-6caa-464f-9c54-fb5c702096d0" />
- [X]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - Yes!
    - Block 클래스와 Bottleneck 클래스를 각각 독립적으로 구현하여 ResNet의 깊이에 따른 구조 변화를 명확히 보여주었음. 특히 Basic Block과 Bottleneck 클래스 내에서 forward 시 identity 값을 저장했다가 마지막에 더해주는 잔차 학습(Residual Connection) 로직이 직관적인 주석과 함께 작성되어 있어 이해가 쉬웠음.
      !<img width="692" height="679" alt="Basic" src="https://github.com/user-attachments/assets/68375445-7418-4ac7-8862-e62f97278d4d" />
      !<img width="960" height="658" alt="Bottleneck" src="https://github.com/user-attachments/assets/5ec948ba-810b-4943-a22e-5978d62649ca" />


- [X]  **3. 에러가 난 부분을 디버깅하여 문제를 해결한 기록을 남겼거나
새로운 시도 또는 추가 실험을 수행해봤나요?**
    - _make_layer 메서드를 통해 스테이지별로 레이어 수와 스트라이드를 유연하게 조절할 수 있도록 동적 빌드 방식을 보여줌. 단순 구현이 아니라, summary를 활용하여 실제 모델의 파라미터 수와 텐서 출력 크기(Output Shape)가 논문 규격인 224x224 입력 시 어떻게 변하는지 체계적으로 검증했음.
      !<img width="863" height="389" alt="3-1" src="https://github.com/user-attachments/assets/e043cb27-e07d-4597-b7f9-57b395e8b072" />
      !<img width="617" height="83" alt="3-2" src="https://github.com/user-attachments/assets/04f71ac9-8cc9-483e-b5a8-28a0af860bf1" />
      
- [X]  **4. 회고를 잘 작성했나요?**
    - Yes!
    - 실험 결과에서 나타난 ResNet-50이 34보다 성능이 낮은 현상에 대해 단순 수치 기록에 그치지 않고, 공학적인 가설을 세운 분석이 회고에서 보임.
      단순한 '실패'로 치부할 수 있는 성능 역전 현상을 데이터셋 크기에 적합한 모델 용량(Capacity)'의 관점에서 재해석하려는 생각이 좋고, 향후 개선 방향(데이터셋 교체 등)까지 명확히 제시됨.
        !<img width="1004" height="593" alt="회고" src="https://github.com/user-attachments/assets/cf7656e9-6f4e-454e-8d78-63178ec70432" />

- [X]  **5. 코드가 간결하고 효율적인가요?**
    - 간결/ 효율 (우수)
    - OOP 원칙을 잘 지켰음. ResNet이라는 메인 클래스 하나로 34층과 50층을 모두 생성할 수 있도록 block 타입을 인자로 받는 범용적인 설계를 구현하여 코드 중복을 최소화함.
       !<img width="1114" height="676" alt="5-1" src="https://github.com/user-attachments/assets/fee6cb73-e22f-4d7d-94d6-54dbe314afef" />

       !<img width="1127" height="282" alt="5-2" src="https://github.com/user-attachments/assets/0d8d62a4-3e40-4ab5-85a9-9ad2f6598cfc" />

# 회고(참고 링크 및 코드 개선)
```
#코드가 커리큘럼대로 파이토치 기반으로 작성되어, 레이어 정의(__init__)와 연산(forward)이 명확히 분리된 점이 인상적이었고, torchsummary 라이브러리를 통해 논문 규격(224 * 224)에서 정상 작동 여부를 검증한 점도 좋았음.

#지금 Bottleneck 구조에서 downsample(stride=2) 시 1x1 Conv를 통한 차원 맞춤 로직이 잘 구현되어 있는데, 여기서 더 나아가 추후 CIFAR-10(32x32) 데이터셋에 최적화하려면 첫 번째 7x7 conv와 MaxPool 레이어의 스트라이드를 조절하는 CIFAR 버전 커스텀을 시도해보는 것도 흥미로운 실험이 될 수 있겠다.

#회고와 코드 발표시 의문점이었던, ResNet-34가 ResNet-50보다 낮은 Loss, 높은 Accuracy를 기록한 원인에 대해 세가지 관점에서 볼 수 있다. 
1. 데이터셋 규모와 모델 복잡도의 불일치로, CIFAR-10과 같은 소규모, 저해상도 데이터에서는 모델 체급이 너무 큰 ResNet-50보다 적절한 복잡도를 가진 ResNet-34가 특징 추출에 더 효율적임.
2. 하이퍼파라미터 최적화 및 학습 시간의 한계로 인해 파라미터 수가 훨씬 많은 50층 모델이 최적점에 수렴하기에는 주어진 Epoch가 상대적으로 부족했을 가능성이 큼.
3. 50층부터 적용된 Bottleneck 구조는 연산 효율성에는 유리하나, 데이터가 충분하지 않은 상황에서는 Basic Block의 직관적인 연산 방식이 학습 안정성 측면에서 우위를 점한 것으로 보여짐.
결과적으로, 이번 실험은 모델의 depth보다 데이터셋의 특성에 최적화된 적정 model capacity를 선정하는것이 중요하다는것을 알려준다고 생각한다.
```
