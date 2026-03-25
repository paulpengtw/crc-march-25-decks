<h3 style="color: #e74c3c;">第三階段：30–60 分鐘</h3>

## 剛剛還好好的，怎麼又壞了？

Note:
進入第三階段。壅塞已經穩定下來，ISP 開始做流量管理。
但聽眾會發現一個詭異的現象：之前還能用的東西，開始一個一個壞掉。
這個階段要解釋三個機制：CDN 快取過期、Auth Token 過期、DNS 快取過期。
這三個機制造成的「漸進式崩壞」比完全斷線更危險，因為它讓人無法判斷問題在哪。

---

### 你感受到的

- Google Drive 剛剛還能開——現在卡住了 <!-- .element: class="fragment" -->
- 新聞網站文字有、圖片全消失 <!-- .element: class="fragment" -->
- LINE 閃退後重開——登不回去了 <!-- .element: class="fragment" -->
- 網銀 app 要你重新輸入密碼——然後轉圈圈 <!-- .element: class="fragment" -->

<p class="fragment fade-up" style="font-size: 1.3em; margin-top: 1em;">
  不是一次全壞<br>
  <strong style="color: #f39c12;">是一個一個壞掉</strong>——而且看起來毫無規律
</p>

Note:
這種「漸進式故障」是最讓人困惑的。
完全斷線大家反而知道「網路斷了」，會去找替代方案。
但東西一個一個壞、有些還能用、有些不行——會讓人反覆嘗試、浪費時間、更焦慮。
接下來我們要解釋為什麼會「一個一個壞」。答案是三種「保存期限」同時在倒數。

---

### 便利商店的比喻

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.2em;">你家旁邊的 <strong>7-11</strong> 🏪</p>
  <p style="color: #aaa;">架上有飲料、便當、零食的「複製品」</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.2em;">這些商品從哪來？<strong>海外的倉庫</strong> 🚢</p>
  <p style="color: #aaa;">7-11 不生產東西，它從倉庫進貨、放在架上給你拿</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.1em;">
  網路世界也一樣——<br>
  <strong style="color: #3498db;">CDN</strong> 就是你家旁邊的數位便利商店
</p>

Note:
CDN = Content Delivery Network，內容傳遞網路。
Cloudflare、Akamai、CloudFront 等公司在台灣設有「邊緣節點」（edge node）。
這些節點就像便利商店：把海外伺服器的內容複製一份放在台灣，讓使用者不用每次都跑到海外去拿。
你瀏覽的網頁圖片、CSS、JavaScript 檔案，很多都是從台灣的 CDN 節點送到你手上的。

---

### 保存期限：TTL

<div class="fragment" style="margin-top: 0.8em;">
  <p>便利商店的便當有<strong>保存期限</strong></p>
  <p style="color: #aaa;">過期了就不能賣，要從倉庫補新的</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p>CDN 快取也有保存期限——叫做 <strong style="color: #3498db;">TTL</strong></p>
  <p style="color: #aaa;">Time To Live：這份複製品可以用多久</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.1em;">TTL 可能是 <strong>5 分鐘</strong>，也可能是 <strong>24 小時</strong></p>
  <p style="color: #aaa;">每個網站、每個檔案的設定都不同</p>
</div>

<p class="fragment fade-up" style="margin-top: 0.8em; font-size: 1.1em; color: #f39c12;">
  前 30 分鐘大部分快取還沒到期——所以東西「還能用」<br>
  現在，保存期限開始一個一個到了
</p>

Note:
TTL 是伺服器設定的，告訴 CDN「這份複製品可以用多久」。
新聞網站的首頁圖片可能 TTL 只有 5 分鐘（因為要即時更新）。
jQuery 函式庫可能 TTL 有 1 年（因為幾乎不會變）。
海纜剛斷的前 30 分鐘，大部分快取還在有效期內，所以使用者感覺「還行」。
但隨著時間推移，各種快取的 TTL 陸續到期，問題就開始浮現。

---

### 便利商店補不到貨了

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.2em;">架上的便當過期了 → 要從倉庫補貨</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p style="color: #e74c3c; font-size: 1.2em;">但通往倉庫的路塞爆了 🚛💨</p>
  <p style="color: #aaa;">（海纜壅塞 = 國際連線極度緩慢）</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p>補貨卡車出發了⋯⋯但塞在路上回不來</p>
  <p style="color: #aaa;">CDN 向海外原始伺服器要新資料 → 逾時 → 失敗</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.3em; color: #e74c3c;">
  快取過期 + 補不到貨 = 架上空了
</p>

Note:
當 CDN 快取的 TTL 到期，CDN 節點會向海外的「原始伺服器」（origin server）發出 revalidation 請求。
正常情況下這只需要幾十毫秒。
但現在國際連線壅塞，這個請求要嘛超時、要嘛回應極慢。
CDN 拿不到新資料，就不能繼續提供內容——使用者看到的就是載入失敗。

---

### 為什麼有些能看、有些不行？

<div style="display: flex; justify-content: center; gap: 2em; flex-wrap: wrap; margin-top: 0.8em;">
  <div class="fragment" style="background: rgba(46,204,113,0.1); padding: 1em; border-radius: 8px; flex: 1; min-width: 250px; max-width: 350px;">
    <p style="font-size: 1.1em;">✅ 還能看</p>
    <p style="color: #aaa; font-size: 0.9em;">熱門 YouTube 影片<br>常用網站的 CSS/JS<br>大家都在看的新聞圖片</p>
    <p style="color: #2ecc71; font-size: 0.9em;">→ 快取剛補過、TTL 還沒到</p>
  </div>
  <div class="fragment" style="background: rgba(231,76,60,0.1); padding: 1em; border-radius: 8px; flex: 1; min-width: 250px; max-width: 350px;">
    <p style="font-size: 1.1em;">❌ 看不到了</p>
    <p style="color: #aaa; font-size: 0.9em;">冷門頁面、舊文章<br>你很久沒開的文件<br>TTL 短的即時內容</p>
    <p style="color: #e74c3c; font-size: 0.9em;">→ 快取已過期、補貨失敗</p>
  </div>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.1em;">
  這就是為什麼<strong style="color: #f39c12;">同一個網站有些部分能看、有些不行</strong><br>
  <span style="color: #aaa;">——不是網站壞了，是快取到期的時間不同</span>
</p>

Note:
這解釋了為什麼使用者會覺得故障「很隨機」。
同一個網站，HTML 文字可能快取 TTL 24 小時（還有效），但圖片 TTL 只有 1 小時（已過期）。
所以你會看到文字出現、但圖片全空白的詭異畫面。
越多人同時存取的內容，快取越「新鮮」——因為一直有人觸發補貨。
冷門內容則相反，快取很可能已經過期很久了。

---

### 什麼是 Auth Token？

<p style="font-size: 1.2em; margin-top: 0.5em;">
  先忘掉技術——<br>
  我們去<strong style="color: #3498db;">遊樂園</strong>
</p>

<div class="fragment" style="margin-top: 1em;">
  <p style="font-size: 5em; margin: 0;">🎢</p>
</div>

<p class="fragment fade-up" style="font-size: 1.1em; margin-top: 0.5em;">
  想像你去遊樂園——<br>
  在入口<strong>買了票、驗了身分</strong><br>
  然後工作人員在你手上蓋了一個<strong style="color: #3498db;">章</strong>
</p>

Note:
我們用遊樂園的比喻來解釋 Auth Token。
這個概念對非技術背景的聽眾來說很陌生，但它是理解「為什麼 app 會一個一個登出」的關鍵。
先用最直覺的方式建立概念，後面再補技術細節。

---

### 手上的章 = Auth Token

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.2em;">蓋了章之後，你可以：</p>
  <p>玩雲霄飛車 🎢 — 給工作人員看手上的章 ✓</p>
  <p>玩旋轉木馬 🎠 — 看章 ✓</p>
  <p>買園區餐點 🍔 — 看章 ✓</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p style="color: #aaa;">每次不用重新排隊買票、重新驗身分</p>
  <p style="color: #aaa;">手上的章就代表「這個人已經驗證過了」</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.1em; color: #3498db;">
  <strong>Auth Token 就是你手上的那個章</strong><br>
  <span style="font-size: 0.9em; color: #aaa;">你登入 Google 之後，瀏覽器拿到一個「章」<br>之後開 Gmail、Drive、YouTube 都不用重新登入</span>
</p>

Note:
Auth Token（認證令牌）就是這個「章」。
你在 Google 登入一次，瀏覽器就拿到一個 token。
之後你開 Gmail、Google Drive、YouTube，每次請求都帶著這個 token。
伺服器看到 token 就知道「這是已經登入的使用者」，不用每次都問你帳號密碼。

---

### 但是——章會褪色

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.2em;">遊樂園的章用的是<strong style="color: #f39c12;">特殊墨水</strong></p>
  <p style="color: #aaa;">15 分鐘到 1 小時後，章就會褪色、看不到了</p>
</div>

<div class="fragment" style="margin-top: 1em;">
  <p style="font-size: 1.1em;">為什麼不用永久的墨水？</p>
</div>

<div class="fragment" style="background: rgba(231,76,60,0.1); padding: 0.8em; border-radius: 8px; margin-top: 0.5em; max-width: 80%; margin-left: auto; margin-right: auto;">
  <p style="text-align: left;">🔒 如果有人<strong>偷印了你的章</strong>（token 被盜）</p>
  <p style="text-align: left; color: #aaa;">用褪色墨水 → 小偷最多用 15 分鐘</p>
  <p style="text-align: left; color: #aaa;">用永久墨水 → 小偷可以<strong>永遠冒充你</strong></p>
</div>

<p class="fragment fade-up" style="margin-top: 0.8em; color: #f39c12;">
  所以 token 故意設計成會過期——這是安全機制
</p>

Note:
Token 設計成短期有效是一個安全決策。
如果 token 永遠有效，一旦被竊取（例如透過 XSS 攻擊、中間人攻擊），攻擊者就能永遠冒充你。
短期 token 限制了被盜後的損害範圍：就算被偷，15 分鐘後就失效了。
這就像信用卡的到期日——不是為了方便你，是為了限制被盜用的風險。

---

### 章褪色了——回售票口重蓋

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.2em;">章褪色了 → 走回入口售票處 🎫</p>
  <p style="color: #aaa;">出示你的年票卡，工作人員重新蓋章</p>
  <p style="color: #aaa;">整個過程只要幾秒鐘，你幾乎不會注意到</p>
</div>

<div class="fragment" style="margin-top: 1em;">
  <p style="font-size: 1.3em;">平常這完全不是問題</p>
  <p style="color: #aaa;">app 在背景自動幫你「重蓋章」，你根本感覺不到</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.3em; color: #e74c3c;">
  但是⋯⋯如果售票處在<strong>海的另一邊</strong>呢？
</p>

Note:
正常情況下，token 過期後的「重新認證」是背景自動完成的。
瀏覽器或 app 會自動用 refresh token（像年票卡）去跟認證伺服器要新的 access token。
整個過程幾百毫秒，使用者完全無感。
但關鍵問題來了：認證伺服器在哪裡？

---

### 售票處在海的另一邊

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.1em;">Google 的認證伺服器 → 🇺🇸 美國</p>
  <p style="font-size: 1.1em;">LINE 的認證伺服器 → 🇯🇵 日本</p>
  <p style="font-size: 1.1em;">Microsoft 的認證伺服器 → 🇺🇸 美國</p>
</div>

<div class="fragment" style="margin-top: 1em;">
  <p>你的章褪色了 → 要跨海去重新蓋章</p>
  <p style="color: #e74c3c; font-size: 1.2em;">但海纜壅塞 = 那條路大塞車 🚗🚗🚗</p>
</div>

<p class="fragment fade-up" style="margin-top: 0.8em; font-size: 1.1em;">
  重新蓋章的請求<strong>送出去了⋯⋯但回不來</strong><br>
  <span style="color: #aaa;">等了 30 秒 → 逾時 → 失敗</span>
</p>

<p class="fragment" style="margin-top: 0.5em; font-size: 1.2em; color: #e74c3c;">
  你被登出了。而且<strong>登不回去</strong>。
</p>

Note:
這是 Auth Token 在海纜事件中的核心問題。
Google 的 OAuth 認證伺服器主要在美國（accounts.google.com 解析到美國 IP）。
LINE 的認證走日本的伺服器。Microsoft 的 Azure AD 也在美國。
Token 過期後，app 嘗試跟這些海外伺服器重新認證。
但國際連線壅塞，請求逾時——你就被登出了。
而且登入頁面本身也需要連到海外伺服器，所以連「重新登入」都做不到。

---

### 每個人在不同時間被登出

<div style="margin-top: 0.5em;">
  <div class="fragment" style="background: rgba(255,255,255,0.05); padding: 0.6em 1em; border-radius: 8px; margin-bottom: 0.5em; max-width: 85%; margin-left: auto; margin-right: auto;">
    <p style="text-align: left; margin: 0;">⏱️ 斷纜後 20 分鐘：<span style="color: #e74c3c;">Google Drive</span> 的章褪色了 → 登出</p>
  </div>
  <div class="fragment" style="background: rgba(255,255,255,0.05); padding: 0.6em 1em; border-radius: 8px; margin-bottom: 0.5em; max-width: 85%; margin-left: auto; margin-right: auto;">
    <p style="text-align: left; margin: 0;">⏱️ 斷纜後 35 分鐘：<span style="color: #e74c3c;">LINE</span> 的章褪色了 → 閃退後登不回去</p>
  </div>
  <div class="fragment" style="background: rgba(255,255,255,0.05); padding: 0.6em 1em; border-radius: 8px; margin-bottom: 0.5em; max-width: 85%; margin-left: auto; margin-right: auto;">
    <p style="text-align: left; margin: 0;">⏱️ 斷纜後 45 分鐘：<span style="color: #e74c3c;">網路銀行</span> 的章褪色了 → 要求重新登入 → 失敗</p>
  </div>
  <div class="fragment" style="background: rgba(255,255,255,0.05); padding: 0.6em 1em; border-radius: 8px; margin-bottom: 0.5em; max-width: 85%; margin-left: auto; margin-right: auto;">
    <p style="text-align: left; margin: 0;">⏱️ 斷纜後 50 分鐘：<span style="color: #e74c3c;">公司 Slack</span> 的章褪色了 → 完全斷線</p>
  </div>
</div>

<p class="fragment fade-up" style="margin-top: 0.8em; font-size: 1.2em; color: #f39c12;">
  這就是為什麼看起來「毫無規律」<br>
  <span style="font-size: 0.9em; color: #aaa;">——每個 app 的章在不同時間褪色</span>
</p>

Note:
每個服務的 token 有效期不同：Google 通常 1 小時，有些銀行 app 15 分鐘。
而且每個使用者上次登入的時間不同，所以 token 到期的時間也不同。
這就造成了「漸進式故障」的混亂局面：
你隔壁同事的 Google Drive 還能用（因為他剛登入），你的已經不行了（因為你的 token 剛好到期）。
大家互相詢問「你的能不能用？」得到不同答案，更加困惑。

---

### Token 的真面目

<div class="fragment" style="margin-top: 0.5em;">
  <p style="font-size: 1.1em;">技術上，Token 長這樣：</p>
</div>

<div class="fragment" style="background: rgba(255,255,255,0.05); padding: 0.8em; border-radius: 8px; max-width: 90%; margin: 0.5em auto; font-family: monospace; font-size: 0.7em; word-break: break-all; text-align: left;">
  eyJhbGciOiJSUzI1NiJ9.<span style="color: #3498db;">eyJ1c2VyIjoi5bCP5piOIiwic2NvcGUiOiJkcml2ZSIsImV4cCI6MTcxMTEyMzQ1Nn0</span>.SflKxwRJSMeKKF2QT4fw
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p>這叫 <strong style="color: #3498db;">JWT</strong>（JSON Web Token），裡面包含：</p>
</div>

<div class="fragment" style="display: flex; justify-content: center; gap: 1.5em; flex-wrap: wrap; margin-top: 0.5em;">
  <div style="background: rgba(52,152,219,0.1); padding: 0.6em 1em; border-radius: 8px;">
    <p style="margin: 0;">👤 你是誰</p>
    <p style="margin: 0; color: #aaa; font-size: 0.8em;">user: 小明</p>
  </div>
  <div style="background: rgba(52,152,219,0.1); padding: 0.6em 1em; border-radius: 8px;">
    <p style="margin: 0;">🔑 能做什麼</p>
    <p style="margin: 0; color: #aaa; font-size: 0.8em;">scope: drive</p>
  </div>
  <div style="background: rgba(52,152,219,0.1); padding: 0.6em 1em; border-radius: 8px;">
    <p style="margin: 0;">⏰ 何時到期</p>
    <p style="margin: 0; color: #aaa; font-size: 0.8em;">exp: 1 hr</p>
  </div>
</div>

<p class="fragment" style="margin-top: 0.5em; color: #aaa; font-size: 0.9em;">
  最後一段是<strong>數位簽章</strong>——防止偽造，只有伺服器能產生
</p>

Note:
JWT 是目前最常見的 token 格式。
它分為三段（用 . 分隔）：header（演算法）、payload（內容）、signature（簽章）。
中間的 payload 段用 base64 編碼，裡面就是 JSON 資料。
重點是最後的 signature：它是用伺服器的私鑰簽的，所以沒人能偽造。
伺服器收到 token 時，驗證簽章就知道這是不是自己發的。

---

### Token 的一生

<div style="margin-top: 0.5em; max-width: 90%; margin-left: auto; margin-right: auto;">
  <div class="fragment" style="background: rgba(46,204,113,0.1); padding: 0.6em 1em; border-radius: 8px; margin-bottom: 0.5em;">
    <p style="text-align: left; margin: 0;">1️⃣ <strong>登入</strong>：輸入帳號密碼 → 伺服器給你兩個東西</p>
    <p style="text-align: left; margin: 0; color: #aaa; font-size: 0.9em;">　　Access Token（通行章）有效 15 分鐘～1 小時</p>
    <p style="text-align: left; margin: 0; color: #aaa; font-size: 0.9em;">　　Refresh Token（年票卡）有效數天～數週</p>
  </div>
  <div class="fragment" style="background: rgba(52,152,219,0.1); padding: 0.6em 1em; border-radius: 8px; margin-bottom: 0.5em;">
    <p style="text-align: left; margin: 0;">2️⃣ <strong>使用中</strong>：每次操作都帶著 Access Token</p>
    <p style="text-align: left; margin: 0; color: #aaa; font-size: 0.9em;">　　伺服器看章就放行，不用每次都驗密碼</p>
  </div>
  <div class="fragment" style="background: rgba(243,156,18,0.1); padding: 0.6em 1em; border-radius: 8px; margin-bottom: 0.5em;">
    <p style="text-align: left; margin: 0;">3️⃣ <strong>章褪色</strong>：Access Token 到期 → 用 Refresh Token 自動換新章</p>
    <p style="text-align: left; margin: 0; color: #aaa; font-size: 0.9em;">　　背景自動完成，你完全不會發現</p>
  </div>
  <div class="fragment" style="background: rgba(231,76,60,0.1); padding: 0.6em 1em; border-radius: 8px;">
    <p style="text-align: left; margin: 0;">4️⃣ <strong>年票也到期</strong>：Refresh Token 也失效 → 必須重新輸入帳號密碼</p>
    <p style="text-align: left; margin: 0; color: #aaa; font-size: 0.9em;">　　這就是為什麼你偶爾會被要求「重新登入」</p>
  </div>
</div>

Note:
OAuth 2.0 的標準流程。
Access Token 是短期的（像蓋在手上的章），Refresh Token 是長期的（像年票卡）。
正常情況下，Access Token 到期時，app 會自動用 Refresh Token 去跟伺服器換新的 Access Token。
這整個流程在背景發生，使用者毫無感覺。
只有當 Refresh Token 也到期（通常幾天到幾週），才會要求使用者重新輸入帳號密碼。

---

### 為什麼不能給永久通行證？

<div style="display: flex; justify-content: center; gap: 2em; flex-wrap: wrap; margin-top: 0.8em;">
  <div class="fragment" style="background: rgba(231,76,60,0.1); padding: 1em; border-radius: 8px; flex: 1; min-width: 250px; max-width: 350px;">
    <p style="font-size: 1.1em;">🔓 如果 Token 永久有效</p>
    <p style="color: #aaa; font-size: 0.9em;">被偷了 → 攻擊者永遠能冒充你</p>
    <p style="color: #aaa; font-size: 0.9em;">權限變了 → 舊 token 還有舊權限</p>
    <p style="color: #aaa; font-size: 0.9em;">離職了 → token 還能用</p>
  </div>
  <div class="fragment" style="background: rgba(46,204,113,0.1); padding: 1em; border-radius: 8px; flex: 1; min-width: 250px; max-width: 350px;">
    <p style="font-size: 1.1em;">🔒 Token 定期過期</p>
    <p style="color: #aaa; font-size: 0.9em;">被偷了 → 最多 15 分鐘就失效</p>
    <p style="color: #aaa; font-size: 0.9em;">權限變了 → 下次換發會更新</p>
    <p style="color: #aaa; font-size: 0.9em;">離職了 → token 自然失效</p>
  </div>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.1em;">
  Token 過期不是設計缺陷——是<strong style="color: #2ecc71;">安全機制</strong><br>
  <span style="color: #aaa; font-size: 0.9em;">就像門鎖密碼定期更換：不方便，但更安全</span>
</p>

Note:
這是安全與便利的根本取捨。
永久 token 就像一把永遠不換的鑰匙——方便，但一旦被複製就完蛋。
短期 token 就像定期更換的密碼鎖——麻煩，但被破解的損害有限。
在正常網路環境下，這個取捨很合理：重新認證只要幾百毫秒，使用者無感。
但在海纜事件中，「重新認證」這個步驟突然變成了致命弱點。

---

### 海纜斷裂時的連鎖反應

<div style="margin-top: 0.5em; max-width: 90%; margin-left: auto; margin-right: auto;">
  <div class="fragment" style="border-left: 3px solid #3498db; padding-left: 1em; margin-bottom: 0.5em;">
    <p style="margin: 0;">Access Token 到期</p>
    <p style="margin: 0; color: #aaa; font-size: 0.9em;">app 在背景嘗試用 Refresh Token 換新的</p>
  </div>
  <div class="fragment" style="border-left: 3px solid #f39c12; padding-left: 1em; margin-bottom: 0.5em;">
    <p style="margin: 0;">Refresh 請求送往海外認證伺服器</p>
    <p style="margin: 0; color: #aaa; font-size: 0.9em;">但國際連線壅塞⋯⋯等 10 秒、20 秒⋯⋯</p>
  </div>
  <div class="fragment" style="border-left: 3px solid #e74c3c; padding-left: 1em; margin-bottom: 0.5em;">
    <p style="margin: 0; color: #e74c3c;">逾時失敗 ✗</p>
    <p style="margin: 0; color: #aaa; font-size: 0.9em;">app 判定「認證失效」→ 強制登出</p>
  </div>
  <div class="fragment" style="border-left: 3px solid #e74c3c; padding-left: 1em; margin-bottom: 0.5em;">
    <p style="margin: 0;">跳出登入頁面 → 你輸入帳號密碼</p>
    <p style="margin: 0; color: #e74c3c; font-size: 0.9em;">但登入頁面本身也要連到海外伺服器 → 也逾時 ✗</p>
  </div>
</div>

<p class="fragment fade-up" style="margin-top: 0.8em; font-size: 1.2em; color: #e74c3c;">
  登出了，而且<strong>登不回去</strong>
</p>

Note:
這是一個連鎖失敗：
1. Access Token 過期（正常機制）
2. Refresh 請求因為壅塞而逾時（不正常）
3. App 判定認證失效，強制登出（正常反應）
4. 使用者嘗試重新登入，但登入流程本身也需要國際連線（致命弱點）
特別是 OAuth 登入流程——「用 Google 登入」按鈕要連到 accounts.google.com，
而那個伺服器在美國。所以連登入頁面都打不開。

---

### 你的 App 正在一個一個登出

<div class="fragment" style="background: rgba(255,255,255,0.05); padding: 0.8em; border-radius: 8px; margin-top: 0.5em; max-width: 90%; margin-left: auto; margin-right: auto;">
  <p style="text-align: left; font-size: 1em;">
    <span style="color: #2ecc71;">t+0 min</span>　海纜斷裂——所有 Token 開始倒數<br>
    <span style="color: #2ecc71;">t+15 min</span>　<span style="color: #aaa;">網銀 token 到期 → 被登出</span><br>
    <span style="color: #f39c12;">t+25 min</span>　<span style="color: #aaa;">Slack token 到期 → 離線</span><br>
    <span style="color: #f39c12;">t+35 min</span>　<span style="color: #aaa;">LINE 需要重新驗證 → 失敗</span><br>
    <span style="color: #e74c3c;">t+45 min</span>　<span style="color: #aaa;">Google Drive token 到期 → 無法存取文件</span><br>
    <span style="color: #e74c3c;">t+60 min</span>　<span style="color: #aaa;">幾乎所有需要認證的服務都已失效</span>
  </p>
</div>

<p class="fragment fade-up" style="margin-top: 0.8em; font-size: 1.1em;">
  不是「一次斷線」——是一場<strong style="color: #e74c3c;">慢動作的大規模登出</strong><br>
  <span style="color: #aaa; font-size: 0.9em;">每個人、每個 app、不同時間——看起來完全隨機</span>
</p>

Note:
這張投影片把整個 auth token 故事做一個時間軸總結。
重點是讓聽眾理解：這不是「網路斷了」這麼簡單。
而是一個看不見的倒數計時器在每個 app 裡面跑著，
到期的那一刻，那個 app 就「死」了。
而且因為每個服務的 token 有效期不同、每個人登入的時間不同，
所以整個過程看起來完全隨機、毫無規律。

---

### 接下來——DNS

<p style="font-size: 1.2em; margin-top: 1em;">
  CDN 快取到期 → 內容消失<br>
  Auth Token 到期 → 被登出
</p>

<p class="fragment fade-up" style="font-size: 1.3em; margin-top: 1em; color: #f39c12;">
  還有第三個東西也在倒數⋯⋯
</p>

<p class="fragment" style="font-size: 1.2em; margin-top: 0.5em;">
  而且這個東西壞掉的話<br>
  <strong style="color: #e74c3c;">連網站在哪都找不到</strong>
</p>

Note:
過渡頁面，從 Auth Token 轉到 DNS。
前面兩個問題是「內容拿不到」和「身分認不了」。
第三個問題更根本：「地址都查不到」。
DNS 對一般聽眾來說更陌生，所以我們需要從最基礎開始解釋。

---

### 什麼是 DNS？網路的電話簿

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.2em;">你在瀏覽器打 <strong style="color: #3498db;">google.com</strong></p>
  <p style="color: #aaa;">但電腦不懂「google.com」是什麼</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.2em;">電腦只懂<strong>數字地址</strong>：<span style="color: #3498db;">142.250.185.46</span></p>
  <p style="color: #aaa;">這叫 IP 位址——像電話號碼一樣，每台伺服器都有一組</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.2em;">
  <strong style="color: #3498db;">DNS</strong> = 一本電話簿 📒<br>
  <span style="font-size: 0.9em; color: #aaa;">把「名字」翻譯成「電話號碼」</span><br>
  <span style="font-size: 0.9em; color: #aaa;">google.com → 142.250.185.46</span>
</p>

<p class="fragment" style="margin-top: 0.5em; font-size: 1.1em; color: #f39c12;">
  沒有這本電話簿，你就算知道對方的名字也打不了電話
</p>

Note:
DNS = Domain Name System（網域名稱系統）。
我們用網址（domain name）上網，但電腦之間溝通用的是 IP 位址。
DNS 就是中間的翻譯層，把人看得懂的名字轉成電腦看得懂的數字。
沒有 DNS，你就必須記住每個網站的 IP 位址才能上網——就像沒有通訊錄就要背所有人的電話號碼。

---

### DNS 怎麼查？像打 104 查號台

<div class="fragment" style="margin-top: 0.5em;">
  <p>你的手機想找 <strong style="color: #3498db;">google.com</strong> 的電話號碼</p>
</div>

<div style="margin-top: 0.5em; max-width: 90%; margin-left: auto; margin-right: auto;">
  <div class="fragment" style="border-left: 3px solid #3498db; padding-left: 1em; margin-bottom: 0.4em;">
    <p style="margin: 0;">1️⃣ 先翻自己的通訊錄（本機快取）</p>
    <p style="margin: 0; color: #aaa; font-size: 0.85em;">之前查過就直接用，不用再問別人</p>
  </div>
  <div class="fragment" style="border-left: 3px solid #3498db; padding-left: 1em; margin-bottom: 0.4em;">
    <p style="margin: 0;">2️⃣ 沒有 → 打給 ISP 的查號台（DNS 解析器）</p>
    <p style="margin: 0; color: #aaa; font-size: 0.85em;">你的中華電信 / 台灣大 有一台專門幫你查號碼的伺服器</p>
  </div>
  <div class="fragment" style="border-left: 3px solid #3498db; padding-left: 1em; margin-bottom: 0.4em;">
    <p style="margin: 0;">3️⃣ ISP 也沒有 → 一路往上問到「總機」</p>
    <p style="margin: 0; color: #aaa; font-size: 0.85em;">Root Server → .com 管理者 → google.com 的權威伺服器</p>
  </div>
  <div class="fragment" style="border-left: 3px solid #2ecc71; padding-left: 1em;">
    <p style="margin: 0; color: #2ecc71;">4️⃣ 查到了！把結果記在通訊錄裡下次用</p>
    <p style="margin: 0; color: #aaa; font-size: 0.85em;">這就是「DNS 快取」——記住查到的結果，省得每次都打電話問</p>
  </div>
</div>

Note:
DNS 查詢的層級：
1. 本機快取（你的裝置記住之前查過的結果）
2. ISP 的 DNS 解析器（像 8.8.8.8 或中華電信的 DNS）
3. 根伺服器（Root Server）→ TLD 伺服器（管 .com 的）→ 權威伺服器（google.com 的管理者）
正常這整個流程只要幾十毫秒。
而且查到的結果會被快取在各個層級，下次再查就不用從頭問。
但——快取也有保存期限。

---

### DNS 快取也有保存期限

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.1em;">通訊錄裡的電話號碼也會「過期」</p>
  <p style="color: #aaa;">google.com 的 TTL 可能設 300 秒（5 分鐘）</p>
  <p style="color: #aaa;">某些 .tw 網站可能設 3600 秒（1 小時）</p>
</div>

<div class="fragment" style="margin-top: 1em;">
  <p style="font-size: 1.1em;">為什麼不永久記住？</p>
  <p style="color: #aaa;">因為伺服器可能搬家（換 IP）、做負載平衡、或做故障切換</p>
  <p style="color: #aaa;">如果永遠用舊號碼，可能打到空號</p>
</div>

<p class="fragment fade-up" style="margin-top: 1em; font-size: 1.1em; color: #f39c12;">
  所以 DNS 快取也有 TTL——過期就要<strong>重新查號</strong><br>
  <span style="color: #aaa; font-size: 0.9em;">平常幾十毫秒搞定，你完全不會發現</span>
</p>

Note:
DNS 記錄的 TTL 由網站管理者設定。
大型網站通常 TTL 很短（幾分鐘），因為需要經常調整流量分配。
小網站可能 TTL 較長（幾小時到一天）。
正常情況下 DNS 重新查詢很快，但在海纜事件中，
很多網站的權威 DNS 伺服器在海外——重新查詢就要走壅塞的國際連線。

---

### DNS 快取過期 = 找不到門牌號碼

<div class="fragment" style="margin-top: 0.5em;">
  <p style="font-size: 1.1em;">假設有一個網站 <strong style="color: #3498db;">service.gov.tw</strong></p>
  <p style="color: #aaa;">伺服器就在台北市內 🏢</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p>你的裝置之前查過，通訊錄裡有它的 IP</p>
  <p style="color: #2ecc71;">→ 連線正常、速度快 ✓</p>
</div>

<div class="fragment" style="margin-top: 0.8em;">
  <p>但 DNS 快取到期了——需要重新查號</p>
  <p style="color: #aaa;">權威 DNS 伺服器在哪？<span style="color: #e74c3c;">美國的 AWS Route 53</span></p>
</div>

<p class="fragment fade-up" style="margin-top: 0.8em; font-size: 1.2em; color: #e74c3c;">
  查號的電話打不通 → 你<strong>查不到門牌號碼</strong><br>
  <span style="font-size: 0.9em;">伺服器就在 10 公里外——但你找不到它</span>
</p>

<p class="fragment" style="margin-top: 0.5em; color: #aaa; font-size: 0.9em;">
  不是伺服器掛了，不是網路斷了<br>
  <strong style="color: #f39c12;">是你忘了地址，而且問不到</strong>
</p>

Note:
這是 DNS 快取過期最諷刺的場景：
一個 .tw 網站，伺服器實體在台灣，資料在台灣，完全不需要國際連線。
但它的 DNS 權威伺服器用的是 AWS Route 53（在美國）。
當你的 DNS 快取過期，需要重新查詢時，查詢請求要送到美國——
經過壅塞的海纜——然後逾時。
結果：一個完全在台灣的服務，因為 DNS 查不到而無法連線。
這就是「依賴鏈」的概念——表面上是本土服務，實際上隱藏了海外依賴。
