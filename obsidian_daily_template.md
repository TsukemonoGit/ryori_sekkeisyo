<%*  
/*  
@template: daily-cooking  
@plugin: templater  
  
@trigger:  
- Daily Note作成時に適用  
  
@behavior:  
- 曜日に応じて内容を切替  
- 今日の料理を表示  
- 明日の準備を1件表示  
*/  
_%>
<%*

const day = Number(tp.date.now("d"))

const list = [
  {
    name: "Sun",
    title: "🍛 一掃（煮る）",
    next: "鶏むね下味冷凍・ラベル「月」",
    action: "煮る",
    rule: "カレー",
    note: "全部入れる",
    time: "15分"
  },
  {
    name: "Mon",
    title: "🍗 鶏肉（焼く）",
    next: "パスタ・ベーコン確認",
    action: "焼く",
    rule: "甘辛",
    note: "フライパン12分",
    time: "12分"
  },
  {
    name: "Tue",
    title: "🍝 麺（茹でて和える）",
    next: "豚肉 冷蔵へ移動（必要時）",
    action: "茹でて和える",
    rule: "にんにく油",
    note: "パスタ or うどん",
    time: "15分"
  },
  {
    name: "Wed",
    title: "🥓 豚肉（炒める）",
    next: "魚 左手前へ移動",
    action: "炒める",
    rule: "塩",
    note: "肉→野菜",
    time: "8分"
  },
  {
    name: "Thu",
    title: "🐟 魚（焼く）",
    next: "ひき肉 冷蔵へ移動",
    action: "焼く",
    rule: "バター＋醤油",
    note: "凍ったまま",
    time: "12分"
  },
  {
    name: "Fri",
    title: "🥩 ひき肉（炒める）",
    next: "干物 左手前へ移動・日曜具材確認",
    action: "炒める",
    rule: "甘辛",
    note: "加熱前に混ぜる",
    time: "10分"
  },
  {
    name: "Sat",
    title: "🐠 干物（焼く）",
    next: "余り食材リスト化",
    action: "焼く",
    rule: "不要",
    note: "弱火",
    time: "10分"
  }
]

const d = list[day]
_%>

# 🗓 <% tp.date.now("YYYY-MM-DD") %>（<% d.name %>）

## 今日の料理
- メニュー: <% d.title %>
- 操作: <% d.action %>
- 味: <% d.rule %>

## 明日の準備
- [ ] <% d.next %>

## 実行チェック
- ⏱ 目安: <% d.time %>
- [ ] 食材確認
- [ ] 器具準備
- [ ] タイマー設定
- [ ] 調理

## 補充候補
- [ ] 

## 今日の一言
- 

## メモ
- <% d.note %>

---

## 判断ルール
- 余り2種以上 → 煮る
- 10分以内 → 焼く
- 野菜あり → 炒める
- その他 → 麺

---

## 副菜
- [ ] 作り置き使用
- [ ] なければ省略

---

## エラー処理
- 味薄い → 醤油 小さじ1
- 水多い → 強火1分
- 火不足 → +2分
- 面倒 → 焼く
