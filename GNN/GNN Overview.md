## [참고 링크](https://velog.io/@whattsup_kim/Graph-Neural-Networks-%EA%B8%B0%EB%B3%B8-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)

<br>

# Graph란

```
꼭지점(Node)들과 그 노드를 잇는 변(간선, Edge)들을 모아 구성한 자료구조를 의미.

간선의 타입은 다음과 같이 나눌 수 있다.
 - 간선에 방향이 있는가?
    -> directed / undirected
 - 간선에 가중치가 있는가?
    -> weighted / unweighted
```


# Graph의 표현

<br>

## Adjacency matrix(인접행렬)

```
그래프를 0과 1의 행렬로 표현한 방법
유한 그래프는 컴퓨터에서 정사각 행렬의 형태로 표현될 수 있으며, 여기서 행렬의 부울 값은 두 정점 사이에 직접 경로가 있는지 여부를 나타냄
```

<br>

### 장점

- 간선 추가, 간선 제거, 정점 i에서 정점 j까지 간선이 있는지 확인하는 것과 같은 기본 작업은 매우 시간 효율적이며 일정한 시간 작업
- 그래프가 조밀하고 간선 수가 많으면 인접 행렬을 먼저 선택해야됨. 그래프와 인접 행렬이 희소 행렬인 경우에도 희소 행렬의 데이터 구조를 사용하여 표현 가능
- 그러나 가장 큰 장점은 행렬을 사용한다는 것. 최근 하드웨어의 발전으로 인해 GPU에서 비용이 많이 드는 행렬 작업도 수행할 수 있게 됨
- 인접 행렬에 대한 연산을 수행함으로써 그래프의 특성과 정점 간의 관계에 대한 중요한 통찰력을 얻을 수 있음

<br>

### 단점

- `VxV` 인접 행렬의 공간 요구 사항으로 인해 메모리를 많이 차지. 실제 그래프에는 일반적으로 연결이 너무 많지 않으며 이것이 대부분의 작업에 인접 목록이 더 나은 선택인 주요 이유
- 기본 작업은 쉽지만 인적 행렬 표현을 사용할 때 `inEdges` 및 같은 작업은 비용이 많이 듦. `outEdgeds`

<br>

### 종류

- Undirected

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/9163a403-ee41-4d86-8210-1f057c8ab685)

<br>

- Directed

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/a0b71200-9c29-4a4c-9a9a-5e80343805d1)

<br>

- Directed + Self-Loop (Adjacency + Identiry matrix)

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/20f1fb69-b6fa-4394-ac0a-e54ea3068908)

<br>

- Weighted directed (Edge Information이 있음)

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/7d42b89d-0625-4f92-bcef-4258fb7bf51f)

<br>
<br>

## Degree Matrix

```
각 노드가 주변과 몇 개나 연결되어 있는지를 표현해놓은 matrix
```

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/fc44c521-234f-44a2-81e9-c721e8c3999d)

<br>
<br>

## Laplacian Matrix

degree matrix에서 adjacency matrix를 뺀 matrix

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/5060f6ad-4e05-4d05-830d-f06ed88bd00d)

<br>
<br>

## Node-feature Matrix

```
각 노드에 hidden representation이 있을 때, 그것들을 모아놓은 matrix
```

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/728ac454-76e9-478d-9d2d-361575c7fbb1)

<br>
<br>

---

# GNN이란

```
GNN(Graph Neural Network)이란, 그래프로 표현할 수 있는 데이터를 처리하기 위한 인공 신경망의 한 종류

 - CNN: 이미지에서 특징 추출
 - RNN: 시계열 데이터에서 특징 추출
 - GNN: 그래프 구조에서 특징 추출

GNN을 통해 우리는 각 노드를 잘 표현할 수 있는 임베딩 추출(Efficient node embedding)을 기대할 수 있음
```

<br>

- GNN은 그래프 구조를 활용하여 loss를 최적화시키는 과정이다.

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/7cbb0457-e29d-4f30-a70c-b51074445196)


```
point-wise loss: Loss function에서 한번에 하나의 아이템만 고려
                 하나의 Query(=User)가 들어왔을 때, 이에 대응하는 하나의 item만 가져와서, Score(User, Item)를 계산하고, 이를 Label score와 비교해서 최적화
                 종류: LSE

pair-wise loss: Loss function에서 한번에 2개의 아이템을 고려
                1개의 Positive itme, 1개의 negative item을 고려.
                Pos, Neg item pair가 들어오면, Pos item > Neg item이라는 Rank가 자연스레 형성되고, 이를 통해 학습과정 부터 Rank를 고려할 수 있게 됨
                종류: BPR, WARP, CLiMF 

list-wise loss: Loss function에서 한번에 전체의 아이템을 고려
                잘되면 가장 좋은 방법이라고 생각할 수 있지만, 복잡함
                2가지 방법론
                    - NDCH 등의 IR measure을 직접적으로 최적화하는 방법론: SoftRank, AdaRank
                    - 자신이 필요한 Rank의 특성을 이해하고, 새롭게 정의한 Loss를 최적화하는 방법론: ListNet, ListMLE
```

<br>
<br>

## GNN 방법론

- Spectral
- Spatial

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/72bf7be7-2a45-4da9-93cd-d58ab12295b8)

<br>

```
발전 과정을 보면 초기에는 Spectral한 방법으로 접근하였지만 현재에는 Spatial한 방법론들을 사용하는 것을 확인할 수 있음

GCN은 Spectral -> Spatial로 연결해주었던 아키텍쳐. GCN 등장 이후 Spatial 방법론의 연구가 주를 이루게 됨

Spatial Methods는 Neighborhood Aggregation으로 간단히 표현할 수 있음.
즉 어떠한 Target Node의 주변 정보를 가지고 자기 자신을 업데이트하는 방식
```

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/f6b48708-2a9a-41d5-9a2e-4bf2bb2eb994)

<br>
<br>

## GCN (Graph Convolutional Networks)

#### Euclidean vs Non-euclidean

<br>

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/c05deccb-91a6-427f-b3a9-6e1e6502656f)

<br>

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/5b555df2-8efd-4cfb-bb78-d669a3ae9a8c)

<br>

- 이미지나 텍스트, 정형 데이터는 격자 형태로 표현 가능. 즉, 유클리디언 공간상의 격자 형태로 표현 가능.
- 유클리디언 공간에서는 '거리'가 중요
- 반면 소셜 네트워크와 분자 데이터 등은 유클리디언 공간으로 표현할 수 없음
- 논유클리디언은 연결 '연결 여부'와 '연결 강도'가 중요합니다.

<br>

## Convolution (on Graphs)

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/7c584d3b-9894-4d0e-96f9-27a1b106126c)

- CNN에서 사용되는 Convolution은 결국 커널을 움직여가며 커널 크기에 해당하는 정보들을 한 곳에 모으는 과정
- Graph에서는 (Target)노드와 연결된 이웃들의 정보를 weighted average함으로써 Convolution 효과를 만들어냄

<br>

## 가중치 업데이트 과정

### 가중치 업데이트 계산

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/11442146-afe8-475a-a1e0-847aa0a68943)

- Target node(그림에서 1번)의 주변 노드에 대한 정보를 학습
  - 각 벡터 값에 파라미터를 곱하고, 이를 모두 더함(Convolution 효과)
  - A는 Adj matrix를 의미
- 이때, 학습되는 파라미터(W)는 공유됩니다.

<br>

### GCN은 결국, AH * W

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/f3753ffc-169f-4120-88f7-8b7b8eafda1c)

- 위 과정을 통해 계산된 output은 1번 업데이트된 임베딩임
- Shape
  - A: N * N
  - H: N * h
  - W: H_in * H_out
  - output: N * H_out

<br>

### 가중치 계산에서의 한계점과 해결책

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/8bfcbbee-d16f-4fff-a00d-5f7f2f082eb3)

```
2번에 대한 식
```

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/92044072-372f-452b-8daa-2561393fec52)

<br>
<br>

## GAT (Graph Attention Networks)

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/5782e44e-1f5a-4f36-89b9-72adaa9f903f)

- GAT는 Spatial한 방법론을 사용. 따라서 Adj matrix를 사용하지 않음.

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/2e896852-7a9d-4175-a312-619b8caa488c)

-  GAT는 단순히 합치는 것이 아니라, 노드 별 상이한 weight를 가지고 가중합하는 방식. 즉, '나'한테 영향을 주는 '정도'까지 학습.

<br>

## 가중치 업데이트 과정

### 가중치 업데이트 계산

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/f56cd935-eaa9-4c4e-9d6c-e287389aa536)

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/cb5e30bf-b0b0-4021-a783-b2ece88c713f)

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/b97f2103-ac54-4643-98a8-c4dbff8a893f)

### Multi-head Attention(MHA)

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/5b6af76e-3983-4f11-abcc-de53092ae8d0)

- GAT에서도 Multi-head Attention을 사용할 수 있습니다.
- GAT에서의 MHA는 단순히 가중치 업데이트 계산 과정을 여러 번 수행한 것으로, 이를 Average하거나 Concatenate하여 사용합니다.

<br>
<br>

## Receptive Field

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/172c79b3-b017-4285-8c95-0c6536a182b1)

- 위와 같은 네트워크가 있습니다.

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/1880e88a-ec1a-4dc9-981b-5981e3212737)

- 분홍색 원을 Target Node라고 했을 때, 한 번 GCN을 수행하면 자기 주변(한 칸) 이웃들의 정보를 모으게 됩니다.
- 한 번의 GCN을 거치면, 이와 같은 과정이 모든 노드들에 수행될 것입니다.
- 즉, 한 번의 GCN이 수행된 후 Target Node의 바로 주변 이웃들은 그 옆 이웃들의 정보(Target Node로부터 두 번 떨어진 노드들의 정보)도 포함하고 있게 됩니다.

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/8a6878db-83c0-4845-b79d-9823ab1a7a50)

- 따라서 GCN을 N번 수행하게 되면, N번 떨어진 노드들의 정보까지 Target Node가 학습할 수 있게 되는 것입니다.
- CNN에서 Convolution을 여러 번 수행하면 Receptive Field가 넓어지는 것과 마찬가지로, GCN에서 Layer를 깊게 쌓으면 더 넓은 주변 정도를 가져올 수 있습니다.
- GNN에서는 이를 hop이라고 표현합니다.

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/9edb267d-604c-4850-805a-52b52569175f)

<br>
<br>

## Over-Smoothing Issue

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/3baf94ce-f47a-48f6-a07e-62b05021e740)

- GNN은 결국 주변 정보를 이용하여 Node들을 임베딩하는 것입니다. Layer가 너무 깊어지게 된다면 '주변 정보'가 아니게 될 수 있음

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/0a366e2f-16c2-405d-b604-e8c26c2b84fb)

- 이러한 Over-Smoothing Issue를 방지하기 위해 여러가지 방법을 적용할 수 있음
  - Node Dropout
  - Edge Dropout
  - Layer-wise Edge Dropout(Random Walk): Layer별로 Edge Dropout을 다 다르게 하는 것
  - PairNorm: centering + rescaling을 통해 펼치는 것

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/8f357a63-bf55-4b04-b99a-10e8407c0fb9)

<br>
<br>

## Aggregate(Message Passing) & Combine(Update)

- 전체 구조

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/1bdaa43f-3440-40d1-9268-9cd2b08ac741)

<br>

- Aggregate(Message Passing)

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/fb37174d-95d7-47bf-b31f-39ce81a17686)

- 타겟 노드의 이웃 노드들의 k-1 시점의 hidden state를 결합하는 것입니다.
- 즉, 주변 노드들의 정보를 가져오는 단계입니다.

<br>

## Conbine(Update)

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/2d1c7d2a-db8d-453a-a60b-888905260657)

- k-1 시점 타겟 노드의 hidden state와 Aggregated Information을 사용하여 k시점의 타겟 노드의 hidden state를 업데이트합니다.
- 즉, Aggregate에서는 자기 자신의 정보를 고려하지 않았으므로, 이를 고려해주는 것(conbine)을 의미합니다.

-> A + I

#### Readout: K 시점의 모든 Node들의 Hidden state를 결합하여 Graph-level의 hidden state를 생성하는 것입니다.

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/ef9cc396-03a5-40f1-87b8-fbd2d40b2107)

## Message Passing 정리

- Message: 엣지 별 전파시킬 값 계산 (Target에게 얼만큼 줄 것인가?)
- Aggregate(add, mean, ...): 노드 별 합산 (가져온 것을 어떻게 합칠 것인가?)
- Update(=combine: concat, sum, ...): 합산 결과 반영 (원래 자기 것과 Aggregate된 것(주변정보)를 어떻게 합칠 것인가?)

<br>
<br>

## Message Passing: GCN

![image](https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/dac5b644-5fce-48b4-a3c6-7b0f4211b1ad)

- 각 이웃 노드별로 얼만큼 message passing을 할 것인지(얼만큼 정보를 전달할 것인가?)가 핵심 포인트라고 할 수 있음
  - GCN에서는 이것이 단순 가중합(같은 비율만큼 합쳐짐)이었다면, GAT에서는 이를 Attention Score(Attention Coefficient)만큼 다르게 합침












