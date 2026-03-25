### 模擬回顧

<div style="font-size: 0.75em; margin-top: 0.5em;">

| 階段 | 你的體驗 | 可能的原因 |
|:---:|:---|:---|
| 0–5 分 | VoIP 通話斷線 | BGP reconvergence + 通話控制伺服器在日本 | 
| 5–30 分 | 全部變慢、卡住 | 壅塞崩潰 + 長號效應 | 
| 30–60 分 | 逐一斷線、登出 | 快取 TTL 到期、Token 過期、DNS 失效 | 
| 1–6 時 | 雲端後台全掛 | 控制平面依賴海外 | 

</div>

Note:
這張投影片是整場演講的「全景回顧」。
用表格幫聽眾建立完整的因果鏈：體驗 → 技術原因 → 決策者。
重點是讓聽眾看到：沒有任何一個故障是「自然現象」，每一個都是某個組織的工程或政策選擇。
表格刻意簡化，因為前面已經詳細講過每一個階段。

---

### 做完模擬了

<div class="fragment" style="margin-top: 0.8em;">
  <p style="font-size: 1.1em;">各個利害關係人</p>
  <p style="color: #aaa;">需要做一次<strong>斷網演習</strong></p>
</div>

---

### 🤔 留給大家的問題

<p style="font-size: 1.4em; margin-top: 0.8em;">
  在剛剛的情境下<br>
  <strong style="color: #f39c12;">政府網站 gov.tw</strong><br>
  撐得住嗎？
</p>

<p style="font-size: 1.4em; margin-top: 0.8em;">
  目前<br>
  <strong style="color: #f39c12;">海底電纜還好嗎</strong><br>
</p>


<p class="fragment fade-up" style="font-size: 1.2em; margin-top: 1em;">
  接下來的講者會帶大家<strong style="color: #2ecc71;">一起研究</strong>
</p>

Note:
這是銜接下一位講者的橋樑投影片。
把問題丟給聽眾，讓他們用剛剛學到的分析框架去思考：gov.tw 到底撐不撐得住？
四個問題分別對應四個階段的技術概念：DNS、認證、CDN、控制平面。
不給答案——讓聽眾帶著好奇心進入下半場。
這也是對整場演講的最佳驗證：聽眾是否真的學會了分析方法？