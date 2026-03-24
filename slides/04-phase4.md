<h3 style="color: #e74c3c;">第四階段：1–6 小時</h3>

## 有些東西回來了，有些⋯⋯再也回不來

Note:
進入第四階段。海纜斷裂已經超過一小時了。
BGP 早就重新收斂完成，壅塞也穩定下來，各種快取和 token 過期的「漸進式崩壞」也差不多走完了。
但聽眾會發現一個新的現象：有些東西開始恢復了，但另一些東西卻完全死掉。
這個階段要解釋兩件事：(1) ISP 開始手動做流量管理，(2) 雲端的「控制平面」依賴海外。
這兩個機制共同造成了一條新的分水嶺：純國內的服務活了，有海外大腦的服務死了。

---

### 你感受到的

- LINE 文字訊息——又通了！ ✅ <!-- .element: class="fragment" -->
- 一些之前看過的網頁——可以開 <!-- .element: class="fragment" -->
- YouTube——能看，但畫質剩 144p 馬賽克 <!-- .element: class="fragment" -->
- Instagram——文字有，圖片全是灰框 🖼️❌ <!-- .element: class="fragment" -->
- 要登入任何 SaaS 工具——轉圈圈、失敗 <!-- .element: class="fragment" -->
- AWS / GCP 管理後台——完全打不開 <!-- .element: class="fragment" -->

<p class="fragment fade-up" style="font-size: 1.3em; margin-top: 1em;">
  出現了一條<strong style="color: #f39c12;">新的分水嶺</strong><br>
  <span style="font-size: 0.9em; color: #aaa;">能用 vs 不能用，取決於服務的「大腦」在哪裡</span>
</p>

Note:
這個階段的關鍵感受是「不公平」：為什麼有些東西恢復了，有些反而更慘？
LINE 文字恢復是因為 ISP 開始做流量管理，把訊息列為高優先。
SaaS 和雲端後台完全掛掉，是因為它們的「控制平面」在海外。
接下來我們分兩部分解釋：(1) ISP 在做什麼，(2) 雲端的「大腦在海外」問題。

---

### ISP 在幕後做了什麼？

<p style="font-size: 1.2em; margin-top: 0.5em;">
  先忘掉網路——<br>
  我們去<strong style="color: #e74c3c;">醫院急診室</strong>
</p>

<div class="fragment" style="margin-top: 1em;">
  <p style="font-size: 5em; margin: 0;">🏥</p>
</div>

<p class="fragment fade-up" style="font-size: 1.1em; margin-top: 0.5em;">
  想像一場大地震後，<strong>大量傷患湧入急診室</strong><br>
  醫生人力有限——不可能同時救所有人<br>
  <span style="color: #aaa;">所以急診室有一套制度：<strong style="color: #e74c3c;">檢傷分類</strong></span>
</p>

Note:
這裡用急診室的比喻來解釋 ISP 的流量工程（traffic engineering）。
海纜斷裂後，國際頻寬只剩一半，但流量需求沒有減少——就像大量傷患湧入但醫生不夠。
ISP 的網路工程師這時候必須手動介入，決定哪些流量優先通過。
這就是網路世界的「檢傷分類」（triage）。

---

### 檢傷分類：誰先救？

<div style="display: flex; justify-content: center; gap: 1em; flex-wrap: wrap; margin-top: 0.8em;">
  <div class="fragment" style="background: rgba(231,76,60,0.15); padding: 0.8em; border-radius: 8px; flex: 1; min-width: 180px; max-width: 250px; border-left: 4px solid #e74c3c;">
    <p style="font-size: 1.1em; color: #e74c3c;">🔴 最優先</p>
    <p style="color: #aaa; font-size: 0.85em;">DNS 查詢<br>政府網站<br>即時訊息（LINE 文字）<br>金融交易</p>
  </div>
  <div class="fragment" style="background: rgba(243,156,18,0.15); padding: 0.8em; border-radius: 8px; flex: 1; min-width: 180px; max-width: 250px; border-left: 4px solid #f39c12;">
    <p style="font-size: 1.1em; color: #f39c12;">🟡 次優先</p>
    <p style="color: #aaa; font-size: 0.85em;">一般網頁瀏覽<br>Email 收發<br>低解析度串流</p>
  </div>
  <div class="fragment" style="background: rgba(46,204,113,0.15); padding: 0.8em; border-radius: 8px; flex: 1; min-width: 180px; max-width: 250px; border-left: 4px solid #2ecc71;">
    <p style="font-size: 1.1em; color: #2ecc71;">🟢 可延後</p>
    <p style="color: #aaa; font-size: 0.85em;">YouTube 高畫質<br>Instagram 圖片/影片<br>軟體更新下載<br>雲端備份</p>
  </div>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.1em;">
  ISP 工程師<strong>手動介入</strong>，決定誰的封包優先通過<br>
  <span style="color: #aaa;">這就是為什麼有些東西「恢復了」、有些變更慢</span>
</p>

Note:
ISP 的流量管理（traffic engineering）在正常情況下大多是自動化的。
但在海纜事件中，工程師會手動介入，設定 QoS（Quality of Service）規則。
DNS 和政府網站被列為最高優先，因為 DNS 是所有網路服務的基礎。
即時訊息（LINE 文字）流量小但對民眾影響大，所以也被優先處理。
YouTube、Instagram 的圖片和影片流量極大（佔總流量的高比例），被降級處理。
這就是為什麼你會看到 YouTube 畫質驟降、Instagram 只有文字沒有圖片。

---

### 為什麼 LINE 文字恢復了？

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.1em;">1️⃣ LINE 文字訊息 = <strong>超小封包</strong></p>
  <p style="color: #aaa;">一則文字訊息大約 1 KB</p>
  <p style="color: #aaa;">一張 Instagram 照片大約 2,000 KB</p>
  <p style="color: #aaa;">差 <strong style="color: #3498db;">2000 倍</strong></p>
</div>

<div class="fragment" style="margin-top: 1em;">
  <p style="font-size: 1.1em;">2️⃣ LINE 在台灣有<strong>本地對等連線（TPIX）</strong></p>
  <p style="color: #aaa;">文字訊息走的是島內的捷徑，不需要出海</p>
</div>

<div class="fragment" style="margin-top: 1em;">
  <p style="font-size: 1.1em;">3️⃣ ISP 把訊息列為<strong style="color: #e74c3c;">高優先</strong></p>
  <p style="color: #aaa;">小封包 + 本地路由 + 高優先 = 擠得過去 ✅</p>
</div>

<p class="fragment fade-up" style="margin-top: 0.8em; font-size: 1.1em; color: #f39c12;">
  Instagram 圖片？大封包 + 海外來源 + 被降級 = 灰框 🖼️
</p>

Note:
LINE 文字訊息能恢復有三個原因疊加：
(1) 封包極小——文字訊息的資料量微不足道，就算頻寬極度壅塞也能擠過去。
(2) LINE 在 TPIX（台灣網際網路交換中心）有對等連線——代表很多 LINE 文字訊息其實走島內路由，根本不需要經過海纜。
(3) ISP 的流量管理把即時訊息列為高優先——在檢傷分類中排在前面。
三個因素加起來，LINE 文字訊息就恢復了。
相比之下，Instagram 照片動輒數 MB，來源在海外，又被降級——所以只剩灰框。

---

### 你的網路不是壞了——是被「管」了

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.2em;">🚦 ISP 工程師 = <strong>十字路口的交通警察</strong></p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p>正常時候：綠燈，所有車都能過</p>
  <p style="color: #aaa;">你感覺不到交通警察的存在</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p>危機時候：警察出來指揮 🖐️</p>
  <p style="color: #aaa;">「救護車先走！公車可以過！私家車等一下！」</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.2em;">
  你的 YouTube 不是「斷了」<br>
  是被<strong style="color: #f39c12;">讓道</strong>給更重要的東西了
</p>

Note:
這張投影片的重點是讓聽眾理解：此刻他們感受到的「部分恢復」不是隨機的，
而是 ISP 工程師刻意決策的結果。
ISP 有能力區分不同類型的流量，也有能力決定優先順序。
平常你不會注意到，因為頻寬足夠、所有人都能通過。
但在頻寬緊縮的時刻，ISP 的「選擇」就直接決定了你能用什麼、不能用什麼。

---

### 這代表什麼？一件你該知道的事

<div class="fragment" style="background: rgba(52,152,219,0.1); padding: 1em; border-radius: 8px; margin-top: 0.8em; max-width: 85%; margin-left: auto; margin-right: auto; text-align: left;">
  <p style="font-size: 1.1em;">ISP <strong>有能力</strong>做流量分類和管理</p>
  <p style="color: #aaa;">他們知道哪些封包去哪裡、是什麼類型</p>
</div>

<div class="fragment" style="background: rgba(243,156,18,0.1); padding: 1em; border-radius: 8px; margin-top: 0.8em; max-width: 85%; margin-left: auto; margin-right: auto; text-align: left;">
  <p style="font-size: 1.1em;">這代表 ISP <strong>平時的路由決策</strong>也是「選擇」</p>
  <p style="color: #aaa;">把你的流量繞去東京再繞回來（長號效應）——不是「只能這樣」</p>
  <p style="color: #aaa;">不在 TPIX 做本地對等連線——不是「做不到」</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.3em; color: #e74c3c;">
  <strong>危機時 ISP 能選擇救誰<br>
  代表平時 ISP 也在選擇——犧牲誰</strong>
</p>

<p class="fragment" style="color: #aaa; font-size: 0.9em; margin-top: 0.5em;">
  還記得 Phase 2 的長號效應嗎？那也是 ISP 的「選擇」
</p>

Note:
這是 ISP 流量管理段落的核心洞見。
聽眾剛看到 ISP 在危機中有能力做流量分類和優先排序——這代表 ISP 平時也有這個能力。
Phase 2 講到的長號效應——ISP 把本地流量繞去東京再繞回來——不是技術限制，是成本考量下的選擇。
不在 TPIX 做本地對等連線——也是選擇。
這個段落把 Phase 2 的批判和 Phase 4 的觀察連結起來：
ISP 不是被動的管道，是有能力、有選擇、該被追究的行動者。

---

### 接下來：雲端的問題

<p style="font-size: 1.1em; margin-top: 0.5em; color: #aaa;">
  LINE 文字恢復了、一些網頁能看了——<br>
  但 SaaS 工具和雲端服務<strong style="color: #e74c3c;">完全死掉</strong>
</p>

<p class="fragment" style="font-size: 1.2em; margin-top: 1em;">
  要理解為什麼，我們需要先搞懂一件事：
</p>

<p class="fragment fade-up" style="font-size: 1.5em; margin-top: 1em;">
  <strong style="color: #3498db;">「雲端」到底是什麼？</strong>
</p>

<div class="fragment" style="margin-top: 1em;">
  <p style="font-size: 5em; margin: 0;">☁️🤔</p>
</div>

Note:
轉場到雲端控制平面的部分。
很多聽眾對「雲端」的理解停留在很模糊的層次——「資料存在雲上」。
我們需要先把「雲端」講清楚，才能解釋為什麼「台灣的雲端」不等於安全。
接下來五張投影片要用最白話的方式，讓完全沒有技術背景的聽眾理解雲端和控制平面的概念。

---

### 「雲端」其實是⋯⋯別人的電腦

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.2em;">你聽過「資料存在雲端」☁️</p>
  <p style="color: #aaa;">聽起來很輕盈、很抽象、飄在天上</p>
</div>

<div class="fragment" style="margin-top: 1em;">
  <p style="font-size: 1.2em;">真相：</p>
  <p style="font-size: 1.3em; color: #3498db;"><strong>你的資料存在別人的電腦裡</strong></p>
  <p style="color: #aaa;">那台電腦放在一棟巨大的建築物裡，有空調、有保全、有備用發電機</p>
  <p style="color: #aaa;">這棟建築叫做<strong>「資料中心」（Data Center）</strong></p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.1em;">
  好消息：台灣有這樣的建築物 🏢<br>
  <span style="color: #2ecc71;">AWS 在板橋、GCP 在彰化——都有台灣機房</span><br>
  <span style="color: #aaa; font-size: 0.9em;">你的資料<strong>確實</strong>存在台灣的土地上 ✓</span>
</p>

Note:
先破除「雲端」的抽象感。
很多人聽到「雲端」就覺得資料飄在某個虛無的空間裡。
但雲端就是別人的電腦——放在大型資料中心裡。
AWS 在 2022 年啟用了台灣區域（ap-northeast-3 → 實際是板橋的資料中心）。
GCP 在彰化也有資料中心。
所以「資料在台灣」這句話在物理上是成立的——但接下來要解釋為什麼這還不夠。

---

### 工廠和總部

<p style="font-size: 1.1em; margin-top: 0.5em;">
  想像一家<strong style="color: #3498db;">跨國企業</strong>在台灣設了一座<strong>工廠</strong>
</p>

<div style="display: flex; justify-content: center; gap: 2em; flex-wrap: wrap; margin-top: 1em;">
  <div class="fragment" style="background: rgba(46,204,113,0.1); padding: 1em; border-radius: 8px; flex: 1; min-width: 240px; max-width: 320px;">
    <p style="font-size: 1.1em;">🏭 台灣工廠</p>
    <p style="color: #aaa; font-size: 0.9em;">生產產品（存你的檔案）</p>
    <p style="color: #aaa; font-size: 0.9em;">出貨給客戶（回應你的請求）</p>
    <p style="color: #aaa; font-size: 0.9em;">倉庫裡有原料（你的資料）</p>
    <p style="color: #2ecc71; font-size: 0.85em;">→ 這叫「資料平面」<br>Data Plane</p>
  </div>
  <div class="fragment" style="background: rgba(231,76,60,0.1); padding: 1em; border-radius: 8px; flex: 1; min-width: 240px; max-width: 320px;">
    <p style="font-size: 1.1em;">🏢 美國總部</p>
    <p style="color: #aaa; font-size: 0.9em;">核發員工證（身分驗證 IAM）</p>
    <p style="color: #aaa; font-size: 0.9em;">核准預算（配置資源）</p>
    <p style="color: #aaa; font-size: 0.9em;">簽發合約（SSL 憑證）</p>
    <p style="color: #e74c3c; font-size: 0.85em;">→ 這叫「控制平面」<br>Control Plane</p>
  </div>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.1em; color: #f39c12;">
  工廠在台灣 ✓　但做任何重要決定——都要<strong>打電話回總部</strong>
</p>

Note:
這是理解雲端控制平面的關鍵比喻。
雲端服務分成兩層：
(1) 資料平面（Data Plane）：實際存儲和處理資料的地方——這在台灣。
(2) 控制平面（Control Plane）：管理、認證、授權、配置的地方——這通常在美國。
AWS 的控制平面很多核心功能集中在 us-east-1（維吉尼亞）。
GCP 的全球控制平面也有類似的集中化設計。
工廠的比喻讓聽眾直覺理解：工廠能生產，但沒有總部的授權，工廠不能開門、不能出貨、不能做任何事。

---

### 工廠在台灣，但鑰匙在美國

<div class="fragment" style="margin-top: 0.5em;">
  <p style="font-size: 1.1em;">🔑 <strong>員工要進工廠</strong>（你要登入 AWS）</p>
  <p style="color: #aaa;">→ 要跟美國總部確認身分（IAM 驗證）</p>
  <p style="color: #aaa;">→ 請求走海纜到維吉尼亞 → <span style="color: #e74c3c;">逾時</span></p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.1em;">📋 <strong>工廠要出貨</strong>（網站要更新安全憑證）</p>
  <p style="color: #aaa;">→ 要跟美國總部簽發合約（SSL 憑證驗證）</p>
  <p style="color: #aaa;">→ 請求走海纜 → <span style="color: #e74c3c;">逾時</span> → HTTPS 連線失敗</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.1em;">📞 <strong>客戶要查工廠地址</strong>（DNS 解析）</p>
  <p style="color: #aaa;">→ 地址簿在美國（Route 53 在 us-east-1）</p>
  <p style="color: #aaa;">→ 查詢走海纜 → <span style="color: #e74c3c;">逾時</span> → 找不到工廠</p>
</div>

<p class="fragment fade-up" style="margin-top: 0.8em; font-size: 1.2em; color: #e74c3c;">
  工廠完好、原料充足、機器正常<br>
  但就是<strong>開不了門</strong>
</p>

Note:
三個具體場景說明控制平面依賴的影響：
(1) IAM（Identity and Access Management）——AWS 的身分驗證系統。登入 AWS Console 或 API 呼叫都需要 IAM 驗證。IAM 的核心服務在 us-east-1。海纜壅塞時，驗證請求逾時，你就登不進去。
(2) SSL/TLS 憑證——HTTPS 連線需要有效的安全憑證。憑證的驗證和更新需要連到海外的 CA（Certificate Authority）或 AWS Certificate Manager（也在 us-east-1）。憑證過期無法更新 → HTTPS 連線就建不起來。
(3) Route 53——AWS 的 DNS 服務。如果你的網站用 Route 53 做 DNS，你的「地址簿」就在美國。DNS 查詢逾時 → 找不到你的網站。
三個場景的共同結論：你的資料和伺服器都在台灣，但「打開門的鑰匙」在美國。

---

### 你的資料就在這裡——但你沒有「授權」打開它

<div style="background: rgba(231,76,60,0.1); padding: 1em; border-radius: 8px; margin-top: 0.5em; max-width: 85%; margin-left: auto; margin-right: auto;">
  <p style="font-size: 1.3em;">🔐</p>
  <p style="font-size: 1.1em;">你的 Google Drive 檔案<br>物理上存在彰化的 GCP 機房</p>
  <p style="font-size: 1.1em; margin-top: 0.5em;">你的公司 AWS 伺服器<br>物理上在板橋的資料中心</p>
  <p style="font-size: 1.2em; margin-top: 0.8em; color: #e74c3c;">
    <strong>但你就是打不開</strong>
  </p>
  <p style="color: #aaa; font-size: 0.9em;">因為「你有權打開它」這句話<br>需要美國的伺服器來確認</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.2em;">
  這就像把<strong style="color: #f39c12;">保險箱放在家裡</strong><br>
  但把鑰匙寄放在<strong style="color: #f39c12;">國外的銀行</strong> 🏦
</p>

<p class="fragment" style="color: #aaa; font-size: 0.9em; margin-top: 0.5em;">
  銀行正常營業——但你打不了國際電話了
</p>

Note:
這張投影片用最直白的方式總結控制平面問題。
保險箱/鑰匙的比喻非常直覺：你不會覺得「保險箱在家就安全了」，如果鑰匙在國外。
但這正是目前大多數台灣企業的雲端架構現狀——資料在台灣，但授權機制在海外。
2021 年 12 月 AWS us-east-1 大當機就是前例：
其他物理上完全正常的 AWS 區域也受到影響，因為 IAM、Route 53 等控制平面服務集中在 us-east-1。
那次不是海纜問題，是 us-east-1 自己出問題——但效果一模一樣：
你的區域正常，但控制平面掛了，所以你也跟著掛。

---

### 「雲端在台灣」≠ 安全

<div style="display: flex; justify-content: center; gap: 1.5em; flex-wrap: wrap; margin-top: 0.8em;">
  <div class="fragment" style="flex: 1; min-width: 140px; max-width: 200px; text-align: center;">
    <p style="font-size: 2.5em; margin: 0;">🏭</p>
    <p style="font-size: 0.9em;">工廠在台灣</p>
    <p style="color: #2ecc71;">✓</p>
  </div>
  <div class="fragment" style="flex: 1; min-width: 140px; max-width: 200px; text-align: center;">
    <p style="font-size: 2.5em; margin: 0;">🔑</p>
    <p style="font-size: 0.9em;">鑰匙在美國</p>
    <p style="color: #e74c3c;">✗</p>
  </div>
  <div class="fragment" style="flex: 1; min-width: 140px; max-width: 200px; text-align: center;">
    <p style="font-size: 2.5em; margin: 0;">📞</p>
    <p style="font-size: 0.9em;">電話線塞爆了</p>
    <p style="color: #e74c3c;">✗</p>
  </div>
</div>

<div class="fragment" style="margin-top: 1.2em;">
  <p style="font-size: 1.3em;">所以⋯⋯</p>
</div>

<p class="fragment fade-up" style="font-size: 1.4em; margin-top: 0.5em; color: #f39c12;">
  <strong>「我們用台灣的 AWS/GCP」<br>
  ≠<br>
  「我們的服務在海纜斷裂時還能用」</strong>
</p>

Note:
這是雲端控制平面段落的核心結論。
很多企業和政府機構在回答「你的服務有韌性嗎？」的時候，
會說「我們用的是台灣的 AWS 區域」或「我們的 GCP 機房在彰化」。
但這只代表「工廠在台灣」——不代表「鑰匙也在台灣」。
如果控制平面依賴海外，那海纜一斷，你的服務就跟著掛。
「台灣雲端」給人一種虛假的安全感——這是最需要被點破的認知誤區。

---

### 贏家：真正的純國內服務

<p style="font-size: 1.1em; margin-top: 0.5em; color: #aaa;">
  在這場模擬中，有些服務完全不受影響——
</p>

<div class="fragment" style="background: rgba(46,204,113,0.12); padding: 1em; border-radius: 8px; margin-top: 0.8em; max-width: 85%; margin-left: auto; margin-right: auto; text-align: left; border: 1px solid rgba(46,204,113,0.3);">
  <p style="font-size: 1.1em; color: #2ecc71;"><strong>✅ 活下來的服務長這樣：</strong></p>
  <p>伺服器在台灣 <!-- .element: class="fragment" --></p>
  <p>認證（Auth）在台灣 <!-- .element: class="fragment" --></p>
  <p>DNS 權威伺服器在台灣 <!-- .element: class="fragment" --></p>
  <p>CDN 來源站在台灣 <!-- .element: class="fragment" --></p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.2em; color: #2ecc71;">
  <strong>整條鏈都在島內 → 海纜斷不斷，跟它無關</strong>
</p>

Note:
這裡要強調的是：要在海纜事件中存活，不是只要「伺服器在台灣」就夠了。
你的整條依賴鏈——伺服器、認證、DNS、CDN 來源——都必須在台灣。
任何一個環節依賴海外，就是一個弱點。
能在這場模擬中完全不受影響的服務，是那些「選擇」了把每一層都做到國內自主的服務。
這不是偶然，是有意識的架構決策。

---

### 差別在哪？一張表

<div style="font-size: 0.85em; margin-top: 0.5em;">

| | 伺服器 | 認證 | DNS | CDN 來源 | 海纜斷裂時 |
|---|:---:|:---:|:---:|:---:|---|
| **純國內 stack** | 🟢 台灣 | 🟢 台灣 | 🟢 台灣 | 🟢 台灣 | ✅ 正常運作 |
| **台灣雲端** | 🟢 台灣 | 🔴 美國 | 🔴 美國 | 🟢 台灣 | ⚠️ 能跑但登不進去 |
| **完全海外** | 🔴 海外 | 🔴 海外 | 🔴 海外 | 🔴 海外 | ❌ 完全掛掉 |

</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.1em;">
  大多數台灣企業的服務落在<strong style="color: #f39c12;">中間那一列</strong><br>
  <span style="color: #aaa;">看起來在台灣——但關鍵依賴在海外</span>
</p>

Note:
這張表讓聽眾一目了然地看到三種架構的差異。
重點是中間那一列——「台灣雲端」。
大多數台灣企業和政府服務落在這一列：伺服器確實在台灣（AWS 台灣區域、GCP 彰化），
但認證、DNS、甚至部分 CDN 邏輯依賴海外的控制平面。
這給人「在台灣」的安全感，但在海纜事件中照樣出問題。
這也是最危險的狀態——因為沒出事之前，沒人會去檢視這些隱藏的依賴。

---

### 這不是技術限制——是工程選擇

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.1em;">那些活下來的服務，不是運氣好</p>
  <p style="color: #aaa;">是有人<strong>選擇</strong>多花時間、多花錢、把每一層都做到國內自主</p>
</div>

<div class="fragment" style="margin-top: 1em;">
  <p style="font-size: 1.1em;">那些掛掉的服務，也不是命中注定</p>
  <p style="color: #aaa;">是有人<strong>選擇</strong>用最方便的預設值——而預設值指向海外</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.3em;">
  <strong style="color: #3498db;">差別不在技術能力<br>在於有沒有人問過這個問題：</strong>
</p>

<p class="fragment" style="font-size: 1.4em; margin-top: 0.5em; color: #f39c12;">
  「如果海纜斷了，我們的服務還能用嗎？」
</p>

Note:
這是本段落最重要的訊息：韌性是選擇，不是命運。
AWS、GCP 的預設配置就是會依賴海外控制平面——因為它是全球化服務，這是合理的預設。
但如果你在台灣經營關鍵服務，你需要主動去改這個預設。
大多數企業沒有這樣做，不是做不到，是沒有人問過那個問題。
這也是為什麼我們需要政策和法規來推動——因為靠企業自覺是不夠的。

---

### 該找誰負責？

<p style="font-size: 1.1em; margin-top: 0.5em; color: #aaa;">
  這個階段的每一個問題，都有明確的負責對象
</p>

<div style="display: flex; justify-content: center; gap: 1.5em; flex-wrap: wrap; margin-top: 1em;">
  <div class="fragment" style="text-align: center; min-width: 120px;">
    <p style="font-size: 2.5em; margin: 0;">☁️</p>
    <p>雲端供應商</p>
  </div>
  <div class="fragment" style="text-align: center; min-width: 120px;">
    <p style="font-size: 2.5em; margin: 0;">🏢</p>
    <p>國內服務業者</p>
  </div>
  <div class="fragment" style="text-align: center; min-width: 120px;">
    <p style="font-size: 2.5em; margin: 0;">🏛️</p>
    <p>政府採購<br>+ 數位部</p>
  </div>
  <div class="fragment" style="text-align: center; min-width: 120px;">
    <p style="font-size: 2.5em; margin: 0;">⚖️</p>
    <p>立法者</p>
  </div>
</div>

Note:
進入 Phase 4 的壓力目標環節。
這個階段有四個主要的施壓對象。
接下來逐一說明每個對象該做什麼、可以被要求什麼。
和前面幾個 phase 一樣，每個技術問題都指向一個可以被追究的行動者。

---

### ☁️ 雲端供應商（AWS / GCP / Azure）

<div style="border: 2px solid #3498db; padding: 1em; border-radius: 8px; margin-top: 0.5em; max-width: 85%; margin-left: auto; margin-right: auto; text-align: left;">
  <p style="font-size: 1.1em;"><strong>訴求：</strong></p>
  <p class="fragment">1️⃣ <strong>公布控制平面依賴地圖</strong><br>
    <span style="color: #aaa; font-size: 0.9em;">你台灣區域的哪些功能依賴 us-east-1？哪些依賴東京？客戶有權知道</span></p>
  <p class="fragment" style="margin-top: 0.5em;">2️⃣ <strong>開發「降級自主模式」</strong><br>
    <span style="color: #aaa; font-size: 0.9em;">當國際連線中斷時，台灣機房能獨立完成基本的身分驗證、DNS 解析、憑證驗證</span></p>
  <p class="fragment" style="margin-top: 0.5em;">3️⃣ <strong>不需要完美——需要能動</strong><br>
    <span style="color: #aaa; font-size: 0.9em;">降級模式下功能可以縮減，但核心服務必須維持「可用」</span></p>
</div>

Note:
對雲端供應商的訴求重點是「透明度」和「自主降級」。
(1) 透明度：目前 AWS/GCP/Azure 都沒有公開文件說明台灣區域的控制平面依賴。企業要自己去測試、自己去猜。這不應該是客戶的責任。
(2) 降級自主模式：不是要求每個區域完全獨立（那不現實），而是要求基本功能（登入、DNS、憑證）在國際連線中斷時能繼續運作。
(3) 2021 年 AWS us-east-1 故障後，AWS 有做一些改進——但主要是針對美國境內的區域。台灣的情境（海纜切斷導致整個區域半隔離）需要特別的方案。

---

### 🏢 國內服務業者

<div style="border: 2px solid #f39c12; padding: 1em; border-radius: 8px; margin-top: 0.5em; max-width: 85%; margin-left: auto; margin-right: auto; text-align: left;">
  <p style="font-size: 1.1em;"><strong>訴求：</strong></p>
  <p class="fragment">1️⃣ <strong>稽核你的國際依賴鏈</strong><br>
    <span style="color: #aaa; font-size: 0.9em;">你的認證在哪裡？DNS 權威在哪裡？CDN 來源站在哪裡？<br>「我們用台灣的 AWS」不是答案</span></p>
  <p class="fragment" style="margin-top: 0.5em;">2️⃣ <strong>測試「國際連線中斷」情境</strong><br>
    <span style="color: #aaa; font-size: 0.9em;">你做過災難復原演練（DR drill）嗎？有沒有模擬過海纜斷裂的情況？</span></p>
  <p class="fragment" style="margin-top: 0.5em;">3️⃣ <strong>建立國內降級備援</strong><br>
    <span style="color: #aaa; font-size: 0.9em;">至少在 DNS 和認證這兩層做到國內備援——這是可行的工程選擇，不是天方夜譚</span></p>
</div>

Note:
對國內服務業者——包括銀行、電商、媒體、企業 SaaS 服務等。
最關鍵的訴求是「你知道你的依賴鏈嗎？」
很多企業的 CTO 可能知道「我們用 AWS 台灣區域」，但不一定清楚 IAM、Route 53、ACM 等控制平面服務在哪裡。
第二個訴求是測試：很多企業做災難復原演練（DR drill），但很少模擬「國際連線中斷但國內連線正常」的情境。
這個情境和「機房斷電」或「伺服器故障」完全不同，需要專門的測試。
第三個訴求是建立備援：至少在 DNS（用台灣的 DNS 權威伺服器做備援）和認證（本地 identity provider）這兩層做到國內備援。

---

### 🏛️ 政府採購 + 數位部

<div style="border: 2px solid #2ecc71; padding: 1em; border-radius: 8px; margin-top: 0.5em; max-width: 85%; margin-left: auto; margin-right: auto; text-align: left;">
  <p style="font-size: 1.1em;"><strong>訴求：</strong></p>
  <p class="fragment">1️⃣ <strong>政府外包的數位服務 → 要求國內降級運作計畫</strong><br>
    <span style="color: #aaa; font-size: 0.9em;">採購規格書加一條：「海纜中斷時，本服務如何維持運作？」<br>不能回答就不該得標</span></p>
  <p class="fragment" style="margin-top: 0.5em;">2️⃣ <strong>政府自己的服務要帶頭做到</strong><br>
    <span style="color: #aaa; font-size: 0.9em;">報稅系統、健保卡系統、戶政系統<br>這些服務的整條依賴鏈都在國內嗎？做過測試嗎？</span></p>
  <p class="fragment" style="margin-top: 0.5em;">3️⃣ <strong>數位部應該建立並公布「連線韌性」評估標準</strong><br>
    <span style="color: #aaa; font-size: 0.9em;">不只數海纜有幾條——要看 ISP 路由拓撲、雲端控制平面依賴、國內備援程度</span></p>
</div>

Note:
政府有兩個角色：(1) 作為數位服務的採購者，(2) 作為自己數位服務的運營者。
作為採購者：政府的數位服務外包案（例如各部會的網站、線上申辦系統）的採購規格書，
應該要求廠商提供「海纜中斷降級運作計畫」。這是可以立刻做的行政措施，不需要立法。
作為運營者：政府自己的關鍵服務——報稅、健保、戶政——有沒有做到完全國內自主？
數位部的角色：不只是數海纜有幾條。要深入到 ISP 路由拓撲、雲端控制平面依賴這些細節。
Phase 2 講的「只數海纜」的批判在這裡再次出現。

---

### ⚖️ 立法者

<div style="border: 2px solid #e74c3c; padding: 1em; border-radius: 8px; margin-top: 0.5em; max-width: 85%; margin-left: auto; margin-right: auto; text-align: left;">
  <p style="font-size: 1.1em;"><strong>訴求：</strong></p>
  <p class="fragment">1️⃣ <strong>立法要求關鍵數位服務提供者<br>　維護並公開「連線韌性計畫」</strong><br>
    <span style="color: #aaa; font-size: 0.9em;">包含：國際依賴鏈揭露、降級運作方案、年度測試報告</span></p>
  <p class="fragment" style="margin-top: 0.5em;">2️⃣ <strong>定義「關鍵數位服務」的範圍</strong><br>
    <span style="color: #aaa; font-size: 0.9em;">金融、醫療、政府、通訊、電商——哪些服務必須有韌性要求？</span></p>
  <p class="fragment" style="margin-top: 0.5em;">3️⃣ <strong>沒有立法 = 自願性質 = 沒人會做</strong><br>
    <span style="color: #aaa; font-size: 0.9em;">直到下一次危機發生——然後我們再開檢討會</span></p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.1em; color: #e74c3c;">
  台灣有《資通安全管理法》管資安<br>
  但<strong>沒有法律管「連線韌性」</strong>
</p>

Note:
最後也是最根本的施壓對象：立法者。
目前台灣有《資通安全管理法》要求關鍵基礎設施的資安防護，
但「連線韌性」——也就是當國際連線中斷時服務能否持續運作——不在現行法律的涵蓋範圍內。
沒有法律要求 → 企業沒有動機 → 沒有人會主動去做。
這是一個典型的「沒出事就不會有人在意」的治理缺口。
需要法律框架來定義哪些是「關鍵數位服務」、要求這些服務維護韌性計畫、並定期測試和公開報告。
否則這一切都是自願性質——而自願意味著在下一次危機之前，什麼都不會改變。