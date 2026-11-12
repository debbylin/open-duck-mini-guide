# Open Duck Mini v2 中文組裝教學

這份教學搭配互動組裝圖與官方照片，從舵機編號、零位設定開始，依序組裝小鴨。

本版配置：**全新 Feetech STS3215 C001 ×15；14 顆安裝，1 顆備用**。不沿用 SO-101 拆機舵機的配置。

## 第一次使用

1. 直接開啟 [互動組裝圖](https://debbylin.github.io/open-duck-mini-guide/)，不需邀請或登入。
2. 先讀 [繁中實作 Step by Step](https://debbylin.github.io/open-duck-mini-guide/step-by-step.html) 與 [繁中 FAQ](https://debbylin.github.io/open-duck-mini-guide/faq.html)，尤其是 C001、零位及校正風險。
3. 若要離線使用，在專案頁選 **Code → Download ZIP**，解壓縮後雙擊 **index.html** 或 **step-by-step.html**。
4. 逐項完成 [零件核對清單](CHECKLIST.md)，實作手冊可在瀏覽器記錄完成進度。

不需要安裝程式，也不需要架設伺服器。組裝照片已包含在網頁裡，離線也能看；官方原文與 CAD 連結需要網路。GitHub 的檔案檢視頁只會顯示 HTML 原始碼，請下載後開啟。

## 圖怎麼操作

- 上方選 10 個組裝步驟，右側看對應說明及官方原圖。
- 點官方照片可放大；按 Esc 關閉。
- 下方點舵機 ID，圖中會標出對應的舵機。
- 可展開組件、隱藏外殼、切換深淺色，或看官方完成外觀。
- 接線表內可開啟官方完整配線圖。
- 圖取得焦點後，左右鍵切換步驟，Home / End 到第一步及最後一步，空白鍵切換展開。

## 先看實作與 FAQ

- [18 個實作步驟（Markdown 版）](STEP-BY-STEP.md)：每步包含準備、動作、完成檢查與停止條件。
- [19 題常見問題（Markdown 版）](FAQ.md)：英文 GitHub、論壇與實作紀錄的繁中整理，附證據狀態及來源。
- 本教學已更正：校正程式可能同時移動多個關節，偏移數值需確認手動儲存；C001 對既有步態的追蹤仍有未解討論。

## 建議組裝順序

**核對零件 → 舵機逐顆設 ID 及回零 → 軀幹 → 腳掌 → 小腿 → 大腿 → 髖部合體 → 頭頸 → 電子零件 → 校正測試 → 外殼。**

先讀過整個步驟再鎖緊。舵盤方向與需要先穿過的線材，尤其要先對照官方照片。

## 舵機 ID 表

左右以機器人自身的方向判定。

| 關節 | 右腿 ID | 左腿 ID |
|---|---:|---:|
| hip_yaw | 10 | 20 |
| hip_roll | 11 | 21 |
| hip_pitch | 12 | 22 |
| knee | 13 | 23 |
| ankle | 14 | 24 |

| 頭頸關節 | ID |
|---|---:|
| neck_pitch | 30 |
| head_pitch | 31 |
| head_yaw | 32 |
| head_roll | 33 |

一次只接一顆新舵機來設定 ID，避免預設 ID 重複。設定完成後貼上 ID 與關節名稱。第 15 顆留作備品，不加入工作的匯流排。

## Camera 呢？

目前這版基本組裝不包含相機。官方 v2 BOM 將相機放在表情擴充套件，並註記尚未整合；基本走路不依賴相機。

若之後要加相機，還需要核對模組、Pi Zero 專用排線、頭部固定空間，以及拍攝或串流軟體。加上相機不會自動具備辨識、追蹤或避障功能。

眼睛 LED、投射燈、音效、麥克風及天線舵機也屬於另外擴充的功能。

## 軟體與實機核對

圖形是簡化的子組件位置示意，**不是精確 CAD 或實際插入路徑**。固定孔、軸承、舵盤朝向、螺絲長度及線材餘量，應以實際列印件、官方照片及 Onshape CAD 為準。

組裝後還需要依官方 Runtime 完成系統安裝、IMU 測試、舵機檢查及關節偏移校正，才能測試走路。請先支撐好機器人，讓腳離地，以低速確認關節及線材，再進入步行測試。

- [官方組裝指南](https://github.com/apirrone/Open_Duck_Mini/blob/b23317a485b3cec7d8417f352478778b3475173c/docs/assembly_guide.md)
- [官方舵機設定](https://github.com/apirrone/Open_Duck_Mini/blob/b23317a485b3cec7d8417f352478778b3475173c/docs/configure_motors.md)
- [官方列印清單](https://github.com/apirrone/Open_Duck_Mini/blob/b23317a485b3cec7d8417f352478778b3475173c/docs/print_guide.md)
- [官方 Onshape CAD](https://cad.onshape.com/documents/64074dfcfa379b37d8a47762/w/3650ab4221e215a4f65eb7fe/e/0505c262d882183a25049d05)
- [官方 Runtime v2](https://github.com/apirrone/Open_Duck_Mini_Runtime/tree/v2)

## 來源與授權

本專案為非官方中文整理與簡化組裝示意，不代表 Open Duck Mini 官方背書。原始照片與資料的版本、授權及修改說明見 [ATTRIBUTION.md](ATTRIBUTION.md)。
