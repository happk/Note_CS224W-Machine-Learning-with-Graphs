# 12 Identifying and Counting Motifs in Networks

- subgraph matching

### 1

在一個 graph中，找到 "常見" 的 subgraphs

![12-motifs_頁面_02](slide/12-motifs_%E9%A0%81%E9%9D%A2_02.png)

- subgraphs 是組成network 的成分。具有特別的意義。( [>Why do we need motifs?](##Network Motifs))

- ex:  化學分子中的"官能基"

  ![12-motifs_頁面_03](slide/12-motifs_%E9%A0%81%E9%9D%A2_03.png)

---

如何辨別出這些高頻出現的subgraph

1. 
2. GNN產生embedding表示subgraphs
3. 在embedding space中識別

![12-motifs_頁面_04](slide/12-motifs_%E9%A0%81%E9%9D%A2_04.png)

# 12.1 Subgraphs and Motifs

## Subgraphs 

2 ways to formalize "network building blocks (*subgraphs*)"

1. induced subgraph
2. spanning subgraph
   含所有的點（即點集合同原本的graph），但不一定有所有的邊

(現在沒人再提edge-induced subgraph了，所以induced subgraph = node-induced subgraph。
不過先跟著slide作筆記吧 )

根據不同的定義，是不同類型的subgraph。



### 1. Node-induced subgraph (induced subgraph)

取一個"節點子集"和"連接這些節點的所有邊"。(node-induce 由節點決定)

![12-motifs_頁面_06](slide/12-motifs_%E9%A0%81%E9%9D%A2_06.png)

$G'=(V',E')$ 

如果G'是一個node-induced subgraph 則滿足:

- $V^{\prime}\subseteq V$

  節點集V' 是原圖G的節點集V的子集

- $E^{\prime}=\{(u,v)\in E\mid u,v\in V^{\prime}\}$

  邊集E' 是原圖中兩端點都屬於該V'的所有邊的集合

  

### 2. Edge-induced subgraph (non-induced subgraph)

![12-motifs_頁面_07](slide/12-motifs_%E9%A0%81%E9%9D%A2_07.png)

V' 為所有的選擇的邊 E′ 中的相關聯的節點

### Graph isomorphism

先前定義的subgraph中，$V' \subseteq V \text{and} \ E' \subseteq E$ 都源自 原圖 G。

![12-motifs_頁面_09](slide/12-motifs_%E9%A0%81%E9%9D%A2_09.png)

那要如何定義 V', E' 來自完全不同的 graph。

> 例如 "**𝑮𝟏 is “contained in” 𝑮𝟐**" 
> G~1~ 的三角結構出現在 G~2~ 中

如何表示 G~1~ 被包含在一個更大的G~2~中?

---

- **Graph isomorphism problem**: Check whether two
  graphs are identical

  G~1~ 與 G~2~ 是 isomorphic，如果存在bijection。

  - bijection: 一個圖的節點與另一個圖的節點存在一一對應的關係使得所有邊都得以保留
    -> 若 u, v 在 G1中相連，則mapping of u, mapping of v 在G2中也相連

    (Slide中的 bijection

    $\begin{aligned}&f{:} \bar{V}_{1}\to V_{2} \mathrm{such~that} (u,v)\in E_{1} \\& \mathrm{iff}\left(f(a),f(b)\right)\in E_{2}\end{aligned}$ 

    應該改成

    $\begin{aligned}&f{:} \bar{V}_{1}\to V_{2} \mathrm{such~that} (u,v)\in E_{1} \\& \mathrm{iff}\left(f(u),f(v)\right)\in E_{2}\end{aligned}$

    )

    - mapping f : isomorphism

      映射 *f*

> NP-hard ?

![12-motifs_頁面_10](slide/12-motifs_%E9%A0%81%E9%9D%A2_10.png)

(沒有提到weight of edge)

### -

𝐺~2~ is subgraph-isomorphic to 𝐺~1~ if some
subgraph of 𝐺~2~ is isomorphic to 𝐺~1~

- = G~1~ 是 G~2~ 的subgraph
  - 利用先前的subgrapg的定義



![12-motifs_頁面_11](slide/12-motifs_%E9%A0%81%E9%9D%A2_11.png)

### -

all subgraphs up to a given size (# nodes)

隨著size的提升，subgraph的種類(non-iosmorphic)會超指數增加。

而在任務"在一個 graph中，找到 "常見" 的 subgraphs"中
此時若用傳統方法逐一計數會耗費大量算力。

## Network Motifs

- **network motifs**: 圖中的一個反覆出現且有顯著模式的連接。

  > “recurring, significant patterns of interconnections”

  - pattern: 指 (induced) subgraph
  - recurring: found many times → *frequency*
  - significant: more frequent than expected (ex: in randomly generated graphs) → *random graphs*

  後面這2好像是一個意思

![12-motifs_頁面_14](slide/12-motifs_%E9%A0%81%E9%9D%A2_14.png)

- 範例:
  ![12-motifs_頁面_15](slide/12-motifs_%E9%A0%81%E9%9D%A2_15.png)

- Why do we need motifs?

  ![12-motifs_頁面_16](slide/12-motifs_%E9%A0%81%E9%9D%A2_16.png)

### Subgraph frequency

#### Graph-level Frequency of G~Q~ in G~T~

G~Q~ : query graph  

G~T~ : target graph dataset

---

G~T~ 中存在多少與 G~Q~ 為 subgraph-isomorphic 的子圖

![12-motifs_頁面_17](slide/12-motifs_%E9%A0%81%E9%9D%A2_17.png)

#### Node-level Frequency of G~Q~ in G~T~

G~Q~ : query graph  

v : anchor (a node in G~Q~)

G~T~ : target graph dataset

---

計算 anchor node 可以映射到多少不同的nodes

![12-motifs_頁面_18](slide/12-motifs_%E9%A0%81%E9%9D%A2_18.png)

### Motif Significance

To define significance we need to have a null-model (i.e., point of comparison)
Key idea: Subgraphs (that occur in a real network much more often than in a random network) have functional significance

#### Random Graphs

1. ER random graphs

   - 2個參數 -  n: 節點數；p:邊的概率
   - method: 
     1. Create n nodes
     2. for each pair of nodes (u, v), flip a biased coin with bias p

   ![12-motifs_頁面_21](slide/12-motifs_%E9%A0%81%E9%9D%A2_21.png)

2. Configuration model

   - #nodes, #edges, degrees of the nodes

   - degree sequence k~1~, k~2~ ... k~N~ ，N = #nodes

     指定每個節點的度數但沒有指定如何連接

   - method:

     1. 把每個節點的degree視為nodes
     2. 隨機連接這些nodes
        - 忽略 self-loop, multiple spokes linked to each other

   - G^rand^可以用於比較擁有**相同degree sequence**的G^real^

   ![12-motifs_頁面_22](slide/12-motifs_%E9%A0%81%E9%9D%A2_22.png)

3. -

#### overview

![12-motifs_頁面_24](slide/12-motifs_%E9%A0%81%E9%9D%A2_24.png)

- Z-score
  - | z | \> 2 在統計上是顯著的 

↓ (normalize) 多個z score組成的vector

- Network significance profile (SP)

  ![12-motifs_頁面_25](slide/12-motifs_%E9%A0%81%E9%9D%A2_25.png)

### 尚有很多擴展

![12-motifs_頁面_29](slide/12-motifs_%E9%A0%81%E9%9D%A2_29.png)

# 12.2 Neural Subgraph Matching

利用 embedding space 的 geometric shape 來捕捉subgraph的關係

![12-motifs_頁面_33](slide/12-motifs_%E9%A0%81%E9%9D%A2_33.png)

![12-motifs_頁面_34](slide/12-motifs_%E9%A0%81%E9%9D%A2_34.png)

- Consider a binary prediction: 
  Return *True* if **query** is isomorphic to a subgraph of the **target graph**, else return *False*

  不找到實際的對應映射，只返回True or False

- Target graph → a set of neighborhoods
  - 用GNN產生每個neighborhood的embedding
  - 用GNN產生 Query 的embedding
  - 預測器根據這兩個embeddings 產生 True or False

## Neural Architecture for Subgraphs

- node-anchored neighborhoods

  ![12-motifs_頁面_38](slide/12-motifs_%E9%A0%81%E9%9D%A2_38.png)

## Decomposing G~T~ into Neighborhoods

1. 對於每個Node，獲取該節點周圍的k-hop neighborhood

   - 可以用breadth-first search(BFS)

   - k 通常用3~4

2. 將該node作為anchor ，計算每個anchor在其對應neighborhood中的node embedding

![12-motifs_頁面_40](slide/12-motifs_%E9%A0%81%E9%9D%A2_40.png)

## Order Embedding Space

Map graph 𝐴 to a point Z~A~ into a high-dimensional
(e.g. 64-dim) embedding space, such that Z~A~ is **non-negative in all dimensions** (>= 0)

- Transitivity
- Anti-symmetry
- Closure under intersection

### Order Constraint

We use a GNN to learn to embed neighborhoods and preserve the order embedding structure

- → design loss functions based on the order
  constraint

![12-motifs_頁面_46](slide/12-motifs_%E9%A0%81%E9%9D%A2_46.png)

- └ 如果G~Q~ 是 G~T~ 的subgraph，則G~Q~的嵌入座標的每個維度都應小於G~T~

![12-motifs_頁面_47](slide/12-motifs_%E9%A0%81%E9%9D%A2_47.png)

- max-margin loss
- └ Z~q~ - Z~t~ > 0 當Z~q~在Z~t~的"上"或"右"方

$\begin{align*}
    E(G_q, G_t) &= 
    \begin{cases}
        0, & \text{when } G_q \text{ is a subgraph of } G_t \\ 
        > 0, & \text{when } G_q \text{ is not a subgraph of } G_t
    \end{cases}
\end{align*}$

![12-motifs_頁面_49](slide/12-motifs_%E9%A0%81%E9%9D%A2_49.png)

![12-motifs_頁面_50](slide/12-motifs_%E9%A0%81%E9%9D%A2_50.png)

- 每次迭代使用不同的subgraph
- BFS - depth: 3~5

# 12.3 Finding Frequent Subgraphs

![12-motifs_頁面_56](slide/12-motifs_%E9%A0%81%E9%9D%A2_56.png)

任務:

1. 給定size的所有subgraph集合

   用搜索程序，因為只需要知道頻率最高的motifs

2. 計算集合中每個subgraph在G~T~中的frequency

   使用GNN預測而非計算



![12-motifs_頁面_59](slide/12-motifs_%E9%A0%81%E9%9D%A2_59.png)

## SPMiner

![12-motifs_頁面_60](slide/12-motifs_%E9%A0%81%E9%9D%A2_60.png)

![12-motifs_頁面_62](slide/12-motifs_%E9%A0%81%E9%9D%A2_62.png)

- super-graph region



![12-motifs_頁面_63](slide/12-motifs_%E9%A0%81%E9%9D%A2_63.png)

![12-motifs_頁面_64](slide/12-motifs_%E9%A0%81%E9%9D%A2_64.png)