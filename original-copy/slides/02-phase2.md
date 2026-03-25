<h3 style="color: #e74c3c;">第二階段：5–30 分鐘</h3>

## 殭屍網路

Note:
進入第二階段。BGP 已經收斂完成，路由穩定了。
但聽眾會發現：網路「活著」但幾乎不能用。
這個階段要解釋兩件事：壅塞崩潰和 Trombone Effect。

---

### 你感受到的

- 網頁載入一半⋯⋯卡住不動 <!-- .element: class="fragment" -->
- 圖片只出現一半，下面是灰色空白 <!-- .element: class="fragment" -->
- YouTube 轉圈圈轉到天荒地老 <!-- .element: class="fragment" -->
- LINE 文字勉強能傳，但要等很久才送達 <!-- .element: class="fragment" -->

<p class="fragment fade-up" style="font-size: 1.3em; margin-top: 0.8em;">
  訊號滿格 📶 網路「有通」<br>
  <strong style="color: #e74c3c;">但慢到幾乎不能用</strong>
</p>

Note:
跟 Phase 1 不同——Phase 1 是完全斷線（BGP 收斂期間）。
現在是「有連線但極度緩慢」，這其實更讓人困惑。
聽眾會不斷重新整理、重試，反而讓情況更糟。

---

### 等一下——路不是已經修好了嗎？

<div class="fragment" style="margin-top: 1em;">
  <p>BGP 收斂完成 ✓</p>
  <p style="color: #aaa;">所有郵局的路牌已經更新一致</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p>剩餘 50% 的海纜正常運作 ✓</p>
  <p style="color: #aaa;">路是通的，信可以送</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.3em; color: #f39c12;">
  那為什麼還是這麼慢？
</p>

<p class="fragment" style="margin-top: 0.5em; font-size: 1.1em;">
  因為路沒斷——但<strong style="color: #e74c3c;">路太擠了</strong>
</p>

Note:
這張投影片是 Phase 1 到 Phase 2 的轉折。
Phase 1 的問題是「路牌混亂」（BGP 收斂）。
Phase 2 的問題是「路太少車太多」（壅塞崩潰）。
兩個完全不同的故障機制，但對使用者來說感覺差不多。

---

### 壅塞崩潰：想像一條高速公路

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.2em;">台灣的國際網路 = 一條 <strong>10 線道</strong>的高速公路 🛣️</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p>平常車流量大約用了 <strong>5–6 線道</strong></p>
  <p style="color: #aaa;">還有空間，大家都能順暢通行</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p style="color: #e74c3c; font-size: 1.2em;">海纜斷 50% = 突然只剩 <strong>5 線道</strong></p>
</div>

<p class="fragment fade-up" style="margin-top: 0.8em; font-size: 1.1em;">
  但車流量沒有變——<br>
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
  <p style="color: #f39c12;">但全台灣 2,300 萬人的電腦<br>都在同一時間「再寄一次」⋯⋯</p>
</div>

<p class="fragment fade-up" style="margin-top: 0.5em; font-size: 1.2em; color: #e74c3c;">
  🏔️ 信件雪崩
</p>

Note:
TCP 重傳機制在正常情況下很好用——偶爾掉一個封包就重送。
但在全面壅塞時，所有人同時重送變成災難。
這就像塞車時大家都猛按喇叭、硬切換車道，
只會讓交通更癱瘓。

---

### 數字會說話

<div style="display: flex; justify-content: center; gap: 2em; flex-wrap: wrap; margin-top: 0.8em;">
  <div class="fragment" style="background: rgba(46,204,113,0.1); padding: 1em; border-radius: 8px; flex: 1; min-width: 200px; max-width: 280px; text-align: center;">
    <p style="font-size: 0.9em; color: #aaa;">海纜剩餘容量</p>
    <p style="font-size: 2.5em; font-weight: bold; color: #2ecc71;">50%</p>
  </div>
  <div class="fragment" style="background: rgba(231,76,60,0.1); padding: 1em; border-radius: 8px; flex: 1; min-width: 200px; max-width: 280px; text-align: center;">
    <p style="font-size: 0.9em; color: #aaa;">實際可用頻寬</p>
    <p style="font-size: 2.5em; font-weight: bold; color: #e74c3c;">15–20%</p>
  </div>
</div>

<p class="fragment fade-up" style="margin-top: 1.2em; font-size: 1.1em;">
  物理容量剩一半<br>
  但壅塞崩潰讓<strong>實際可用的只剩不到五分之一</strong>
</p>

Note:
這是最反直覺的部分。
物理容量 50% 不等於可用頻寬 50%。
因為壅塞崩潰的非線性效應，
當連結利用率超過某個臨界點，有效吞吐量急遽下降。
研究顯示，在嚴重壅塞下，有效利用率可能降到 15-20%。

---

### 你的日常體驗

<div class="fragment" style="margin-top: 0.8em; text-align: left; max-width: 80%; margin-left: auto; margin-right: auto;">
  <p>📄 網頁 → 文字出來了，圖片永遠在轉圈</p>
</div>

<div class="fragment" style="text-align: left; max-width: 80%; margin-left: auto; margin-right: auto;">
  <p>🎬 影片 → 240p 馬賽克畫質，還一直暫停緩衝</p>
</div>

<div class="fragment" style="text-align: left; max-width: 80%; margin-left: auto; margin-right: auto;">
  <p>📥 下載 → 速度從 100 Mbps 掉到 2 Mbps</p>
</div>

<div class="fragment" style="text-align: left; max-width: 80%; margin-left: auto; margin-right: auto;">
  <p>📱 App → 開得起來，但操作什麼都要等 10 秒以上</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; color: #f39c12; font-size: 1.1em;">
  不是斷線，是<strong>「慢到不能用」</strong><br>
  <span style="font-size: 0.85em;">這比完全斷線更讓人抓狂</span>
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
  <p style="font-size: 1.2em;">不需要走海纜，不應該受影響</p>
</div>

<p class="fragment fade-up" style="margin-top: 0.8em; font-size: 1.5em; color: #e74c3c;">
  但它們也壞了 🤯
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

### 但你的電信商說⋯⋯

<div class="fragment" style="margin-top: 0.8em; background: rgba(231,76,60,0.1); padding: 1em; border-radius: 8px;">
  <p style="font-size: 1.1em;">「不行！你不能直接去巷口那間！」</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p>你的電信商規定的路線：</p>
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

### 為什麼電信商要這樣繞？

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.1em;">因為台灣的電信商之間<br><strong>沒有在本地「牽手」</strong></p>
</div>

<div class="fragment" style="margin-top: 0.8em; background: rgba(255,255,255,0.05); padding: 0.8em; border-radius: 8px; max-width: 85%; margin-left: auto; margin-right: auto; text-align: left;">
  <p>🤝 <strong>對等互連（Peering）</strong>：<br>兩家電信商說好「你的用戶可以直接連我的伺服器」</p>
  <p style="margin-top: 0.5em;">🏢 <strong>網路交換中心</strong>：<br>一個讓大家來牽手的地方，台灣的叫 <strong>TPIX</strong></p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p style="color: #e74c3c;">問題：很多台灣 ISP 不來 TPIX 牽手<br>
  <span style="color: #aaa; font-size: 0.85em;">或者來了，但只分享很少的流量</span></p>
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
  <p>所以 ISP 覺得：</p>
  <p style="font-size: 1.1em;">「反正用戶不會發現，何必花錢在本地牽手？」</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.2em; color: #e74c3c;">
  直到海纜出事——<br>
  <strong>那條繞去東京的路塞死了</strong>
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
  海纜壅塞 → 這段繞路卡死<br>
  → 你連 10 公里外的伺服器都連不上
</p>

<div class="fragment" style="margin-top: 0.8em;">
  <p>這就是 <strong>Trombone Effect</strong> 🎺</p>
  <p style="color: #aaa; font-size: 0.85em;">「長號效應」：資料像長號的管子一樣繞一大圈</p>
</div>

---

### 這個階段，該找誰負責？

<div style="margin-top: 0.8em;">
  <p class="fragment" style="font-size: 1.3em;">三個對象 👇</p>
</div>

<div class="fragment" style="display: flex; justify-content: center; gap: 1.5em; flex-wrap: wrap; margin-top: 0.8em;">
  <div style="background: rgba(52,152,219,0.1); padding: 0.6em 1em; border-radius: 8px;">
    <p style="font-size: 1.1em;">🏢 電信商（ISP）</p>
  </div>
  <div style="background: rgba(52,152,219,0.1); padding: 0.6em 1em; border-radius: 8px;">
    <p style="font-size: 1.1em;">🏛️ NCC / 數位部</p>
  </div>
  <div style="background: rgba(52,152,219,0.1); padding: 0.6em 1em; border-radius: 8px;">
    <p style="font-size: 1.1em;">⚖️ 立法者</p>
  </div>
</div>

Note:
Phase 2 的壓力目標比 Phase 1 多。
Phase 1 只有一個目標（LINE），
Phase 2 有三個：ISP、監管機構、立法者。
因為這個問題的根源是系統性的——
不是某一家公司的問題，而是整個產業結構和監管的問題。

---

### 🏢 電信商（尤其中華電信）

<div style="margin-top: 0.8em; text-align: left; max-width: 85%; margin-left: auto; margin-right: auto;">
  <p class="fragment">你的<strong>對等互連政策</strong>直接決定：<br>
    <span style="color: #aaa;">→ 危機時，用戶能不能連到本地伺服器</span></p>

  <p class="fragment" style="margin-top: 0.8em;">為了省成本而拒絕在 TPIX 對等互連<br>
    <span style="color: #e74c3c;">= 選擇讓你的用戶在海纜出事時斷線</span></p>

  <p class="fragment" style="margin-top: 0.8em;">而且你<strong>不公開</strong>你的對等互連政策<br>
    <span style="color: #aaa;">→ 用戶根本不知道自己的流量被繞到哪裡去</span></p>
</div>

<div class="fragment" style="margin-top: 1em; border: 2px solid #3498db; padding: 0.8em; border-radius: 8px;">
  <p style="font-size: 1.05em;"><strong>訴求：</strong>公開對等互連政策<br>
  在 TPIX 進行充分的本地對等互連<br>
  確保本地流量不會被繞到海外</p>
</div>

Note:
中華電信是台灣最大的 ISP，掌握最多國際頻寬。
但它也因為市場地位強勢，對等互連意願較低——
它認為自己的網路最大，其他人應該付費來連它。
這在商業上可以理解，但在國家韌性的角度是不負責任的。
訴求：透明化對等互連政策，在 TPIX 充分互連。

---

### 🏛️ NCC / 數位部

<div style="margin-top: 0.8em; text-align: left; max-width: 85%; margin-left: auto; margin-right: auto;">
  <p class="fragment">你在稽核 ISP 的<strong>路由拓撲</strong>嗎？<br>
    <span style="color: #aaa;">→ 知道哪些本地流量正在被繞到國際嗎？</span></p>

  <p class="fragment" style="margin-top: 0.8em;">「台灣有 14 條海纜」這個數字<br>
    <span style="color: #e74c3c;"><strong>毫無意義</strong></span><br>
    <span style="color: #aaa;">→ 如果 ISP 把本地流量都繞到國際鏈路上</span></p>

  <p class="fragment" style="margin-top: 0.8em;">重點不是有幾條纜<br>
    <span style="color: #f39c12;">→ 而是流量實際上怎麼走</span></p>
</div>

<div class="fragment" style="margin-top: 1em; border: 2px solid #3498db; padding: 0.8em; border-radius: 8px;">
  <p style="font-size: 1.05em;"><strong>訴求：</strong>稽核 ISP 的 transit 和 peering 拓撲<br>
  不要只數纜線數量，要看實際路由架構</p>
</div>

Note:
NCC 和數位部是台灣的電信監管機關。
目前的韌性討論集中在「有幾條海纜」，
但這只是硬體層面的數字。
真正重要的是：流量實際上怎麼走？
如果 ISP 不做本地對等互連，
那就算有 100 條海纜，本地流量還是會被繞到國際。
監管機關需要從「數纜線」升級到「看拓撲」。

---

### ⚖️ 立法者

<div style="margin-top: 0.8em; text-align: left; max-width: 85%; margin-left: auto; margin-right: auto;">
  <p class="fragment">目前<strong>沒有任何法律</strong>要求 ISP：</p>

  <div class="fragment" style="margin-top: 0.5em; background: rgba(255,255,255,0.05); padding: 0.8em; border-radius: 8px;">
    <p style="text-align: left;">❌ 在本地進行對等互連</p>
    <p style="text-align: left;">❌ 公開揭露對等互連政策</p>
    <p style="text-align: left;">❌ 確保本地流量留在島內</p>
  </div>

  <p class="fragment" style="margin-top: 0.8em; color: #f39c12; font-size: 1.1em;">
    這是一個<strong>監管空白</strong>
  </p>

  <p class="fragment" style="color: #aaa;">
    ISP 沒有法律義務做這些事<br>
    所以它們選擇不做——因為省錢
  </p>
</div>

<div class="fragment" style="margin-top: 0.8em; border: 2px solid #3498db; padding: 0.8em; border-radius: 8px;">
  <p style="font-size: 1.05em;"><strong>訴求：</strong>立法要求 ISP 對等互連透明化<br>
  並建立本地流量路由的最低標準</p>
</div>

Note:
這是結構性的問題。
即使 NCC 想管，目前法律也沒有給它足夠的工具。
需要立法層面的改變：
1. 要求 ISP 公開對等互連政策
2. 建立本地流量路由的最低標準
3. 讓監管機關有法源去稽核和要求改善
沒有法律框架，一切都是自願的——
而自願意味著直到出事前沒人會做。

---

### Phase 2 壓力目標總覽

<div style="margin-top: 0.5em; font-size: 0.9em;">

| 對象 | 訴求 |
|------|------|
| **ISP（尤其中華電信）** | 公開 peering 政策；在 TPIX 充分互連；本地流量留島內 |
| **NCC / 數位部** | 稽核 ISP 路由拓撲，不要只數纜線 |
| **立法者** | 立法要求 ISP 對等互連透明化 |

</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.1em;">
  <strong>共同主題：</strong><br>
  問題不是硬體不夠<br>
  是<strong>路由政策</strong>讓本地流量被迫出海
</p>

Note:
總結 Phase 2 的三個壓力目標。
共同主題：硬體（海纜數量）不是瓶頸，
路由政策（ISP 不做本地互連）才是。
這是一個可以被改變的政策選擇，不是物理限制。

---

### 這個階段的兩個關鍵

<div class="fragment" style="margin-top: 1em; display: flex; justify-content: center; gap: 2em; flex-wrap: wrap;">
  <div style="background: rgba(231,76,60,0.1); padding: 1em; border-radius: 8px; flex: 1; min-width: 250px; max-width: 380px;">
    <p style="font-size: 1.1em; color: #e74c3c;">❶ 壅塞崩潰</p>
    <p style="font-size: 0.95em;">50% 容量 ≠ 50% 速度<br>重傳風暴讓可用頻寬<br>只剩 <strong>15–20%</strong></p>
  </div>
  <div style="background: rgba(243,156,18,0.1); padding: 1em; border-radius: 8px; flex: 1; min-width: 250px; max-width: 380px;">
    <p style="font-size: 1.1em; color: #f39c12;">❷ Trombone Effect</p>
    <p style="font-size: 0.95em;">ISP 不做本地互連<br>本地流量被迫出海繞路<br>連<strong>台灣的伺服器</strong>都受害</p>
  </div>
</div>

<p class="fragment fade-up" style="margin-top: 1.2em; font-size: 1.1em;">
  這兩個問題加在一起：<br>
  <strong>「有網路」和「能用」之間的巨大落差</strong>
</p>

Note:
總結 Phase 2 的兩個核心概念。
壅塞崩潰：物理容量和實際可用頻寬的非線性關係。
Trombone Effect：路由政策讓不需要出海的流量也受害。
兩者疊加造成「殭屍網路」：看起來活著，實際上幾乎不能用。

---

### 但更糟的還在後面⋯⋯

<div class="fragment" style="margin-top: 1em;">
  <p>現在你能用的那些服務——</p>
  <p style="color: #aaa;">Google 搜尋偶爾能出結果、一些網頁還看得到</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.1em;">它們之所以還活著，是因為<strong>「快取」</strong></p>
  <p style="color: #aaa;">之前存在台灣的副本，暫時還能用</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.1em;">但快取有<strong>保存期限</strong>⋯⋯</p>
</div>

<p class="fragment fade-up" style="margin-top: 0.8em; font-size: 1.5em; color: #e74c3c;">
  時間一到，它們也會一個接一個壞掉 ⏳
</p>

Note:
為 Phase 3 做預告。
目前還能用的服務，很多是靠 CDN 快取在撐。
快取有 TTL（存活時間），一旦過期就要向海外原始伺服器要新的。
但海纜壅塞，要不到 → 快取過期 → 服務掛掉。
這個「逐步崩壞」的模式會在 Phase 3 詳細解釋。