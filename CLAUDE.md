# Claude マスター道場 — CLAUDE.md

## プロジェクト概要

`claude-training-game/` は Claude / Claude Code / GitHub の使い方を  
クイズ形式で学べるブラウザゲームです（フレームワーク不使用、HTML+CSS+JS のみ）。

## 質問をクイズ化するルール

**このプロジェクトを開いているときにユーザーが Claude Code の使い方・機能・操作に  
関する質問をした場合、以下の手順を自動で実行すること。**

### 手順

1. **質問に答える**（通常通り）

2. **クイズ問題を作成する**  
   以下のフォーマットで問題を作る：
   ```js
   { q: '質問内容を問題文に変換',
     choices: ['誤答A', '正答B', '誤答C', '誤答D'],
     correct: 1,  // 正解のインデックス（0〜3）
     explanation: '正解！説明文。' }
   ```
   - 選択肢は4択
   - 正解は毎回同じ位置でなくランダムな位置に配置する
   - 説明文は「正解！」で始め、なぜそれが正しいかを簡潔に説明する

3. **適切なミッションに追加する**  
   `game.js` の `MISSIONS` 配列の中から、最も関連性の高いミッションの  
   `questions` 配列末尾に追加する。
   
   | テーマ | 対象ミッション |
   |--------|--------------|
   | セッション・基本操作 | Mission 1 |
   | ファイル・資料の渡し方 | Mission 2 |
   | Claude の概要・得意不得意 | Mission 3〜4 |
   | プロンプト・指示の書き方 | Mission 5〜7 |
   | Claude Code の特徴 | Mission 8 |
   | スキル（/コマンド） | Mission 9 |
   | フック・自動化 | Mission 10 |
   | CLAUDE.md | Mission 11 |
   | MCP・外部連携 | Mission 12 |
   | GitHub 全般 | Mission 13〜18 |

4. **README.md のミッション一覧を更新する**  
   問題数が変わった場合は README.md の該当ミッション行のメモも更新する。

### 例

ユーザーが「`/compact` って何ですか？」と聞いた場合：

1. `/compact` の説明を返す
2. 以下のような問題を Mission 1 などに追加する：
   ```js
   { q: 'Claude Code で会話が長くなってきたとき、コンテキストを圧縮して続けるコマンドは？',
     choices: ['/compress', '/clear', '/compact', '/reset'],
     correct: 2,
     explanation: '正解！/compact を実行すると会話の履歴を要約・圧縮して、長いセッションでもコンテキスト上限を気にせず続けられます。' }
   ```

## Mission 0「全体マップ」の自動メンテナンスルール

**Claude の新機能・新製品・新プラン（Claude.ai / Claude Code / Claude for Work / デザイン支援など）について会話したとき、以下を自動で実行すること。**

### 手順

1. **質問に答える**（通常通り）

2. **Mission 0 の `knowledge` を更新する**
   - 既存の `term` に該当する内容なら、`desc` を加筆・修正する
   - 新しいカテゴリの機能なら、新しい `{ term, desc }` オブジェクトを `knowledge` 配列に追加する

3. **Mission 0 の `questions` にクイズを追加する**
   - 新機能・新製品に関する4択問題を1問追加する
   - フォーマットは通常のクイズ追加ルールと同じ

4. **README.md の Mission 0 行を更新する**（問題数が変わった場合）

### 対象となる話題の例

| 話題 | 対応 |
|------|------|
| Claude の新モデル（Opus/Sonnet/Haiku など）| knowledge 更新 + クイズ追加 |
| Claude Code の新機能（hooks・MCP など） | knowledge 更新 + クイズ追加 |
| Claude for Work の新機能 | knowledge 更新 + クイズ追加 |
| Claude の画像・デザイン機能の変化 | knowledge 更新 + クイズ追加 |
| 新しいプラン・料金体系 | knowledge 更新 |

---

## 問題ランダム出題について

`game.js` の `startMission()` 内で `shuffle([...currentMission.questions])` を  
呼び出しているため、ミッションを開始するたびに問題の順番が変わります。  
新しく追加した問題も自動的にランダム出題の対象になります。

## ファイル構成

```
claude-training-game/
├── index.html   # 画面レイアウト
├── style.css    # デザイン
├── game.js      # ゲームロジック・ミッションデータ（問題はここに追加）
├── README.md    # ゲーム説明
└── CLAUDE.md    # このファイル（Claude へのルール）
```

## コーディング規約

- フレームワーク・ライブラリは使わない（純粋な HTML/CSS/JS）
- `localStorage` の `claudeGame` キーに状態を保存している
- 新ミッション追加時は `MISSIONS` 配列に追加し、README.md のミッション一覧も更新する
