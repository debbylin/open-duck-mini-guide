# Open Duck Mini v2：繁體中文常見問題 FAQ

查核：2026-11-12（台北）。這是英文原始資料的中文摘要與排查整理，不是實機驗收，也沒有統計每個問題的發生率。

## 01 我買的 C001 是不是買錯？為何抬腳不夠高、跟不上步態？

**證據狀態：** 未解：有多位使用者回報，不能據此判定整批買錯

官方 #48 有 C001 使用者量到實際關節追蹤幅度不足。討論中也質疑 5.24 rad/s 是否是實機硬限制；截至查核日未見維護者對該串給出確定結論。

1. 保留你現有 15 顆 C001 的配置，記錄標籤、減速比、主／次韌體版本、供電及 Runtime 版本。
2. 先排除負載電壓下降、機構干涉、舵盤方向及偏移錯誤；再比對受支撐測試時的命令位置與實際位置。
3. 若仍無法跟上，帶紀錄回報原討論，不直接提高電壓、改控制增益或整批換成更快型號。

**限制：** 這不是已證明的 C001 不相容。維護者在較早的 Runtime #7 說明 5.24 來自系統辨識；不能只看該常數下購買結論。

來源：[原始資料 1](https://github.com/apirrone/Open_Duck_Mini/issues/48) · [原始資料 2](https://github.com/apirrone/Open_Duck_Mini_Runtime/issues/7#issuecomment-2796725889)

## 02 為何跑偏移校正時，其他關節也突然動了？

**證據狀態：** 已核對程式；另有未解損壞回報

find_soft_offsets.py 會先啟用機器人並把全部關節送往零位；逐顆提示不代表其他軸完全不會動。#32 有頭部扭壞回報，原因尚無確定解答。

1. 組裝前就逐顆設 ID、回零及裝舵盤；整機校正前先核對官方機械零位圖。
2. 先支撐機身、清空全身活動範圍並準備切電，再決定是否執行。
3. 一旦接近硬止擋或姿態不對就停止，斷電檢查方向、ID、舵盤與干涉。

**限制：** 放慢操作或只盯目前提示的關節，不能保證其他軸不動；不要把校正當作純讀取。

來源：[原始資料 1](https://github.com/apirrone/Open_Duck_Mini_Runtime/blob/v2/scripts/find_soft_offsets.py) · [原始資料 2](https://github.com/apirrone/Open_Duck_Mini_Runtime/issues/32) · [原始資料 3](https://github.com/apirrone/Open_Duck_Mini/issues/17#issuecomment-2782436850)

## 03 校正顯示 Done，重開機怎麼又歪了？

**證據狀態：** 已核對目前 v2 程式

查閱的 find_soft_offsets.py 最後要求把偏移值複製到 duck_config.json，程式沒有自動寫入該檔的步驟。

1. 保留終端機列出的各關節偏移值，手動填入實際使用的 duck_config.json 之 joints_offsets。
2. 先備份舊檔，再核對全部關節名稱與值，確認是有效 JSON。
3. 重新開啟設定檔確認儲存成功，並核對啟動流程讀的是同一份檔案。

**限制：** 別人的偏移數值不能拿來代替你的實測；不同分支的校正工具可能有不同儲存方式。

來源：[原始資料 1](https://github.com/apirrone/Open_Duck_Mini_Runtime/blob/v2/scripts/find_soft_offsets.py#L75-L76) · [原始資料 2](https://github.com/apirrone/Open_Duck_Mini_Runtime/blob/v2/README.md#make-your-duck_configjson)

## 04 check_motors.py 只是看 ID，可以直接跑嗎？

**證據狀態：** 已核對目前 v2 程式

它會寫入控制參數，之後提供逐顆移動測試。名稱中的 check 不代表完全唯讀。

1. 先讀取自己版本的腳本，支撐機身並確認活動空間。
2. 把「有回應」與「可安全運動」分開判斷；通訊通過不代表機械零位正確。
3. 沒有確認範圍前，不在移動提示時一路按 Enter。

**限制：** 低控制增益也不能讓超出機構可達範圍的目標變安全。

來源：[原始資料 1](https://github.com/apirrone/Open_Duck_Mini_Runtime/blob/v2/scripts/check_motors.py)

## 05 出現 Parsing error、set_kps() 或 rustypot 錯誤怎麼辦？

**證據狀態：** 社群成功個案；不是單一通用解法

Runtime #26 有使用者在全部舵機可被找到的情況下仍發生錯誤；他回報統一主／次韌體版本後排除問題。

1. 先記錄實際序列埠、14 個 ID、資料線、供電及 Python 版本。
2. 比對每顆舵機的主版本與次版本，不能因為同批買就假設完全相同。
3. 若確實版本不同，再依 Feetech 對應型號的官方工具與流程規劃更新，先記下設定並保持穩定供電。

**限制：** 不要把任意最新韌體刷進所有型號；也不要用忽略解析例外的方式繼續走路。

來源：[原始資料 1](https://github.com/apirrone/Open_Duck_Mini_Runtime/issues/26#issuecomment-4070004033) · [原始資料 2](https://github.com/apirrone/Open_Duck_Mini_Runtime/issues/2)

## 06 舵機板有亮燈，為何舵機找不到或完全不動？

**證據狀態：** 官方設定要求＋社群實作交叉核對

USB 指示燈只證明部分電路有電，不等於舵機動力電源正常；ID、資料埠與線材也可能不對。 AIFITLAB 的配置錯誤串中，回覆也指向未設 ID 或未供電。

1. 斷電後只留一顆舵機，依板子說明核對動力電源、極性與運作模式。
2. Pi 使用 USB 資料埠及 OTG／資料線，確認實際出現的 USB 序列裝置，不假設一定是 ttyACM0。
3. 確認新舵機 ID 與設定工具的目標；之後逐段接回線束。

**限制：** 已閱讀英文論壇原帖與回覆；其短答沒有完整診斷紀錄，僅作排查方向。USB 方案不應先靠停用藍牙來排查 UART。

來源：[原始資料 1](https://github.com/seanmayer/open-duck-mini-build/blob/main/docs/build-log.md) · [原始資料 2](https://github.com/apirrone/Open_Duck_Mini_Runtime/blob/v2/scripts/configure_motor.py) · [原始資料 3](https://aifitlab.com/community/forum/topic/186153/what-does-this-specific-servo-configuration-error-indicate)

## 07 斷電後 ID 變回去，或裝好卻控制錯關節？

**證據狀態：** 社群個案＋官方 ID 表

有組裝紀錄指出自寫工具只改了即時 ID，缺少持久寫入流程；另一個 Runtime 議題則是實體 ID 與程式關節表不同。

1. 設定時一次接一顆，貼上 ID 與關節名，重新斷電再讀回確認。
2. 本版使用右 10–14、左 20–24、頭頸 30–33；別照其他作者自訂的 S01–S14 當成官方 ID。
3. 不確定時先讀值辨認，不在整條匯流排上廣播改 ID。

**限制：** ID 標籤、電氣串接次序與程式關節順序是三件事。

來源：[原始資料 1](https://github.com/seanmayer/open-duck-mini-build/blob/main/docs/build-log.md) · [原始資料 2](https://github.com/apirrone/Open_Duck_Mini_Runtime/issues/2) · [原始資料 3](https://github.com/apirrone/Open_Duck_Mini/blob/v2/docs/configure_motors.md)

## 08 舵機回應正常，卻只動一點或一負載就掉電？

**證據狀態：** 社群個案；需在自己的機器量測

英文組裝紀錄曾把幾乎沒電造成的有限動作誤認為舵機或命令問題；另有電池盒接觸不良回報。

1. 先停止動作，檢查電池狀態，再量測動力支路與 UBEC 輸出是否在負載下掉壓。
2. 查電池盒接點、開關、接頭與線路固定，不能只讀空載電壓。
3. 核對電池實際外形與盒子是否匹配；BMS、降壓器與充電器的功能分別確認。

**限制：** 不把換鈕扣頭電池或換 BMS 當成所有人的通用答案，也不為避免跳保護而繞過 BMS。

來源：[原始資料 1](https://scalablehuman.com/2026/10/25/building-open-duck-mini-v2-part-2-assembly-servo-setup-and-soldering-lessons/) · [原始資料 2](https://github.com/apirrone/Open_Duck_Mini/issues/33#issuecomment-3519638128) · [原始資料 3](https://github.com/apirrone/Open_Duck_Mini/issues/51)

## 09 大腿／髖部會磨到螺絲頭，這是我裝錯嗎？

**證據狀態：** 維護者確認過的干涉案例

#17 回報 roll_motor_bottom 的螺絲頭會碰到 roll_to_pitch，維護者確認有此問題，討論預留避讓槽。

1. 先斷電，確認實際碰撞點、螺絲頭型與長度，對照目前 CAD 與列印檔。
2. 確定是相同問題後，再依 CAD 規劃避讓或更正五金；先試裝再受力測試。
3. 把此處加入髖部裝完後的檢查點。

**限制：** 原討論提到磨出避讓空間，不等於教學建議讓通電舵機硬磨出槽。

來源：[原始資料 1](https://github.com/apirrone/Open_Duck_Mini/issues/17#issuecomment-2781350051)

## 10 左右 roll_to_pitch 孔位對不上，能硬鎖嗎？

**證據狀態：** 歷史問題，維護者表示 CAD 已修正

#7 曾回報孔位不同心，維護者表示已修 CAD 並會更新 STL。這可能是檔案版本問題，不能直接怪列印公差。

1. 核對列印件來自哪個版本，左右件有沒有拿反。
2. 拿最新指定版本的 CAD／STL 比較孔位，必要時重印對應件。
3. 試放螺絲確認同軸，不用斜鎖或擴孔強迫裝上。

**限制：** 議題關閉不表示你手上的舊列印件已被修正。

來源：[原始資料 1](https://github.com/apirrone/Open_Duck_Mini/issues/7#issuecomment-2732413741)

## 11 軀幹到底要 M3×6 還是 M3×10？螺絲總數是多少？

**證據狀態：** 特定長度已修正；總數仍需核對

#3 的軀幹上下件長度錯誤已被維護者修正；現行指南為 2 顆 M3×10。完整五金資訊仍出現在待辦中。

1. 軀幹上下件依現行指南使用 2 顆 M3×10；腳底組合的 M3×6 不要混用。
2. 其他位置逐一依照片、螺紋深度與 CAD 核對，不把所有孔都套同一種長度。

**限制：** 整包採購數量不是設計需用數量；本教學不編造完整螺絲總數。

來源：[原始資料 1](https://github.com/apirrone/Open_Duck_Mini/issues/3) · [原始資料 2](https://github.com/apirrone/Open_Duck_Mini/issues/33) · [原始資料 3](https://github.com/apirrone/Open_Duck_Mini/blob/v2/docs/assembly_guide.md)

## 12 頭部裝好才發現身體上蓋放不進去？

**證據狀態：** 社群回報，官方指南有預先裝入提醒

#36 回報外殼與頭部機構的組裝順序造成重拆；官方在組頭部時提醒先放入頭底板與 body_middle_top。

1. 組頭部機構時先試放相關底板與身體上件。
2. 保留可接觸固定點，再接完頭頸；封殼前逐片檢查。

**限制：** 不是所有外殼都只是可最後套上的裝飾件。

來源：[原始資料 1](https://github.com/apirrone/Open_Duck_Mini/issues/36) · [原始資料 2](https://github.com/apirrone/Open_Duck_Mini/blob/v2/docs/assembly_guide.md#assemble-the-head-mechanism)

## 13 裝套件時遇到 onnxruntime 找不到相容版本？

**證據狀態：** 社群有成功回報，依版本選用

Runtime #38 的回覆把問題連到較新系統的 Python 版本；有人回到 Bookworm／Python 3.11 環境後可安裝。

1. 先記錄 OS、32／64 位元、Python 與套件要求。
2. 選與官方流程一致的 Pi Zero 2 W、64 位元環境；確認該分支所要求的套件是否有相容 wheel。
3. 若既有環境已混亂，另用一張卡或新的虛擬環境依支持組合重建，保留校正資料。

**限制：** 這是特定版本的成功個案；不把升級或降級某一個套件列為永久萬用指令。

來源：[原始資料 1](https://github.com/apirrone/Open_Duck_Mini_Runtime/issues/38#issuecomment-3666246783) · [原始資料 2](https://github.com/apirrone/Open_Duck_Mini_Runtime/blob/v2/README.md)

## 14 No module named mini_bdx_runtime / raw_imu 怎麼排查？

**證據狀態：** 環境與路徑問題，需依分支核對

官方安裝說明有虛擬環境與路徑提醒，#39 有使用者修正環境路徑後恢復，Runtime #41 則討論套件結構。

1. 先確認正在使用的 Python 與安裝 Runtime 的虛擬環境相同，並在對應專案目錄執行。
2. 檢查安裝分支與版本，依該版本 README 安裝專案；不要隨意更名套件資料夾。
3. 預製映像改了登入帳號者，要特別核對家目錄與 PYTHONPATH；先看該映像的官方修正段落。

**限制：** 不提供不明來源的一鍵修復腳本，也不把別人的絕對路徑直接貼進設定。

來源：[原始資料 1](https://github.com/apirrone/Open_Duck_Mini/issues/39#issuecomment-3518952970) · [原始資料 2](https://github.com/apirrone/Open_Duck_Mini_Runtime/issues/41) · [原始資料 3](https://github.com/apirrone/Open_Duck_Mini_Runtime/blob/v2/docs/INSTALL.md#fix-if-you-are-unable-to-manually-launch-the-head-puppet-or-walk-scripts-after-picking-a-custom-username-instead-of-bdxv2-during-flashing)

## 15 32GB 卡為何還是 No space left on device？

**證據狀態：** 官方建議＋映像說明

卡片標示容量不代表根檔案系統已展開。官方映像說明指出首次寫入後可能只有約 7GB 可用。

1. 先用 df -h 檢查實際使用量與掛載點，區分根分割區與暫存空間問題。
2. 依自己映像的官方方式擴展檔案系統，再重啟並檢查。
3. 確認使用官方建議的 Lite 64 位元系統，避免在低記憶體板上無限制重複建置大型套件。

**限制：** 先找容量原因，不任意刪除系統目錄；SD 卡容量大也不能保證安裝依賴相容。

來源：[原始資料 1](https://github.com/apirrone/Open_Duck_Mini_Runtime/issues/30#issuecomment-3472279581) · [原始資料 2](https://github.com/apirrone/Open_Duck_Mini_Runtime/blob/v2/docs/INSTALL.md#3-expand-the-filesystem-use-the-full-sd-card)

## 16 BNO055 沒資料、姿態反向，或 DigitalInOut 錯誤？

**證據狀態：** 多種原因，不能用一招處理

官方組裝圖有 IMU 反裝註記；Runtime #12 又包含 I2C 與 Python GPIO／依賴問題，兩者不能混為一談。

1. 先核對實際模組的 VIN／GND／SDA／SCL 標示、Pi 排針編號與 I2C 是否啟用；不同轉接板的排針順序可能不同。
2. 能讀資料後再慢慢傾斜機身，核對座標與 imu_upside_down 設定。
3. 若是匯入時出現 DigitalInOut，記錄板型、Python 與 Adafruit／GPIO 版本，依該平台支持方式處理。

**限制：** Pi 5 的 GPIO 處理不能直接照搬到你的 Pi Zero 2 W；把布林值改掉也不會自動校正所有安裝方向。

來源：[原始資料 1](https://github.com/apirrone/Open_Duck_Mini_Runtime/issues/12) · [原始資料 2](https://github.com/apirrone/Open_Duck_Mini/blob/v2/docs/assembly_guide.md#mount-the-imu)

## 17 手把連上又斷線，或開機後完全不動？

**證據狀態：** 官方前提＋社群特定環境回報

預製映像的腳底開機模式需要手把已配對且連線；#38 有特定藍牙環境調整後改善的個案。

1. 先確認手把型號、配對狀態與連線，再依自己映像的腳底啟動規則操作。
2. 區分「模式未啟動」、「程式報錯」與「舵機未供電」，查看對應服務狀態。
3. 記錄藍牙版本與錯誤，備份設定後再針對相同環境嘗試社群做法。

**限制：** 不預設替換成 PS5 手把就能沿用 Xbox 的按鍵映射，也不先停用藍牙解 USB 問題。

來源：[原始資料 1](https://github.com/apirrone/Open_Duck_Mini_Runtime/blob/v2/docs/INSTALL.md#5-foot-based-start-autostart-service) · [原始資料 2](https://github.com/apirrone/Open_Duck_Mini/issues/38)

## 18 Camera、眼睛燈、天線可以最後直接加上嗎？

**證據狀態：** 部分功能尚待整合；LED 有已知供電問題

官方設定預留相機等開關，但不代表攝影、辨識或避障已完成。#34 也指出高功率 LED 不適合直接讓 Pi GPIO 供電。

1. 先完成基本組裝、感測與步行校正，再分別核對擴充所需的固定件、電源與程式。
2. Camera 另核對 Pi Zero 的 22-pin 相機接頭及模組排線；先訂明只拍攝、串流還是另做辨識。
3. 高功率 LED 需適合的驅動與限流設計，GPIO 僅做控制，不能照簡圖直接承擔燈的電流。

**限制：** 不把討論中的某個電阻值或 MOSFET 型號當成所有 LED 都適用的成品電路。

來源：[原始資料 1](https://github.com/apirrone/Open_Duck_Mini/issues/34) · [原始資料 2](https://github.com/apirrone/Open_Duck_Mini_Runtime/blob/v2/example_config.json) · [原始資料 3](https://www.raspberrypi.com/documentation/accessories/camera-software.html)

## 19 上半身歪斜，是 IMU 壞了嗎？

**證據狀態：** 論壇排查方向，需結合官方零位圖確認

AIFITLAB 的上身傾斜討論有零位／偏移校正的回覆；單憑姿態不能判定 IMU 損壞。

1. 先看機械舵盤與左右件方向，再確認實際使用的關節偏移有儲存。
2. 對照官方零位圖，最後才檢查 IMU 安裝座標與資料；各項分開測。

**限制：** 論壇的 FD／find_offset 用語不是本教學的可直接執行指令；本版依官方 v2 的工具與風險說明操作。

來源：[原始資料 1](https://aifitlab.com/community/forum/topic/180522/why-is-my-duck-robots-upper-body-tilted-and-how-to-correct-it) · [原始資料 2](https://github.com/apirrone/Open_Duck_Mini/issues/17#issuecomment-2782436850)

## 資料範圍與判讀

- 官方文件／程式：核對腳本實際行為，並非只看函式或檔名。
- GitHub 問題串：有維護者確認、社群成功個案、未解三種狀態；關閉議題不直接視為你的問題已解。
- AIFITLAB 英文論壇：閱讀舵機配置錯誤與上身傾斜的原帖及短回覆；未有完整重現或追蹤，因此只作排查方向。
- 英文組裝紀錄：核對供電、ID 及機械止擋案例。作者自己的編號、電源改法與分支不直接搬入這份配置。
- 未登入或讀取私人 Discord 訊息；未把商店彙整、AI 摘要或其他 Microduck 型號的做法當作官方修正。
- 本 FAQ 的排查順序由本教學整理，與來源中的已證實解法分開標示；沒有在此實機上重現問題。
