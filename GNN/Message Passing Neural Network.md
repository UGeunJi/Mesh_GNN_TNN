# Message Passing Neural Network

NPNN은 기본적으로 정보를 종합하고(aggregate)하고 update하는 message passing phase와 이를 활용해서 결과를 도출하는 readout phase로 이루어진다.

## Message Passing Phase

message passing phase는 정보를 종합하는 역할을 하는 message function과 hidden state를 update하는 역할을 하는 update function으로 이루어진다.

### Message function

Message function은 노드 v에 대한 정도를 얻기 위해 정보들을 종합하는 역할을 한다. 노드 v에 대한 message function은 다음과 같은 형태를 가진다.

<img width="443" alt="image" src="https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/f964deaf-0f27-440b-91e7-beb4a0636bda">

위 식에서 hi는 노드 i에 대한 hidden state, N(v)는 노드 v의 neighborhood, evw는 edge vw의 feature vector, Mt는 이 모든 것을 종합하는 message function이다. <br>
즉, message function은 우리가 알아보고 싶은 노드의 현재 상태, 그 노드의 이웃들의 현재 상태, 그리고 그 노드와 노드의 이웃들을 연결하는 엣지들의 정보를 종합하여 우리가 알아보고 싶은 노드의 다음 message를 표현하는 것이다.

ex)

<img width="254" alt="image" src="https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/93a6c963-3e6f-4ba5-9fca-8150518e00db">

A의 정보를 얻고 싶다면 neighborhood인 B, C, D의 정보를 종합해서 message를 얻을 수 있을 것이다. 이 이웃한 노드 B, C, D의 정보 또한 이웃하는 neighborhood를 통해서 얻으면 된다.

<img width="555" alt="image" src="https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/b77995ca-ff21-411d-864c-fd2133f0c6ae">

### Update function

Update function은 이렇게 얻어진 message를 활용해서 다음 hidden state를 update하는 역할을 한다. 노드 v에 대한 update function은 다음과 같은 형태를 가집니다.

<img width="273" alt="image" src="https://github.com/UGeunJi/Mesh_GNN_TNN/assets/84713532/b32ceb9a-a4f9-412b-8a78-9e20c821d68a">

즉, 앞에서 얻었던 message function (m)과 함께 노드의 현재 hidden state(h)를 고려하여 노드의 다음 hidden state를 update하는 것이다.

### readout phase

다음으로, readout phase에서는 이렇게 우리가 얻은 hidden state를 활용해서 우리가 예측하기를 원하느 노드의 label, 그래프의 label 등을 도출한다. <br>
이 hidden state를 readout function에 넣어서, 우리가 원하는 label을 도출하는 것이다.

<br>
<br>

---

## 장점

이러한 MPNN 구조가 가지는 장점은 그래프의 structural한 정보와 feature 정보를 모두 얻을 수 있다는 것이다. <br>
어떤 노드의 embedding을 얻음으로써 우리는 그 노드의 k-hop 이웃들의 정보를 모두 반영할 수 있고, <br>
이는 노드가 어떤 노드들과 연결되어 있는지의 structural한 정보를 반영할 수 있다고 할 수 있다. <br>
또한, k-hop 이웃들의 feature들 또한 종합하여 반영함으로써 feature 정보까지 반영할 수 있다.
