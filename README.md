# 馬達相關論文研讀共同筆記

旨在提供一個論文研讀的共同筆記，方便團隊成員閱讀內容。

## 目錄

- [研讀論文列表](#研讀論文列表)
- [論文研讀紀錄](#論文研讀紀錄)
  - [論文一：One-Dimensional LSTM-Regulated Deep Residual Network for Data-Driven Fault Detection in Electric Machines](#one-dimensional-lstm-regulated-deep-residual-network-for-data-driven-fault-detection-in-electric-machines)
  - [論文二: Online Detection of Induction Motor’s Stator Winding Short-Circuit Faults](#online-detection-of-induction-motors-stator-winding-short-circuit-faults)
- [討論](#討論)
- [待解決問題](#待解決問題)
- [參考資料](#參考資料)


## 論文原文列表(原文摘要對應)

- [x] [論文一：One-Dimensional LSTM-Regulated Deep Residual Network for Data-Driven Fault Detection in Electric Machines](https://itrihq-my.sharepoint.com/:b:/g/personal/b20613_itri_org_tw/Eb78VWLnmlZAr9EDMNqEoSsBeo1dQ2IECybsa_72IKnWfw?e=RYWl6B) - Min-Fu Hsieh 2024
- [ ] 論文二：標題、作者、連結


## 論文研讀紀錄


### one-dimensional lstm regulated deep residual network for data driven fault detection in electric machines
    

#### 摘要

- 論文提出一種結合長短期記憶（LSTM）與深度殘差網絡的新型故障診斷模型，適用於電機故障檢測。此模型具備廣泛的故障類型適應性、高準確率的故障分類以及較快的收斂速度。
- 透過兩種數據集評估，
  - 測量三相電流的永磁同步電動機匝間短路故障
  - 凱斯西儲大學的軸承故障振動數據。
- 模型在測試中實現了100%的準確率，並展現出比其他網絡更快的訓練收斂性能。

#### I. 簡介

- 電機故障通常可分為三類：
  - 電氣故障（例如：匝間短路、相對地故障）
  - 機械故障（如軸承故障、偏心故障）
  - 磁性故障（如永磁同步電機的去磁）。
- 分析電機的任何類型故障時，會使用不同的測量或信號（例如：電流、振動、聲學）。
  - 基於模型的方法   (物理or數學模型)
  - 基於信號分析的方法  (提取特徵、分群、閥值)
    > 指定的指標本質上在不同的工作點下不起作用。
  - 基於數據驅動的方法  (AI)

- 傳統方法：
  - 分類器：如k-最近鄰、支持向量機、淺層神經網絡
  - 特徵工程需求：例如快速傅立葉變換、頻譜峭度分析

> 淺層分別是淺層神經網路，處理通常不夠複雜無法適應不同的情況。

- 現代深度學習方法：
  - 深度神經網絡：如卷積神經網絡、堆疊自編碼器
    > 針對非線性和複雜信號的適應性


- 深度殘差網絡（ResNet）的出現 (CNN變形)：
  - 限制：再探索新特徵的能力有限
  - 改進：深度殘差收縮網絡、多尺度核心基礎ResNet


- 針對不同情況的評估：
  - 不同故障和測量的通用性問題
  - 準確率與數據集依賴性

- 作者提出的方法：

  - 結構：深度殘差網絡結合卷積長短期記憶（LSTM）
  - 目標：提取時空信息、降低複雜度、增加通用性
  - 優點：快速收斂、小型訓練集


#### II. 一維 LSTM 調節的殘差網絡結構和定義

- 傳統殘差網絡(ResNet)的限制，重新探索新互補特徵的能力有限。
- 2D網路的某些特徵不必然適用於1D數據。

- 本研究開發的LSTM調節網路：
  - 針對1D數據進行改進。

![image](https://hackmd.io/_uploads/SJni4G5Oa.png)

- 網絡輸入：
  - 可以輸入多通道信號，如三相電流、不同位置的振動測量等。

- 網路結構：
  - 每層包含多個block。
  - 與傳統ResNet相比，使用了ReLU激活函數和長跳躍連接。

- 網路block結構：

![image](https://hackmd.io/_uploads/SkgsSzcd6.png)



應用單濾波器卷積操作和多濾波器卷積操作。
特徵圖與每個塊的隱藏狀態進行串聯。

- 特徵圖降採樣：

通過卷積操作降低計算時間，改善時間複雜度。

- 多濾波器卷積操作：
  - 針對一種資料類型(振動訊號)調參後，不適用其他資料(定子電流)
  - 三個具有不同核心大小、填充和膨脹的濾波器。
  - 減少濾波器參數的影響，特別是對於不同類型的數據。

- 卷積長短期記憶(ConvLSTM)：
  - 用於處理數據序列。
  - 提取特徵圖的時空信息，進而提取互補特徵。

- 長跳躍連接：
  - 減少了卷積跳躍連接的需要，降低了網絡複雜度。

- ReLU函數的減少使用：
  - 僅在Block的輸入使用一個ReLU函數，有助於加快網路收斂。
  
#### III. 故障檢測
- 使用的資料集：
  - 第一組：針對PMSM的三相電流中的ITSC故障的數據。
  - 第二組：CWRU軸承數據中心的實驗數據，測量電機不同軸承狀態下的振動。

- 故障和測量類型：
  - 包括機械故障和電氣故障，以及電流和振動兩種測量方式。

    - 網路輸入：
      - 電流作為三通道輸入，振動數據作為單通道輸入。
    
    - 資料類別：
      - 第一組數據有四個類別，第二組有12個類別。

    - 優化器和損失函數：
      - 使用隨機梯度下降優化器和類別交叉熵損失，學習率設定為0.001。

  - ITSC故障在PMSM中的影響：
    - ITSC故障是電機中常見的故障，可迅速導致其他故障。
    - 研究假設PMSM電機的A相匝間發生短路。

  - 故障模型：
    - 提出了受故障影響相位和健康相位的電壓方程。

  - 故障檢測實驗設置：
    - 展示了應用ITSC故障的實驗設置，包括電機驅動裝置和測量裝置。

  - 資料採集：
    - 採集了PMSM在健康狀態和不同級別ITSC故障下的三相電流數據。

  - 故障檢測方法的訓練和測試：
    - 對所提網絡進行了ITSC故障診斷測試。
    - 使用不同樣本數量進行訓練和測試。

  - 模型結構：
    - 提出的網路結構包括四層，每層具有不同數量的塊。

  - 軸承故障檢測：
    - 使用CWRU軸承故障數據集進行評估。
    - 考慮了不同類型的軸承故障，包括健康狀態、內圈故障、外圈故障和球故障。

  - 軸承故障的訓練和測試：
    - 對所提網絡進行了軸承故障診斷測試。
    - 使用不同樣本數量進行訓練和測試。

- 綜合評估：
  - 所提網絡在ITSC故障和軸承故障診斷中表現出色。
  - 與其他網絡相比，所提網絡具有更高的準確率和更廣泛的適用性。


#### IV. 結論

- 提出的LSTM調節網絡：
  - 用於電機故障診斷。

- 模型評估：
  - 通過兩種不同電機故障數據集（包含不同測量方式）進行檢驗。

- 總結結果和比較：
  - 提出的網絡在兩種不同故障和測量情況下能夠100%準確分類測試數據。這些數據包括PMSM的三相電流（針對ITSC故障）和感應電機的振動（針對軸承故障）。
  - 與其他類似網絡（包括1D-ResNet和DRSN-CW）相比，所提網絡收斂更快（計算時間與1D-ResNet幾乎相同，且少於DRSN-CW）。
  - LSTM調節網絡通過應用機制來提取互補特徵，從而在準確度和收斂性方面表現更佳。
  - 通過對ITSC故障數據集的1000個數據樣本的測試，證明了該方法即使在小型數據集上也能適當工作。

- 總體性能：
  - 所提網絡在電機故障檢測方面展現出良好的性能。
  - 在現實情況下，測量數據可能受到嚴重噪音的污染，且電機速度可能變化。因此，未來的工作將開發該網絡，以解決噪音問題，並能夠處理由於不同電機速度導致的不同長度數據。





### online detection of induction motors stator winding short circuit faults

#### 摘要

- 目的：開發一種新技術以檢測感應電動機定子繞組中的轉與轉間短路故障。
- 創新：模擬多於一相的轉與轉間短路及相對地面的故障。
- 技術：使用從三相電流模式中提取的特徵。
- 功能：識別故障相位和故障嚴重性，也能檢測到相對地面的故障。
- 優勢：只需常見驅動系統中的電流感應器，不需機器設計細節。

#### I. 簡介

- 背景：三相感應電動機在工業過程中極為重要，但可能會出現故障導致失效。
- 統計：定子故障佔感應電機故障的26%-36%。
- 重點：定子轉與轉間故障被認為是大多數電機繞組故障的起始階段，因此其檢測受到廣泛關注。
- 挑戰：早期檢測轉與轉間短路故障至關重要，以避免進一步損壞和降低修理成本及停機時間。
- 目的：早期檢測三相感應電動機定子繞組中的轉與轉間短路故障，技術能檢測不同類型的轉與轉間短路故障，並識別故障嚴重性。


#### II. 感應電動機定子故障建模

- 目的與需求: 為了設計和評估故障診斷策略，需要理解基本物理問題，以找出與故障相關的特徵。
- 模型建立: 三相電機的模型展示了故障效應的包含，但存在複雜性與可靠性之間的權衡。
- 簡化假設: 提出的模型基於不同的簡化假設，使模型靈活。
- 模型應用: 使用模型來發展並分析定子故障檢測和識別策略，模型展示了具有單相定子繞組故障的感應電機的瞬態模型。
- 故障影響: 模型考慮了定子繞組中轉與轉間短路故障的影響。


#### III. 定子故障檢測與識別的提議策略

- 故障特徵提取: 三相電流模式轉換為特徵空間，為機器故障診斷技術的重要部分。
- 故障檢測系統概覽: 使用三相定子電流測量數據作為輸入數據。
- 特徵提取流程: 從3D電流定位中計算故障特徵，然後將選定的特徵提供給故障決策算法。
- 故障決策算法: 識別特定故障相位/相位並估計故障嚴重性。
- 基於3D橢圓的特徵比較: 將從3D電流定位的橢圓圖案與健康電機的圓形圖案進行比較。
- 故障相位識別: 使用最適合的3D橢圓來計算主要和次要軸長度及其主要方向。
- 嚴重性因素: 通過考慮橢圓的主次軸長來定義故障嚴重性因素，提供自動斷開電機的功能以避免嚴重損壞。

- (左)正常與故障的 3D 電流軌跡比較。
- (右)(a) 故障相「a」、「b」和「c」的 3-D 電流軌跡。 (b) 60°空間兩相 3-D 電流軌跡之間的位移。
- 相接地故障情況下的模擬結果、兩項故障實驗設置、相對地故障狀況的嚴重性係數特徵。

|正常vs異常|(a)|(b)|
|:-:|:-:|:--:|
|![image](https://hackmd.io/_uploads/ByJYoxaOT.png)|![image](https://hackmd.io/_uploads/SkgcilaOp.png)|![image](https://hackmd.io/_uploads/rJDhix6_T.png)|
|![image](https://hackmd.io/_uploads/SkiMhlp_a.png)|![image](https://hackmd.io/_uploads/Bylshl6_6.png)|![image](https://hackmd.io/_uploads/Hk9hhead6.png)|




#### IV. 實驗結果

- 試驗設備描述：使用特別改裝的三相鼠籠式感應電動機，進行實驗。實驗設置包括伺服馬達作為負載，通過彈性聯軸器和扭矩計連接到感應電動機。
- 試驗描述：透過測量三相電流，並從三維電流定位中提取特徵，以驗證模擬結果。實驗中記錄了三相電流樣本，並在特徵提取前過濾高頻成分。為避免電機繞組永久損壞，所有實驗均在降低供電電壓（220伏）和短時間內進行。
- 健康與故障操作：比較健康和故障電機條件下的三維電流定位。短路故障會使三維電流定位從圓形變為橢圓形。
- 故障嚴重性分析：探討不同故障嚴重性和不同負載扭矩下的三維電流定位。實驗結果顯示三維電流定位依賴於負載和故障嚴重性。
- 兩個故障相位的實驗結果：對於不同故障嚴重性和不同負載扭矩下的兩個故障相位進行了測試。這些測試用於檢測故障相位的位置。
- 實驗結果與模擬結果的比較：實驗結果與模擬結果略有不同，這可能是由於電機不對稱性、電源不平衡和模擬簡化所致。
- 結論：提出的策略通過模擬和實驗結果在不同故障和負載條件下得到驗證和驗證。


