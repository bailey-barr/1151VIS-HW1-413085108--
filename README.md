# 1151VIS-HW1-413085108--
# 1151VIS-HW1: Static Visualization using D3.js

## 📊 作業主題
* **主題名稱**：台灣區域人口遷移與都市化分佈（Taiwan Regional Population Migration Sankey Diagram）
* **視覺化類型**：桑基圖（Sankey Diagram）
* **資料來源**：內政部戶政司人口統計與區域社會增加趨勢數據

---

## 💡 製作動機與社會意義
本專案利用 D3.js 呈現台灣近年經典的「核心城市外流、衛星與重劃區磁吸」現象：
1. **推力（Push Effect）**：以台北市為首的核心都會區因高房價與生活成本，呈現顯著的淨遷出。
2. **拉力（Pull Effect）**：桃園市、台中市以及受竹科紅利影響的新竹縣市，成為接收外溢人口的主要磁吸樞紐。
3. 桑基圖的帶狀寬度能直觀呈現多對多的動態人口流動量，幫助觀看者一眼掌握區域人口板塊的位移。

---

## 🛠️ 製作流程與操作步驟
1. **環境建立**：
   * 於 GitHub 建立公開（Public）專案，命名為 `1151VIS-HW1-學號-姓名`。
2. **程式開發**：
   * 使用 HTML5 結合 **D3.js (v7)** 與 **d3-sankey** 外掛模組。
   * 於前端定義資料結構（包含 `nodes` 節點與 `links` 帶狀流量）。
   * 利用 `d3.sankey()` 計算座標，並透過 SVG 渲染出具備互動提示（Tooltip）的靜態桑基圖。
3. **本地測試執行**：
   * 於終端機執行指令啟動本地伺服器：
     ```bash
     python3 -m http.server 8000
     ```
   * 開啟瀏覽器訪問 `http://localhost:8000` 確認圖表正常渲染。
4. **成果繳交**：
   * 將專案打包上傳至 GitHub[cite: 1]。
   * 將專案共用權限開給 `cchu.fju@gmail.com`[cite: 1]。
   * 錄製操作與展示畫面影片[cite: 1]。

---

## 📸 專案預覽
<img width="940" height="596" alt="image" src="https://github.com/user-attachments/assets/af976e9e-f6f2-4bd7-a175-196fa938294e" />

<img width="940" height="596" alt="image" src="https://github.com/user-attachments/assets/3ba7a603-b4bb-4c71-bec3-49bb88b0b262" />
