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

const noteDate = new Date(tp.file.title + "T00:00:00")
const day = Number.isNaN(noteDate.getTime()) ? new Date().getDay() : noteDate.getDay()
const prepHeading = day === 0 ? "仕込み" : "明日の準備"

const list = [
  {
    name: "Sun",
    title: "🍛 一掃（煮る）",
    next: "買い物→下味冷凍→配置整理",
    shopping: [
      "鶏むね肉",
      "ベーコン",
      "豚こま肉",
      "魚切り身",
      "干物",
      "野菜2種",
      "カレールー",
      "バター",
      "米",
      "パスタ or 冷凍うどん"
    ],
    prep: [
      "鶏むね肉を下味冷凍し、ラベル「月」を貼る",
      "魚・干物を冷凍庫左手前へ配置",
      "野菜室を整理する",
      "日曜カレーの具材リストを確認する"
    ],
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
    next: "水曜の豚肉を冷蔵へ移動（冷凍の場合）",
    action: "茹でて和える",
    rule: "にんにく油",
    note: "パスタ or うどん",
    time: "15分"
  },
  {
    name: "Wed",
    title: "🥓 豚肉（炒める）",
    next: "木曜の魚を冷凍庫左手前へ移動",
    action: "炒める",
    rule: "塩",
    note: "肉→野菜",
    time: "8分"
  },
  {
    name: "Thu",
    title: "🐟 魚（焼く）",
    next: "金曜のひき肉を冷蔵へ移動（解凍開始）",
    action: "焼く",
    rule: "バター＋醤油",
    note: "凍ったまま",
    time: "12分"
  },
  {
    name: "Fri",
    title: "🥩 ひき肉（炒める）",
    next: "土曜の干物を冷凍庫左手前へ移動・日曜カレー具材確認",
    action: "炒める",
    rule: "甘辛",
    note: "加熱前に混ぜる",
    time: "10分"
  },
  {
    name: "Sat",
    title: "🐠 干物（焼く）",
    next: "日曜カレーの具材リスト作成（冷蔵庫・冷凍庫確認）",
    action: "焼く",
    rule: "不要",
    note: "弱火",
    time: "10分"
  }
]

const d = list[day]
_%>

# 🗓 <% tp.file.title %>（<% d.name %>）

## 今日の料理
- メニュー: <% d.title %>
- 操作: <% d.action %>
- 味: <% d.rule %>

## <% prepHeading %>
- [ ] <% d.next %>

<%* if (day === 0) { %>
## 買い物
<%* d.shopping.forEach(item => { %>
- [ ] <% item %><%* }) %>

## 下準備
<%* d.prep.forEach(item => { %>
- [ ] <% item %><%* }) %>
<%* } %>

## 実行チェック
- ⏱ 目安: <% d.time %>
- [ ] 食材確認
- [ ] 器具準備
- [ ] タイマー設定
- [ ] 調理

## メモ
- <% d.note %>

> [!INFO]- 参考: 運用ルール・副菜・エラー処理
> 
> ## 判断ルール
> - 余り2種以上 → 煮る
> - 10分以内 → 焼く
> - 野菜あり → 炒める
> - その他 → 麺
> 
> ## 副菜
> - [ ] 作り置き使用
> - [ ] なければ省略
> 
> ## エラー処理
> - 味薄い → 醤油 小さじ1
> - 水多い → 強火1分
> - 火不足 → +2分
> - 面倒 → 焼く
