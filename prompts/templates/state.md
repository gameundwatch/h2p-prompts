# state.md — 進行状態の正本（テンプレート）

`.h2p/state.md` は html-to-project の**唯一の正本**である。現在地・スコープ・
各Phaseの完了状況・重要な決定が、ここにしか恒久的に存在しない。司令塔は
起動時に必ずこれを読み、各Phaseは合意のたびにここを更新する。会話文脈が
失われても（圧縮・エージェント切替・セッション切断）、ここを読めば作業は
復元できる。

以下が初回起動時に書き込む初期テンプレートである。エージェントはこの構造を
保ったまま値を更新していく。セクションの順序・見出しは変更しないこと
（機械的に再読込するため）。

---

```markdown
# html-to-project state

## meta
- created: <ISO8601 日時>
- last_updated: <ISO8601 日時>
- current_phase: 1
- backend: undecided   # undecided | none | mock（Phase2で確定。fullは将来予約・本ツール射程外）
                       # フロントエンドは常に作る前提。分岐はバックエンドの有無のみ

## source
退避済み入力（.h2p/source/ 配下）と分析対象の確定状況。
- copied_files:
  - <例: source/index.html>
- analysis_target: undecided   # 複数HTML時、対象の確定をPhase1冒頭で行う
- notes: <複数ページか単一かなど、確定時のメモ>

## phases
各Phaseの状態。status は pending | in_progress | done | skipped のいずれか。
artifact は成果物の相対パス。done にする条件は各Phaseプロンプトが定義する。

| # | name                         | status      | artifact                              |
|---|------------------------------|-------------|---------------------------------------|
| 1 | HTML分析                      | in_progress | .h2p/phase1-analysis.md               |
| 2 | 要件定義(5W1H)・契約昇華        | pending     | .h2p/phase2-requirements.md           |
| 3 | 構造／フロー分析               | pending     | .h2p/phase3-structure.md              |
| 4 | HTMLリファクタリング           | pending     | .h2p/phase4-refactor.md, source/refactored.html |
| 5 | 技術スタック作成               | pending     | .h2p/phase5-stack.md                  |
| 6 | 開発フロー設計                 | pending     | .h2p/phase6-workflow.md               |
| 7 | HTML to Frontend実装          | pending     | frontend/                             |
| 8 | Backend実装                   | pending     | backend/, shared/                     |
| 9 | ドキュメント生成               | pending     | CLAUDE.md, README.md, documents/      |

## gates
動作検証ゲートの通過記録。進行時に司令塔が参照する。
- p4_html_behaves: not_checked   # not_checked | passed | failed
- p7_frontend_behaves: not_checked
- p8_integration_runs: not_checked   # frontend_only の場合は n/a

## decisions
重要な決定の追記ログ（時系列・末尾追記）。後戻り時の判断根拠にもなる。
各エントリは「日時・Phase・決定内容・根拠」を1行〜数行で。
- <ISO8601> P1: <例: 対象を index.html 単一に確定。other.html は素材置き場と判断>

## backlog
現Phaseでは扱わないが記録しておくべき事項（スコープ外の気づき、将来課題）。
- <なければ空>
```

---

## 運用上の規約

### current_phase と phases テーブルの整合
`current_phase` の値と、phases テーブルで `in_progress` の行は常に一致させる。
Phase完了時、司令塔は (a) その行を `done` に、(b) 次行を `in_progress` に、
(c) `current_phase` を進める、の3つを同時に更新する。

### backend の効き方
フロントエンドは常に作る。分岐はバックエンドの有無のみ。
- `backend: none` のとき、Phase8 の行を `skipped` にし、
  `gates.p8_integration_runs` を `n/a` とする。`shared/` `backend/` は作らない。
- `backend: mock` のとき、Phase8 を通常進行し、`shared/`（契約の正本）と
  `backend/`（モックサーバー）を作る。
- `backend` は Phase2 で確定するまで `undecided`。確定したら decisions に
  根拠を残す（特に外部API痕跡をCORS/キー秘匿の観点でどう判断したか）。

### gates の更新
動作検証ゲートに該当するPhase（4・7・8）の出口で検証を行い、結果を gates に
記録する。司令塔は進行指示を受けた際、該当ゲートが `passed` であることを
確認してから次へ進める。`failed` の間は進行を保留する。

### 後戻り時の扱い
ユーザーが上流Phaseへの後戻りを指示したら：
1. 戻り先Phaseの status を `in_progress`、`current_phase` を戻す。
2. 戻り先より**下流**の全Phaseを `pending` に巻き戻し、該当する gates を
   `not_checked` に戻す。
3. decisions に「いつ・どこへ・なぜ戻ったか」を追記する。
4. 下流の成果物が無効化される旨をユーザーに伝える(ファイルは消さず、
   再実行で上書きされることを明示)。

### 書き込みの原則
- last_updated は更新のたびに必ず書き換える。
- decisions と backlog は**追記**（既存行を消さない）。後の判断根拠になるため。
- このファイルは人間が読んで現在地を完全に把握できる状態を常に保つ。
