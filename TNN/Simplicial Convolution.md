# Simplicial Neural network

### Edge Feature Update

$$ Y = \psi (L^{\downarrow} _{1} X_1 W_1 + L^{\uparrow} _{1} X_1 W_2 + B^{T} _{1} X_0 W_2 + B_2 X_2 W_4 + x_1 W_5)$$

왼쪽부터 하나씩 Lower adj. / Upper adj. / Boundary adj. / Coboundary adj.

---

Node - Edge - Face 각자의 관점에서 서로의 관계를 통해 얻을 수 있는 정보를 통합해서 훈련시킴.

#### Simplicial Convolution은 Graph Incident Matrix라고 한다.












