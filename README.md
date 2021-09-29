# BLDC_control

## 1. 브러시리스 DC 모터(BLDC)의 개념

전기 에너지를 회전 운동으로 변환해 주는 브러시리스 DC 모터는 매일 사용하는 장치부터 더 복잡한 기계까지 어디에나 사용됩니다.

### Brusehless DC motors 특징

- Low maintenance
- High efficiency
- Complex control(DC to AC)
- Electronic commutation

 BLDC를 브러시 모터가 뒤집힌 버전으로 이해해도 무방합니다. BLDC에서는 영구 자석이 회전자가 되고 코일 권선이 고정자 입니다. 고정자의 권선 개수가 다르고 회전자에 여러 극 쌍이 있는 등 자석 배열이 서로 다른 모터가 있습니다. BLDC는 회전자에 영구 자석이 있는 동기식 모터로 정의됩니다. 동기식 모터의 주요 차별화 요소는 역기전력 전압의 형태입니다. 모터는 회전할 때 발전기 역할을 합니다. 따라서 모터의 구동 전압에 반대되는 역기전력 전압이 고정자로 유도됩니다. 역기전력은 모터의 중요한 특성입니다. 역기전력의 형태를 살펴보면 모터의 유형뿐 아니라 모터를 제어하는 데 필요한 제어 알고리즘의 종류도 알 수 있기 때문입니다. 

 BLDC의 역기전력은 사다리꼴 모양이며 일반적으로 사다리꼴 제어에 의해 제어됩니다. 이와 다르게 PMSM은 정현파 역기전력을 보이기 때문에 자속 기준 제어의 의해 제어됩니다. 역기전력의 형태를 쉽게 관찰하는 방법은 시뮬레이션을 사용하는 것입니다. 개회로 단자를 사용하여 극 쌍이 하나인 BLDC를 시뮬레이션할 수 있습니다. 즉 코일은 구동되지 않습니다. 하지만 회전자가 발전기처럼 작동하도록 약간의 토크를 적용하면 회전자를 회전시킨 후에 A 위상에서 전압을 측정하면 A위상 역기전력이 나옵니다. BLDC의 역기전력이 사다리꼴 이며 전압이 평평하게 유지되는 영역이 포함됩니다. 이는 곧 DC 전압을 사용하여 이 모터를 제어할 수 있다는 뜻입니다.

 모터 내부에는 단일 극 쌍으로만 구성된 회전자와 120도 간격의 코일 3개로 구성된 고정자가 있는 간단한 구성을 사용합니다. 코일은 코일을 통과하는 전류를 통해 동력을 받는데 이를 A, B, C라고 지칭하겠습니다. 두 위상에 전압을 가하면 A 위상과 C 위상 사이에 혼합 자기장을 생성합니다. 그 결과 회전자가 회전하기 시작하면서 고정자 자기장과 함께 일직선을 이루게 됩니다. 회전자 각도는 수평축에 따라 측정되며, 각각 60도씩 떨어진 각기 다른 회전자 배치 6개가 생깁니다. 즉, 우리가 60도마다 올바른 위상을 정류할 수 있다면 모터를 회전시킬 수 있다는 뜻입니다. 이를 6단계 정류 또는 사다리꼴 제어라고 부릅니다. 더 많은 극 쌍을 사용하면 정류가 더 자주 발생합니다. 모터를 적절한 시기에 올바른 위상으로 정확하게 정류하려면 회전자 위치를 알아야 합니다. 이 위치는 일반적으로 홀 센서를 사용하여 측정합니다.

 회전자와 고정자에서 같은 종류의 두 개의 극은 서로 밀어내며 회전자가 시계 반대 방향으로 돌게 만듭니다. 동시에 반대편 극은 서로 끌어당겨 회전자를 계속해서 같은 방향으로 회전시킵니다. 60도 회전이 완료되면 다음 정류가 발생합니다. 이 때 회전자가 고정자 자기장과 일적선상에 놓이지 않고 항상 자기장을 쫒기는 방식으로 정류가 발생합니다. 이 움직임을 설명할 수 있는 두 가지 사실이 있습니다. 첫째, 회전자 및 고정자 자기장이 완벽하게 일직선 상에 놓이면 토크는 발생하지 않습니다. 따라서 두 자기장이 절대 일직선상에 놓이지 않도록 해야 합니다. 둘째, 자기장이 서로 90도를 이룰 때 최대 토크가 발생합니다. 따라서 목표는 두 자기장의 각도를 90도에 가깝게 하는 것 입니다. 그러나 사다리꼴 제어의 단순한 특성 때문에 BLDC 모터에서는 6단계 정류로 90도를 달성하지 못하는 대신 각도가 일정 범위 내에서 변동합니다.

 6단계 정류의 위상을 제어하려면 3상 인버터를 사용하여 DC 전원을 3상 전류로 변환해야 합니다. 모터를 일정한 속도로 유지하려면 3상 인버터로 정전압을 변환해야 합니다. 반면 모터를 다양한 속도로 제어하려면 인가 전압을 조정할 수 있어야 합니다. 이를 위한 방법 중 하나는 PWM을 사용하는 것입니다.

## 2. 브러시리스 DC 모터(BLDC)의 제어

 3상 인버터에는 정전압을 공급하는 DC 전압원이 있습니다. 이 전압원은 DC 전원을 3상 전류로 변환하여 서로 다른 코일 쌍에 동력을 공급합니다. 인가 전압이 일정하면 모터는 전압과 속도 간의 비례 관계에 따라 일정한 속도로 회전합니다. 하지만 다양한 속도로 모터를 제어하려면 인가 전압의 진폭을 조정할 컨트롤러를 구축해야 합니다. 

 올바른 위상은 3상 인버터의 스위칭 패턴을 계산하는 정류 논리 회로에 의해 지정됩니다. 정류 논리 테이블에서 문자 A, B, C는 모터의 3상을 나타냅니다. 3상 인버터의 위쪽을 H로, 아래쪽을 L로 표시합니다. 모터를 다양한 속도로 회전시키기 위해서는 적절한 컨트롤러로 루프를 닫으면 전압을 조정할 수 있습니다. 원하는 속도와 측정된 속도간의 차이를 기반으로 컨트롤러가 전압을 조정하여 모터 속도를 원하는 값에 가깝게 바꿉니다. 

 정류가 일어날 때 A위상은 H이고 C위상은 L이고 B위상은 개방 상태입니다. 정류 중에 3상 전류가 바뀌는 경우 속도에서 리플 패턴이 일어나지 않습니다. 하지만 실제로는 한 위사이 움직일 때는 전류가 순간적으로 바뀌지 않습니다. 3상 전류를 보면 시간의 흐름에 따라 위상이 상승하며 이에 따라 속도에 리플 현상이 일어나는 것을 볼 수 있습니다. 영향을 받는 신호는 속도 뿐만 아닙니다. 전류와 토크는 비례 관계이므로 토크 응답에서도 리플을 관찰할 수 있습니다. 토크 응답의 리플은 BLDC 모터의 사다리꼴 제어가 가지는 단점 중 하나로 꼽힙니다.

 3상 전류를 봤을 때 한 위상이 개방 상태에서 높이 올라가는 경우 위상 전류가 다시 상승하기 전에 갑작스러운 변화가 발생합니다. 동시에, 정류 도중 낮게 유지되었던 위상에서도 또 다른 급변화가 발생합니다. 정류 중 위상 전류가 증가함에 따라 동력이 공급되는 위상에 자기장이 구축됩니다. 정류 중에 위상 중 하나, 예를 들어 A위상이 개회로가 되면, 이 개방에 의해 구축된 자기장이 붕괴합니다. 따라서 위상 전류가 0으로 떨어집니다. 이때 완전히 구축된 자기장을 가진 C위상이 B위상에 연결되면 C위상에 구축된 자기장이 거의 즉시 붕괴하는 반면, 동시에 B위상은 이 붕괴에 대응하여 자기장을 구축합니다. 이 B위상에서 갑자기 구축된 자기장 때문에 위상 B전류에서는 급변화를 관찰할 수 있습니다. 또한 C위상에서의 자기장 붕괴로 인해 전류가 급격하게 감소합니다. B위상과 C위상의 자기장이 50%의 자기장 강도에서 균형을 잡기 때문에 진류 진폭은 절반까지 감소합니다. 이러한 위상 전류의 순간적인 변화로 인해 3상 전압에서 스파이크가 관찰됩니다. 이러한 현상을 유도 플라이백이라고 합니다. 

## 3. PWM을 사용한 BLDC 속도 제어

 3상 인버터에 전압을 공급하려면 우선 PWM 또는 펄스 폭 변조라는 기술을 사용하여 고정 전압을 변조해야 합니다. PWM(Pulse Width Modulation) 신호의 모양은 특정 주파수에서 반복되는 구형파 신호입니다. PWM은 특정 주파수에서 일련의 온/오프 펄스를 사용하여 모터에 DC 전압을 가하는 스위치와 같은 역할을 합니다. 이 신호의 듀티 사이클을 계속 변경함으로써 평균 전압을 조절하고 다양한 속도로 모터를 제어할 수 있습니다. 참고로 듀티 사이클이 길어질 전압은 더 높아집니다. 

 1/주기 단위로 계산되는 PWM 주파수를 선택할 때 주의해야 합니다. 스위칭 주파수가 너무 낮으면 모터에 평균 전압 대신 구형파 모양을 따르려는 전압이 표시됩니다. 이 경우 기준 속도를 제대로 추적하지 못하게 되어 모터가 계속 속도를 높이고 낮추는 일을 반복합니다. 반면 PWM 주파수를 적당한 값으로 증가시키면 전압은 평균화되어 속도 제어 성능이 향상됩니다. 이 때 리플은 PWM의 스위칭 특성으로 인해 발생합니다. 일반적으로 BLDC 모터를 제어하는 PWM 주파수는 대략 수 킬로헤르츠이며 모터 시간 상수의 역수보다 훨씬 높게 선택되어야 합니다.
