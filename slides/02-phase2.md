<h3 style="color: #e74c3c;">第二階段：5–30 分鐘</h3>

## 殭屍網路

Note:
進入第二階段。BGP 已經收斂完成，路由穩定了。
但聽眾會發現：網路「活著」但幾乎不能用。
這個階段要解釋兩件事：壅塞崩潰和 Trombone Effect。

---

### 你可能感受到的

- 網頁載入一半⋯⋯卡住不動 <!-- .element: class="fragment" -->
- 圖片只出現一半，下面是灰色空白 <!-- .element: class="fragment" -->
- 可能 YouTube 轉圈圈轉到天荒地老 <!-- .element: class="fragment" -->
- 可能 LINE 文字勉強能傳，但要等很久才送達 <!-- .element: class="fragment" -->

<p class="fragment fade-up" style="font-size: 1.3em; margin-top: 0.8em;">
  訊號滿格 📶 看似「有通」<br>
  <strong style="color: #e74c3c;">但可能慢到幾乎不能用</strong>
</p>

Note:
跟 Phase 1 不同——Phase 1 是完全斷線（BGP 收斂期間）。
現在是「有連線但極度緩慢」，這其實更讓人困惑。
聽眾會不斷重新整理、重試，反而讓情況更糟。

---

### 等欸，路不是已經修好了嗎？

<div class="fragment" style="margin-top: 1em;">
  <p>BGP reconvergence 完成 ✓</p>
  <p style="color: #aaa;">所有郵局的路牌已經更新一致</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p>剩餘 50% 的對外網路流量正常運作 ✓</p>
  <p style="color: #aaa;">路是通的，信可以送</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.3em; color: #f39c12;">
  那為什麼還是這麼慢？
</p>

<p class="fragment" style="margin-top: 0.5em; font-size: 1.1em;">
  路沒斷，但<strong style="color: #e74c3c;">路太擠了</strong>
</p>

Note:
這張投影片是 Phase 1 到 Phase 2 的轉折。
Phase 1 的問題是「路牌混亂」（BGP 收斂）。
Phase 2 的問題是「路太少車太多」（壅塞崩潰）。
兩個完全不同的故障機制，但對使用者來說感覺差不多。

---

### 想像一條高速公路

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.2em;">台灣的對外網路流量 = 一條 <strong>10 線道</strong>的高速公路 🛣️</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p>假設平常車流量大約用了 <strong>7–8 線道</strong></p>
  <p style="color: #aaa;">還有空間，大家都能順暢通行</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p style="color: #e74c3c; font-size: 1.2em;">但失去 50% 對外流量 = 突然只剩 <strong>5 線道</strong></p>
</div>

<p class="fragment fade-up" style="margin-top: 0.8em; font-size: 1.1em;">
  但車流量沒有變<br>
  <strong>一樣多的車，一半的路</strong>
</p>

Note:
用高速公路比喻來解釋壅塞。
平常海纜利用率大約 40-60%，所以有餘裕。
斷掉一半之後，剩餘容量立刻被塞滿。
重點：車（流量）不會因為路變少就自動減少。

---

### 塞車的連鎖反應

<p style="color: #aaa; font-size: 0.9em;">為什麼不是「慢一半」而是「幾乎不能動」？</p>

<div class="fragment" style="margin-top: 0.8em; background: rgba(255,255,255,0.05); padding: 0.8em; border-radius: 8px; max-width: 85%; margin-left: auto; margin-right: auto;">
  <p style="text-align: left;">🚗 車太多，有些車擠不進去 → <strong>被丟包</strong></p>
  <p style="text-align: left;" class="fragment">🔄 被丟包的車說：「我再試一次！」 → <strong>重新上路</strong></p>
  <p style="text-align: left;" class="fragment">🚗🚗🚗 大家都在重試 → 路上的車<strong>反而更多了</strong></p>
  <p style="text-align: left;" class="fragment">💥 更多車被丟包 → 更多重試 → <strong>惡性循環</strong></p>
</div>

<p class="fragment fade-up" style="margin-top: 0.8em; color: #e74c3c; font-size: 1.1em;">
  這就叫「壅塞崩潰」<br>
  <span style="color: #aaa; font-size: 0.8em;">Congestion Collapse</span>
</p>

Note:
這是壅塞崩潰的核心機制。
TCP 協定在偵測到封包遺失時會重傳。
但當所有連線同時重傳，反而製造更多流量，
讓壅塞更嚴重，導致更多封包遺失，再觸發更多重傳。
這個正回饋循環就是 congestion collapse。

---

### 用寄信來理解

<p style="color: #aaa;">（延續上一階段的郵局比喻）</p>

<div class="fragment" style="margin-top: 0.8em;">
  <p>你寄了一封信到美國 ✉️</p>
</div>

<div class="fragment" style="margin-top: 0.5em;">
  <p>路上太擠，信被丟了 → 你沒收到回信</p>
</div>

<div class="fragment" style="margin-top: 0.5em;">
  <p>你想：「大概寄丟了，再寄一次吧！」</p>
  <p style="color: #aaa;">你的電腦也是這樣想的（TCP 重傳）</p>
</div>

<div class="fragment" style="margin-top: 0.5em;">
  <p style="color: #f39c12;">假如全台灣 2,300 萬人的手機<br>都在同一時間「再寄一次」⋯⋯</p>
</div>

<p class="fragment fade-up" style="margin-top: 0.5em; font-size: 1.2em; color: #e74c3c;">
  🏔️ 信件雪崩
</p>

Note:
TCP 重傳機制在正常情況下很好用——偶爾掉一個封包就重送。
但在全面壅塞時，所有人同時重送變成災難。
這就像塞車時大家都猛按喇叭、硬切換車道，
只會讓交通更癱瘓。
這是最反直覺的部分。
物理容量 50% 不等於可用頻寬 50%。
因為壅塞崩潰的非線性效應，
當連結利用率超過某個臨界點，有效吞吐量急遽下降。
研究顯示，在嚴重壅塞下，有效利用率可能降到 15-20%。

---

### 你可能會碰到的體驗

<div class="fragment" style="margin-top: 0.8em; text-align: left; max-width: 80%; margin-left: auto; margin-right: auto;">
  <p>📄 網頁 → 可能文字出來了，圖片永遠在轉圈</p>
</div>

<div class="fragment" style="text-align: left; max-width: 80%; margin-left: auto; margin-right: auto;">
  <p>🎬 影片 → 可能 240p 馬賽克畫質，還一直暫停緩衝</p>
</div>

<div class="fragment" style="text-align: left; max-width: 80%; margin-left: auto; margin-right: auto;">
  <p>📥 下載 → 可能速度從 100 Mbps 掉到 2 Mbps</p>
</div>

<div class="fragment" style="text-align: left; max-width: 80%; margin-left: auto; margin-right: auto;">
  <p>📱 App → 可能開得起來，但操作什麼都要等 10 秒以上</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; color: #f39c12; font-size: 1.1em;">
  可能不是斷線，但會<strong>卡到哭出來</strong><br>
  <span style="font-size: 0.85em;">qq</span>
</p>

Note:
讓聽眾具體感受壅塞崩潰的影響。
「慢到不能用」比「完全斷線」更痛苦，
因為你會一直重試、一直等待，浪費大量時間。
而且你無法判斷是自己的問題還是系統性的問題。

---

### 接下來的問題更奇怪

<div class="fragment" style="margin-top: 1em;">
  <p style="font-size: 1.2em;">有些網站明明伺服器<strong>就在台灣</strong></p>
</div>

<div class="fragment" style="margin-top: 0.5em;">
  <p style="font-size: 1.2em;">理論上不需要走對外流量，不應該受影響</p>
</div>

<p class="fragment fade-up" style="margin-top: 0.8em; font-size: 1.5em; color: #e74c3c;">
  但它們可能也壞了 🤯
</p>

<p class="fragment" style="margin-top: 0.5em; color: #aaa;">
  為什麼？
</p>

Note:
轉折投影片。壅塞崩潰解釋了「國際流量為什麼慢」。
但接下來要解釋一個更詭異的現象：
明明伺服器在台灣、不需要走海纜的服務，為什麼也壞了？
這就要引入 Trombone Effect。

---

### 🏪 便利商店的故事

<div class="fragment" style="margin-top: 1em;">
  <p style="font-size: 1.2em;">你家巷口有一間 7-11</p>
  <p style="color: #aaa;">你想買一瓶水</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.1em;">正常情況：</p>
  <p style="font-size: 1.3em;">🏠 → 🚶 走路 30 秒 → 🏪 買到了！</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.1em;">
  這就像你在台灣連一個<strong>台灣的伺服器</strong><br>
  資料不用出海，直接在島內傳
</p>

Note:
開始用便利商店比喻來解釋 Trombone Effect。
先建立「正常情況」的心智模型：
伺服器在台灣，你也在台灣，資料直接在島內傳遞。
像走路去巷口 7-11 買東西一樣簡單直接。

---

### 但有些電信商說⋯⋯

<div class="fragment" style="margin-top: 0.8em; background: rgba(231,76,60,0.1); padding: 1em; border-radius: 8px;">
  <p style="font-size: 1.1em;">「不行！你不能直接去巷口那間！」</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p>有些電信商規定的路線：</p>
</div>

<div class="fragment" style="margin-top: 0.5em; background: rgba(255,255,255,0.05); padding: 0.8em; border-radius: 8px; max-width: 85%; margin-left: auto; margin-right: auto;">
  <p style="font-size: 1.1em;">
    🏠 你家<br>
    → ✈️ 先搭飛機去<strong>東京</strong><br>
    → 🏪 在東京的 7-11 結帳<br>
    → ✈️ 飛回台灣<br>
    → 🏠 拿到你的水
  </p>
</div>

<p class="fragment fade-up" style="margin-top: 0.8em; font-size: 1.2em; color: #f39c12;">
  就為了一瓶巷口就有的水 🤦
</p>

Note:
這就是 Trombone Effect 的核心比喻。
你的請求被迫出海繞一圈再回來。
明明伺服器就在旁邊，但你的 ISP 的路由設定
把流量送到日本或香港再繞回來。
聽眾應該在這裡感到荒謬——因為這確實很荒謬。

---

### 為什麼某些電信商要這樣繞？

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.1em;">因為台灣的電信商之間<br><strong>沒有在本地「牽手」</strong></p>
</div>

<div class="fragment" style="margin-top: 0.8em; background: rgba(255,255,255,0.05); padding: 0.8em; border-radius: 8px; max-width: 85%; margin-left: auto; margin-right: auto; text-align: left;">
  <p>🤝 <strong>對等互連（Peering）</strong>：<br>兩家廠商說好「你的使用者可以直接連我的伺服器」</p>
  <p style="margin-top: 0.5em;">🏢 <strong>網路交換中心</strong>：<br>一個讓大家來牽手的地方
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p style="color: #e74c3c;">問題：有些台灣 ISP 不是很喜歡跟別人牽手<br>
  <span style="color: #aaa; font-size: 0.85em;">或者分享很少的流量</span></p>
</div>

Note:
解釋 peering 和 Internet Exchange 的概念。
用「牽手」比喻對等互連。
TPIX 是台灣的網路交換中心，理論上 ISP 可以在這裡
直接交換流量，不需要繞到海外。
但現實是：很多 ISP（尤其大的）不願意在 TPIX 對等互連，
因為它們覺得自己的網路比較大，不需要跟小的「牽手」。
或者來了但只開放很小的頻寬。

---

### 平常你感覺不到

<div class="fragment" style="margin-top: 1em;">
  <p>繞去東京再回來只多 <strong>20–30 毫秒</strong></p>
  <p style="color: #aaa;">你根本感覺不到差別</p>
</div>

<div class="fragment" style="margin-top: 1em;">
  <p>所以有些 ISP 覺得：</p>
  <p style="font-size: 1.1em;">「反正使用者不會發現，何必花錢在本地牽手？」</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.2em; color: #e74c3c;">
  直到海纜出事：<br>
  <strong>那條繞去台灣以外的路塞爆了</strong>
</p>

<p class="fragment" style="margin-top: 0.5em; color: #f39c12;">
  你的水瓶在東京的機場跑道上排隊
</p>

Note:
平常繞路的延遲很小（20-30ms），使用者無感。
所以 ISP 沒有動力花錢在本地對等互連。
但這是一個隱藏的脆弱性：
當海纜壅塞時，所有繞路的流量都會受害。
本來不需要出海的本地流量，因為被迫繞路，
也一起卡在壅塞的國際連結上。

---

### 結果：伺服器在你旁邊，你卻連不上

<div style="margin-top: 0.8em;">
  <div class="fragment" style="display: flex; justify-content: center; gap: 1.5em; flex-wrap: wrap;">
    <div style="background: rgba(255,255,255,0.05); padding: 0.8em; border-radius: 8px; min-width: 200px; text-align: center;">
      <p>📍 伺服器位置</p>
      <p style="font-size: 1.3em; font-weight: bold;">台北內湖</p>
      <p style="color: #aaa;">離你 10 公里</p>
    </div>
    <div style="background: rgba(255,255,255,0.05); padding: 0.8em; border-radius: 8px; min-width: 200px; text-align: center;">
      <p>📍 你的資料實際走的路</p>
      <p style="font-size: 1.3em; font-weight: bold;">台北 → 東京 → 台北</p>
      <p style="color: #aaa;">繞了 4,000 公里</p>
    </div>
  </div>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.2em; color: #e74c3c;">
  對外網路流量壅塞 → 這段繞路卡死<br>
  → 你連 10 公里外的伺服器都連不上
</p>

<div class="fragment" style="margin-top: 0.8em;">
  <p>這就是 <strong>tromboning</strong> 🎺</p>
  <p style="color: #aaa; font-size: 0.85em;">「長號效應」：資料像長號的管子一樣繞一大圈</p>
</div>

---

### 這個階段的兩個可能主要瓶頸

<div class="fragment" style="margin-top: 1em; display: flex; justify-content: center; gap: 2em; flex-wrap: wrap;">
  <div style="background: rgba(231,76,60,0.1); padding: 1em; border-radius: 8px; flex: 1; min-width: 250px; max-width: 380px;">
    <p style="font-size: 1.1em; color: #e74c3c;">❶ 大塞車</p>
    <p style="font-size: 0.95em;">50% 對外流量 ≠ 50% 速度<br>可能的可用頻寬<br>只剩 <strong>15–20%</strong></p>
  </div>
  <div style="background: rgba(243,156,18,0.1); padding: 1em; border-radius: 8px; flex: 1; min-width: 250px; max-width: 380px;">
    <p style="font-size: 1.1em; color: #f39c12;">❷ 大繞路</p>
    <p style="font-size: 0.95em;">國內 peering 不夠好<br>本土流量被迫繞路<br>連<strong>本土伺服器</strong>都影響</p>
  </div>
</div>

<p class="fragment fade-up" style="margin-top: 1.2em; font-size: 1.1em;">
  這兩個問題加在一起：<br>
  <strong>「有網路」可能不等於「能用」</strong>
</p>

Note:
總結 Phase 2 的兩個核心概念。
壅塞崩潰：物理容量和實際可用頻寬的非線性關係。
Trombone Effect：路由政策讓不需要出海的流量也受害。
兩者疊加造成「殭屍網路」：看起來活著，實際上幾乎不能用。

---

### 但更糟的可能還在後面⋯⋯

<div class="fragment" style="margin-top: 1em;">
  <p>現在你能用的那些服務：</p>
  <p style="color: #aaa;">Google 搜尋可能偶爾能出結果、一些網頁可能還看得到</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.1em;">它們之所以還活著，可能是因為<strong>「快取」</strong></p>
  <p style="color: #aaa;">之前存在台灣的副本，可能暫時還能用</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.1em;">但快取有<strong>保存期限</strong>⋯⋯</p>
</div>

<p class="fragment fade-up" style="margin-top: 0.8em; font-size: 1.5em; color: #e74c3c;">
  時間一到，它們可能也會一個接一個壞掉 ⏳
</p>

Note:
為 Phase 3 做預告。
目前還能用的服務，很多是靠 CDN 快取在撐。
快取有 TTL（存活時間），一旦過期就要向海外原始伺服器要新的。
但海纜壅塞，要不到 → 快取過期 → 服務掛掉。
這個「逐步崩壞」的模式會在 Phase 3 詳細解釋。