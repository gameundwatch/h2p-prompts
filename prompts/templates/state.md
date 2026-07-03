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
- input_type: undecided   # undecided | A | B（起動時に司令塔が判別・記録）
- backend: undecided   # undecided | none | mock（Phase2で確定。fullは将来予約・本ツール射程外）
                       # フロントエンドは常に作る前提。分岐はバックエンドの有無のみ

## source
入力の退避・作業コピーの状況と、分析対象の確定状況。input_type により記載が変わる。
- **タイプA**：退避済み入力（.h2p/source/ 配下）を列挙する。
  - copied_files:
    - <例: source/index.html>
- **タイプB**：退避はしない。作業コピーの状況を記録する。
  - working_copy: <例: ルートへ作業コピー済み（node_modules/.git 除外）、
    依存インストール済み、起動確認済み。origin/ は凍結>
- analysis_target: undecided   # 対象ファイル群と「1つのアプリである」ことの確認記録
- notes: <複数ページ構成か単一かなど、確定時のメモ>

## phases
各Phaseの状態。status は pending | in_progress | done | skipped のいずれか。
artifact は成果物の相対パス。done にする条件は各Phaseプロンプトが定義する。

| # | name                                   | status      | artifact                              |
|---|----------------------------------------|-------------|---------------------------------------|
| 1 | 分析（A:観測 / B:棚卸し・診断）           | in_progress | .h2p/phase1-analysis.md（挙動チェックリスト含む） |
| 2 | 要件（A:意図遡及 / B:将来意図）・契約昇華  | pending     | .h2p/phase2-requirements.md, .h2p/ubiquitous.md |
| 3 | 構造（A:構造付与 / B:再設計・移行計画）    | pending     | .h2p/phase3-structure.md              |
| 4 | 構造リファクタリング                      | pending     | .h2p/phase4-refactor.md, source/refactored/(A) |
| 5 | 技術スタック作成                          | pending     | .h2p/phase5-stack.md                  |
| 6 | 開発フロー設計                            | pending     | .h2p/phase6-workflow.md               |
| 7 | Frontend実装/移行                        | pending     | frontend/                             |
| 8 | Backend実装                              | pending     | backend/, shared/                     |
| 9 | ドキュメント生成                          | pending     | CLAUDE.md, README.md, documents/      |

## gates
動作検証ゲートの通過記録。進行時に司令塔が参照する。passed にする際は
通過時コミットのハッシュを添える（例: `passed (commit a1b2c3d)`）。
- p4_html_behaves: not_checked   # not_checked | passed (commit <hash>) | failed
- p7_frontend_behaves: not_checked
- p8_integration_runs: not_checked   # backend: none の場合は n/a

## approved_deviations
検証ゲートで見つかった原本との差異のうち、**ユーザーが明示的に許容したもの**の
記録（追記式・既存行を消さない）。各エントリは「Phase・差異の内容・許容の理由」を
1行〜数行で。記録なき黙認は第一原則違反である。
- <なければ空>

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

### git 規律
h2p は git 操作込みの環境構築ワークフローである。
- 初回起動時、作業フォルダが git リポジトリでなければ司令塔が `git init` する。
- **動作検証ゲート（P4/P7/P8）の通過時は必ずコミット**し（メッセージは
  `h2p: P4 gate passed` のような `h2p: ` プレフィックスの定型）、ハッシュを
  gates に記録する。
- **タイプBの移行ステップは1ステップ＝1コミット。**
- `.h2p/`・`.h2p-archive/`・`prompts/` は **git で追跡する**（.gitignore に
  入れない）。移行の意思決定の記録を履歴に残すため。ignore するのは
  `node_modules` 等の生成物だけ。

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
記録する。検証は Phase1 の挙動チェックリストを照合対象とし、視覚・操作の
最終確認はユーザーが行う。ユーザーが許容した差異は `approved_deviations` に
記録してから passed にする。司令塔は進行指示を受けた際、該当ゲートが
`passed` であることを確認してから次へ進める。`failed` の間は進行を保留する。

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
- decisions・backlog・approved_deviations は**追記**（既存行を消さない）。
  後の判断根拠になるため。
- このファイルは人間が読んで現在地を完全に把握できる状態を常に保つ。
