<h3 style="color: #e74c3c;">第一階段：0–5 分鐘</h3>

## 為什麼我的電話突然斷了？

Note:
進入第一階段：海纜斷裂後的前五分鐘。
從聽眾的主觀體驗開始，再逐步揭示背後的技術原因。

---

### 你感受到的

- LINE 語音通話 → 機器人聲音 → 斷線 ☎️❌ <!-- .element: class="fragment" -->
- Instagram → 白畫面 <!-- .element: class="fragment" -->
- Google Drive → 載入一半，卡住不動 <!-- .element: class="fragment" -->
- 你看一眼右上角—— <!-- .element: class="fragment" -->

<p class="fragment fade-up" style="font-size: 1.6em; margin-top: 0.8em;">
  Wi-Fi 訊號滿格 📶
</p>

Note:
列出聽眾會立即經歷的症狀。
重點在於最後的反差：訊號滿格但什麼都不能用。
這個矛盾會引出下一張投影片的解釋。

---

### 「是我的問題，還是網路的問題？」

<div class="fragment" style="margin-top: 1em;">
  <p style="font-size: 1.1em;">Wi-Fi 訊號滿格 ≠ 網路正常</p>
</div>

<div class="fragment" style="margin-top: 1em; text-align: left; max-width: 80%; margin-left: auto; margin-right: auto;">
  <p>📱 → 📡 <strong>Wi-Fi</strong>：你的手機到你家路由器</p>
  <p style="color: #aaa;">這段完全正常 ✓</p>
</div>

<div class="fragment" style="text-align: left; max-width: 80%; margin-left: auto; margin-right: auto;">
  <p>📡 → 🌏 <strong>網路</strong>：你家路由器到全世界</p>
  <p style="color: #e74c3c;">這段出事了 ✗</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em;">
  問題不在你家，在<strong>海底</strong>
</p>

Note:
很多人誤以為 Wi-Fi 滿格 = 網路正常。
Wi-Fi 只是最後一哩：手機到路由器的無線連線。
真正的問題在你家牆壁以外——海底電纜斷了，
國際路由正在混亂重算。

---

### 網路到底是什麼？

想像網路是一個由**幾千間郵局**組成的系統 <!-- .element: class="fragment" style="font-size: 1.1em;" -->

<div class="fragment" style="margin-top: 1em;">
  <p>🏣 每間郵局 = 一個網路機房（ISP、資料中心）</p>
</div>

<div class="fragment">
  <p>✉️ 你的資料 = 一封封信件（封包）</p>
</div>

<div class="fragment">
  <p>🛣️ 郵局之間有很多條路可以互相送信</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.1em;">
  你在台北寄信到東京，信會經過好幾間郵局<br>一站一站轉送過去
</p>

Note:
用郵局比喻來解釋網路結構。
重點：網路不是一條線，是很多節點互相轉送資料。
每個 ISP、每個機房就像一間郵局。
你的資料像信件，會被一站一站轉送到目的地。

---

### 郵局怎麼知道信要往哪送？

<div class="fragment" style="margin-top: 1em;">
  <p>每間郵局門口都有一塊<strong>路牌</strong> 🪧</p>
</div>

<div class="fragment" style="margin-top: 0.5em; background: rgba(255,255,255,0.05); padding: 0.8em; border-radius: 8px; max-width: 70%; margin-left: auto; margin-right: auto;">
  <p style="text-align: left;">「要去日本？→ 交給南邊那間郵局」</p>
  <p style="text-align: left;">「要去美國？→ 交給東邊那間郵局」</p>
  <p style="text-align: left;">「要去高雄？→ 交給隔壁那間郵局」</p>
</div>

<div class="fragment" style="margin-top: 1em;">
  <p>這塊路牌，在網路世界叫做<strong>路由表</strong></p>
</div>

<div class="fragment">
  <p>郵局之間互相更新路牌的方法，就叫 <strong>BGP</strong></p>
  <p style="color: #aaa; font-size: 0.8em;">Border Gateway Protocol — 邊界閘道協定</p>
</div>

Note:
路由表就是每個網路節點的「方向指引」。
BGP 是全球網路用來同步這些路由資訊的協定。
不需要記術語，只要記住：
BGP = 郵局之間互相通知「路怎麼走」的系統。

---

### 海纜斷了 = 路斷了

<div class="fragment" style="margin-top: 1em;">
  <p>光纖裡的光訊號<strong>直接消失</strong>（物理斷裂）</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p>離斷點最近的郵局第一個發現：</p>
  <p style="font-size: 1.3em; color: #e74c3c;">「這條路不通了！」</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p>它立刻向鄰居廣播：</p>
  <p style="font-size: 1.2em;">「大家注意！往南的路斷了！<br>不要再把信往這邊送了！」</p>
</div>

<p class="fragment fade-up" style="margin-top: 0.8em; color: #f39c12;">
  這則消息開始一間接一間傳開⋯⋯
</p>

Note:
海纜是光纖，斷裂時光訊號直接消失——
不是慢慢變弱，是瞬間歸零。
連接在該纜線上的路由器（郵局）偵測到鏈路中斷，
透過 BGP 向鄰居宣告：這條路已經失效。
這個宣告會像漣漪一樣擴散到全球網路。

---

### 重新算路的混亂期

<p style="color: #aaa; font-size: 0.9em;">BGP Reconvergence</p>

<div class="fragment" style="margin-top: 0.8em;">
  <p>全球幾千間郵局同時收到消息</p>
  <p style="color: #e74c3c;">但不是<strong>同時</strong>收到</p>
</div>

<div class="fragment" style="margin-top: 0.8em; background: rgba(255,255,255,0.05); padding: 0.8em; border-radius: 8px; max-width: 80%; margin-left: auto; margin-right: auto;">
  <p style="text-align: left;">🏣 A 郵局已經改路牌了 ✓</p>
  <p style="text-align: left;">🏣 B 郵局還不知道路斷了 ✗</p>
  <p style="text-align: left;">🏣 C 郵局收到了，但還在算新路線 ⏳</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p>你的信被⋯⋯</p>
  <ul>
    <li>送到已經斷掉的路 → <strong>丟失</strong></li>
    <li>在兩間郵局之間來回彈 → <strong>繞圈</strong></li>
    <li>沒有郵局願意收 → <strong>退回</strong></li>
  </ul>
</div>

<p class="fragment fade-up" style="margin-top: 0.5em; color: #f39c12;">
  這段混亂期：30 秒 ~ 數分鐘
</p>

Note:
BGP reconvergence 是整個網路重新達成共識的過程。
問題在於：資訊傳播有延遲，各節點更新速度不同。
在這段過渡期，路由表處於不一致狀態——
有些路由器認為舊路還在，有些已經切換新路。
這導致封包被丟棄、迴圈、或送進死胡同。
時間長短取決於網路拓撲複雜度和 BGP 收斂速度。

---

### 對你來說，就是——

<h2 class="fragment" style="color: #e74c3c;">全部斷了</h2>

<div class="fragment" style="margin-top: 1em; text-align: left; max-width: 75%; margin-left: auto; margin-right: auto;">
  <p>封包被丟掉 → 網頁載不出來</p>
  <p>封包繞遠路 → 延遲從 20ms 變 2000ms</p>
  <p>封包來回彈 → 根本到不了目的地</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.1em;">
  實際上 50% 的海纜還在<br>
  但在路由重算完成之前<br>
  <strong>可用率接近零</strong>
</p>

Note:
重點：50% 容量還在，但 BGP 收斂期間幾乎無法使用。
這就像高速公路出了大車禍，雖然隔壁車道還通，
但因為指示牌混亂，所有車都卡在交流道上動不了。
一旦 BGP 收斂完成（所有郵局路牌更新一致），
剩餘 50% 容量才能真正被利用——但接下來會面臨壅塞問題。

---

### 為什麼 LINE 語音死了，<br>但文字可能還活著？

<div class="fragment" style="margin-top: 1em; display: flex; justify-content: center; gap: 2em; flex-wrap: wrap;">
  <div style="background: rgba(46,204,113,0.1); padding: 1em; border-radius: 8px; flex: 1; min-width: 250px; max-width: 350px;">
    <p style="font-size: 1.2em;">💬 文字訊息</p>
    <p>很小的封包（幾 KB）</p>
    <p>能鑽過混亂的空隙</p>
    <p>晚幾秒到也沒差</p>
  </div>
  <div style="background: rgba(231,76,60,0.1); padding: 1em; border-radius: 8px; flex: 1; min-width: 250px; max-width: 350px;">
    <p style="font-size: 1.2em;">🎙️ 語音通話</p>
    <p>持續的即時串流</p>
    <p>掉幾個封包 = 機器人聲</p>
    <p>延遲超過 300ms = 斷線</p>
  </div>
</div>

<div class="fragment" style="margin-top: 1.2em; border-top: 1px solid rgba(255,255,255,0.2); padding-top: 0.8em;">
  <p>更深的問題：</p>
  <p style="color: #e74c3c; font-size: 1.1em;">
    LINE 的通話控制伺服器<strong>在日本</strong><br>
    你的語音<strong>必須出海</strong>才能接通
  </p>
</div>

Note:
文字訊息是小封包，容錯性高，延遲可接受。
語音通話是即時串流，對封包遺失和延遲極度敏感。
更關鍵的是架構問題：LINE 的通話控制（call signaling）
和語音中繼（relay）伺服器都在日本。
即使兩個台灣用戶互打，語音資料仍需經由海纜到日本再回來。
這是一個工程架構的選擇，不是技術限制。

---

### 這個階段，該找誰負責？

<h2 style="color: #3498db;">LINE</h2>

<div style="margin-top: 1em; text-align: left; max-width: 80%; margin-left: auto; margin-right: auto;">
  <p class="fragment">📊 LINE 在台灣有 <strong>~2,100 萬</strong>用戶<br>
    <span style="color: #aaa;">→ 事實上的關鍵通訊基礎設施</span></p>
  <p class="fragment">✅ 文字中繼可以透過 <strong>TPIX</strong> 本地對等互連運作<br>
    <span style="color: #aaa;">→ LINE 已經有本地 peering，文字可以留在島內</span></p>
  <p class="fragment">❌ 但語音/視訊的<strong>通話控制與中繼</strong>仍在日本<br>
    <span style="color: #aaa;">→ 海纜一斷，兩個台北人也打不通電話</span></p>
</div>

<div class="fragment" style="margin-top: 1.2em; border: 2px solid #3498db; padding: 0.8em; border-radius: 8px;">
  <p style="font-size: 1.1em;"><strong>訴求：</strong>在台灣部署本地通話控制與中繼伺服器<br>
  讓語音通話在海纜降級時仍能島內運作</p>
</div>

Note:
LINE 在台灣的地位等同於關鍵基礎設施。
文字訊息已經可以透過 TPIX 本地交換，但語音通話的
call signaling 和 media relay 仍然依賴日本。
這不是技術上做不到，是 LINE 選擇不做。
訴求很具體：部署台灣本地的通話控制伺服器，
讓語音通話在國際鏈路降級時可以純島內運作。
LINE 有足夠的工程資源做到這件事。
