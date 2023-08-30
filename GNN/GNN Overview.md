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

# GNN






















