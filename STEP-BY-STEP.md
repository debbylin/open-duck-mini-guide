# Open Duck Mini v2：繁體中文實作步驟

配置：全新 STS3215 C001 ×15，14 顆安裝、1 顆備品。

先看 [C001 與校正的重要 FAQ](FAQ.md)。勾選完成代表你自行確認，不是系統驗收。

## 01 開工：分袋、清點與決定配置

**準備：** 15 顆 STS3215 C001、列印件、五金、標籤、筆、零件盒

1. 打開零件核對清單，照實物逐項勾選，不把買過或有線材當作已組齊。
2. 把右腿、左腿、頭頸及電子零件分袋；另外留 1 顆 C001 作備品。
3. 以機器人自己的左右標記所有零件；面對機器人時，左右與你相反。

**完成檢查：**

- [ ] 有 14 顆待裝舵機與 1 顆獨立備品。
- [ ] 驅動板、電池、UBEC 型號可辨識，沒有不明電源接法。

**遇到這些先停：** 缺料或型號不明先補確認；不要用外觀相似的板子直接替代。

[原始參考](CHECKLIST.md)

## 02 處理列印件與熱熔螺母

**準備：** 官方 v2 列印件、M3 熱熔螺母、烙鐵、螺絲、軸承

1. 依官方列印清單核對名稱、左右件及數量；腳底使用 TPU，其餘依官方 PLA 設定。
2. 先清除支撐與孔內殘料，試放舵機、軸承及螺母，確認沒有翹曲。
3. 依各步驟的官方照片，讓螺母垂直壓入指定孔，冷卻後再試鎖。不要把所有看似相同的孔都裝螺母。

**完成檢查：**

- [ ] 螺絲可平順旋入，螺母不跟著空轉。
- [ ] 軸承與舵機能正常就位，不需強壓變形。

**遇到這些先停：** 螺母歪斜、塑膠裂開或螺絲頂到底時先修正，不以加力鎖緊處理。

[原始參考](https://github.com/apirrone/Open_Duck_Mini/blob/v2/docs/print_guide.md)

## 03 先讓 Pi 開機，建立軟體工具

**準備：** Pi Zero 2 W、microSD、讀卡機、電腦、穩定 5V 供電

1. 選一條安裝路線：官方預製 Duck 映像，或官方手動安裝流程。不要混用兩套開機及設定方式。
2. 使用預製映像時，依官方 INSTALL.md 下載並用 Raspberry Pi Imager 寫入；官方測試目標是 32GB 卡。寫入會清除該卡原有內容。
3. 先以獨立穩定 5V 電源啟動 Pi，接好 Wi-Fi，確認能登入。使用預製映像時，依官方說明更改預設登入與熱點密碼。
4. 確認 Runtime v2 已安裝、可進入其 Python 環境，並找到 configure_motor.py。手動安裝者依官方 README 完成套件與 I2C 設定。

**完成檢查：**

- [ ] 可以登入 Pi 並找到 Runtime 的設定工具。
- [ ] 已記下自己的安裝目錄與登入方式，沒有把密碼放進公開專案。

**遇到這些先停：** 先讓 Pi 單獨穩定開機；此時不連 14 顆舵機，也不啟動步行模式。

[原始參考](https://github.com/apirrone/Open_Duck_Mini_Runtime/blob/v2/docs/INSTALL.md)

## 04 建立單顆舵機設定工作台

**準備：** 舵機板、USB OTG 與資料線、1 顆舵機、符合規格的舵機電源

1. 斷電時接線：Pi 的 USB 資料埠經 OTG 接舵機板，舵機板只接 1 顆舵機。
2. 依轉接板原廠說明接舵機電源，核對電壓、極性及接頭；USB 是否供應舵機動力不能靠猜測。
3. 把舵機放穩，讓輸出軸有活動空間，再開電源確認電腦／Pi 能辨識舵機板。

**完成檢查：**

- [ ] 單顆舵機可通訊，Pi 不重開機。
- [ ] 能分別關閉舵機與 Pi 的電源，換線前能確實斷電。

**遇到這些先停：** 無法通訊先查資料線、序列埠、電源與板型，不同時接入多顆新舵機試運氣。

[原始參考](https://github.com/apirrone/Open_Duck_Mini/blob/v2/docs/configure_motors.md)

## 05 逐顆設 ID、回零與貼標籤

**準備：** 14 顆待裝 C001、官方 ID 表、舵盤與標籤

1. 在 Runtime 的 configure_motor.py 所在目錄及已啟用的 Python 環境，執行設定指令；第一顆範例為 python configure_motor.py --id 10。
2. 程式讓舵機回零後，照官方零位照片裝舵盤；貼上 ID 與關節名稱。
3. 斷電，移除這顆，再接下一顆。右腿依序 10–14、左腿 20–24、頭頸 30–33。
4. 已設定的舵機放回對應分袋；備品暫不接入工作匯流排。
5. 每顆設定後重新斷電，再讀回 ID 確認保留；同時記錄主／次韌體版本。C001 的步態追蹤問題見 FAQ 01，組裝完成不等於模型已相容。

**完成檢查：**

- [ ] 14 顆都有唯一 ID，與互動圖下方的關節表一致。
- [ ] 每顆舵盤已依官方圖對齊；沒有憑印象混裝左右。

**遇到這些先停：** 通訊錯誤或輸出軸位置異常時先停止。不要以強扭通電舵機的方式對零。

[原始參考](https://github.com/apirrone/Open_Duck_Mini/blob/v2/docs/configure_motors.md)

## 06 組合軀幹骨架

**準備：** trunk_bottom、trunk_top、軸承、M3 螺母、2 顆 M3×10

1. 在互動圖選「軀幹骨架」，放大官方照片，確認軸承及螺母的安裝面。
2. 先裝 trunk_bottom 的軸承、指定螺母，以及底部預留外殼固定用的 4 個 M3 螺母。
3. 以 2 顆 M3×10 組合上下骨架；中間舵機及 roll_motor_bottom 依官方圖裝入，使用指定的舵機附螺絲。

**完成檢查：**

- [ ] 上下件貼合且不搖晃。
- [ ] 轉動軸承順暢，螺絲沒有突出到活動空間。

**遇到這些先停：** 軸承規格與螺絲長度以 CAD／實物核對；不要用示意圖的方塊大小當尺寸。

[原始參考](https://github.com/apirrone/Open_Duck_Mini/blob/v2/docs/assembly_guide.md#assemble-the-trunk)

## 07 完成兩隻腳掌

**準備：** 每腳的 TPU 底片、PLA 底座、foot_top / side；右 ID 14、左 ID 24；SS-10 ×4

1. 先做一隻，再用相同流程做另一隻：底片與底座裝指定螺母，每腳用 2 顆 M3×6 固定。
2. 在 foot_top 裝指定螺母。腳踝舵機帶驅動板的一側，朝官方圖指定的 foot_top 側。
3. 裝入每腳兩顆觸地開關；接線組合依官方腳掌接線圖，不任意把兩個訊號接到不同 GPIO。

**完成檢查：**

- [ ] 兩腳底面平整，腳踝關節沒有被夾緊。
- [ ] 每個開關能在腳掌觸地時動作、離地時釋放。

**遇到這些先停：** 先確認開關裝配及動作，再封閉腳掌；不要靠軟體掩蓋卡住的開關。

[原始參考](https://github.com/apirrone/Open_Duck_Mini/blob/v2/docs/assembly_guide.md#assemble-the-feet)

## 08 先穿線，再鎖小腿

**準備：** knee_to_ankle 左／右側板、leg_spacer；右 knee 13、左 23

1. 每個 leg_spacer 依官方照片在兩面合計裝 4 個 M3 螺母。
2. 先把三線舵機線插進腳踝舵機，再穿過 right_sheet 所需開口。
3. 對照照片裝兩側連桿與膝舵機；左右腿各做一組，最後才收緊固定點。

**完成檢查：**

- [ ] 線材確實通過指定開口，沒有被螺絲或兩片側板夾住。
- [ ] 斷電且不強扭舵機地檢查活動範圍，線材不會在關節處繃緊。

**遇到這些先停：** right_sheet 是零件名稱，不等於只裝右腿；走錯線先拆開重走。

[原始參考](https://github.com/apirrone/Open_Duck_Mini/blob/v2/docs/assembly_guide.md#assemble-the-shins)

## 09 裝大腿並核對 hip_pitch 朝向

**準備：** 大腿連桿、spacer；右 hip_pitch 12、左 22

1. 依小腿相近的側板結構組合大腿，但 hip_pitch 的舵機朝向要另看官方大腿照片。
2. 比較舵盤側、驅動板側與線材出口，不能只看零件是否鎖得上。
3. 先試合，再穿線、收緊。每完成一腿，就把 ID 標籤與關節表逐顆對一次。

**完成檢查：**

- [ ] 右腿與左腿使用正確 ID、正確鏡像零件。
- [ ] hip_pitch 的安裝朝向與官方圖一致。

**遇到這些先停：** 方向裝錯先修正機構，不能直接假設改軟體符號就能解決。

[原始參考](https://github.com/apirrone/Open_Duck_Mini/blob/v2/docs/assembly_guide.md#assemble-the-thighs)

## 10 髖部合體，接回軀幹

**準備：** left / right_roll_to_pitch、roll_motor_top / bottom；右 ID 10、11，左 20、21

1. 左右各用對應的 roll_to_pitch。先將 roll_motor_top 從底部固定到 hip_yaw 舵機。
2. 先不要把 hip_yaw 鎖進軀幹；依官方照片加入 hip_roll 與腿部子組件。
3. 完成能接觸到的固定點後，才把整組腿裝到軀幹；再做另一側。

**完成檢查：**

- [ ] 左右腿裝好，軀幹與腿部固定點完整。
- [ ] 舵機線接上後能沿結構通過，沒有經過會夾住的關節縫。
- [ ] roll_motor_bottom 螺絲頭與 roll_to_pitch 不互磨；詳見 FAQ 09。

**遇到這些先停：** 若螺絲被另一組件擋住，回到組裝順序確認，不用斜鎖代替。

[原始參考](https://github.com/apirrone/Open_Duck_Mini/blob/v2/docs/assembly_guide.md#assemble-the-hips)

## 11 安裝脖子兩側連桿

**準備：** neck_left_sheet、neck_right_sheet；neck_pitch 30、head_pitch 31

1. 辨認 ID 30 與 31，不把兩個俯仰關節互換。
2. 按官方照片鎖上兩側連桿，預留通往頭部的線束路徑。
3. 先確認頭部抬起、低下所需的線材鬆量，再固定線束。

**完成檢查：**

- [ ] 兩側連桿平行就位，沒有靠螺絲強拉到變形。
- [ ] 頭部線束有活動餘量，不拉扯接頭。

**遇到這些先停：** 本版兩顆都用新 C001；不要再照舊拆機舵機減速比分袋。

[原始參考](https://github.com/apirrone/Open_Duck_Mini/blob/v2/docs/assembly_guide.md#assemble-the-neck)

## 12 完成頭部 yaw / roll 機構

**準備：** head_pitch_to_yaw、head_yaw_to_roll、head_roll_mount；ID 32、33；頭底板

1. 先裝 head_pitch_to_yaw；另外組合 head_yaw_to_roll 與 head_roll_mount。
2. 依官方圖把子組件接起來，確認 yaw 是 ID 32，roll 是 ID 33。
3. 在封閉機構前，先放入頭底板與 body_middle_top，避免最後需要拆回。

**完成檢查：**

- [ ] 四個頭頸 ID 30–33 的位置清楚可查。
- [ ] 頭底板已預先就位，需要的線材都可通過。

**遇到這些先停：** 官方指南的 head_bot_plate 與列印清單 head_bot_sheet 名稱不同，請用 CAD 形狀核對。

[原始參考](https://github.com/apirrone/Open_Duck_Mini/blob/v2/docs/assembly_guide.md#assemble-the-head-mechanism)

## 13 分開完成電源與電子安裝

**準備：** Pi、BNO055、舵機板、UBEC、2S 電池盒／BMS、開關、電表

1. 先不要接 Pi。依你買的 BMS、開關及 UBEC 原廠接法完成電源，核對極性、接點絕緣與固定。
2. 量測 UBEC 輸出確為 5V，再接 Pi。2S 電池充滿可到 8.4V，不能直接送到 Pi 的 5V 輸入。
3. Pi 裝頭部，BNO055 與舵機板依支架固定；IMU 朝向同時核對官方文字，舊照片中有反裝情形。
4. USB OTG 接舵機板；BNO055 與腳底開關按互動圖的官方接線表及原圖接好。

**完成檢查：**

- [ ] Pi 在舵機供電時仍穩定，沒有重開機。
- [ ] 線束已絕緣及固定，接頭不承受拉力。
- [ ] 電源開關可立即切斷舵機動力。

**遇到這些先停：** 未確認板型或腳位時不做通電試錯；BMS 不是充電器，機內充電要另有相容方案。

[原始參考](https://github.com/apirrone/Open_Duck_Mini/blob/v2/docs/assembly_guide.md#electronics)

## 14 建立設定檔，測 IMU

**準備：** 已配線的小鴨、Pi 終端機、Runtime v2

1. 依所選映像說明，先讓自動步行保持停止；初次開機不要刻意壓住可能觸發模式的腳底開關。
2. 在 Runtime 目錄複製範例設定為自己家目錄的 duck_config.json。已存在就先備份，不覆蓋已做的校正。
3. 相機、音效、天線、投射燈等尚未裝的功能維持關閉。
4. 依官方 Test the IMU 流程讀取資料；緩慢傾斜機身，確認數值方向與安裝座標一致。

**完成檢查：**

- [ ] IMU 資料持續更新，沒有 I2C 錯誤。
- [ ] 安裝方向與 imu_upside_down 等設定一致；不是拿別人的偏移值直接套用。

**遇到這些先停：** 資料不動、方向錯或歸零後明顯飄移時，先修正感測器與設定，再測舵機。

[原始參考](https://github.com/apirrone/Open_Duck_Mini_Runtime/tree/v2#test-the-imu)

## 15 架空，核對全部 14 顆舵機

**準備：** 可穩固支撐機身的架子、電源開關、Runtime v2

1. 支撐機身並讓腳離地，手與工具離開關節；確保隨時可關閉電源。
2. 依官方 Test motors 流程，在 Runtime 根目錄執行 python3 scripts/check_motors.py。把它視為可能讓舵機動作的測試。
3. 逐顆比對通訊結果與 ID 表；同時查看線材是否在動作時拉緊、夾住或磨擦。

**完成檢查：**

- [ ] 10–14、20–24、30–33 全部可辨識，沒有重複或遺漏。
- [ ] 沒有異常頂死、快速發熱、連續重啟或接頭鬆脫。

**遇到這些先停：** 缺哪顆就斷電查那一段；不要先跑步行程式期待它自行恢復。

[原始參考](https://github.com/apirrone/Open_Duck_Mini_Runtime/tree/v2#test-motors)

## 16 做自己的關節偏移校正

**準備：** 架空的小鴨、自己的 duck_config.json

1. 先閱讀 FAQ 02 與自己版本的腳本。它一開始會把全部關節快速送往零位；只有確認整個機構可安全到達該姿態，才在支撐及可切電條件下執行 scripts/find_soft_offsets.py。
2. 按程式提示逐關節對齊，核對實際列印件、舵盤方向與左右鏡像。
3. 目前 v2 腳本不會自動寫檔。把顯示的偏移逐項填入自己 duck_config.json 的 joints_offsets，存檔並重新開啟確認，保留備份。

**完成檢查：**

- [ ] 14 個關節都有自己的偏移資料。
- [ ] 機械零位與軟體對應一致，沒有為了對姿態強扭通電舵機。

**遇到這些先停：** 若未確認整機零位範圍，先不要執行。程式初始化階段也可能同時動頭及腿；不能只監看提示中的關節。

[原始參考](https://github.com/apirrone/Open_Duck_Mini_Runtime/tree/v2#find-the-joints-offsets)

## 17 固定外殼並再查一次活動空間

**準備：** 頭殼、body_middle_bottom / top、body_front / back、電池盒與蓋

1. 斷電後先固定 body_middle_bottom，再照官方次序裝 body_middle_top、body_front 與電池盒。
2. 鎖頭殼前確認 Pi、頭部底板、軸承及線材都已定位。
3. 每加一片外殼就檢查一次接頭與線束，不把多餘線硬壓進關節。

**完成檢查：**

- [ ] 外殼固定、電池不晃動，開關仍容易操作。
- [ ] 可動件與外殼無明顯擦碰；插頭沒有被蓋子推歪。

**遇到這些先停：** 裝殼後才出現卡住或電源異常，先退回剛新增的那片外殼查原因。

[原始參考](https://github.com/apirrone/Open_Duck_Mini/blob/v2/docs/assembly_guide.md#body)

## 18 最後才做首次站立／走路測試

**準備：** 完成校正的小鴨、相容控制器、平坦不滑地面、可操作的電源開關

1. 先確認 FAQ 01 的 C001 未解議題及這批實測；本教學不保證既有步態模型在這批硬體上穩定運作。
2. 先依所選 Runtime 的官方流程配對手把，確認暫停與停止方式；預製映像的自動啟動流程需已配對且連線的 Xbox 手把。
3. 確認模型檔、設定檔及 Runtime 版本相符，使用官方 Run the walk 流程；不要把任意模型放進資料夾就當相容。
4. 先以支撐或保護方式觀察站姿與低幅度動作，確認穩定後再在平面做短距離測試。
5. 每次只調整一個問題並記錄結果；不從加速、衝刺或實驗性頭控開始。

**完成檢查：**

- [ ] 能暫停／停止，失衡時能立即切電。
- [ ] 左右腿動作方向一致且無夾線，供電沒有因負載突然中斷。

**遇到這些先停：** 倒下、抖動、頂死或重啟，先停止並檢查電源、IMU、ID 及偏移；相機不是解決走路不穩的必要補件。

[原始參考](https://github.com/apirrone/Open_Duck_Mini_Runtime/tree/v2#run-the-walk-)

