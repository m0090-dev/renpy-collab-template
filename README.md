# 共同制作テンプレート（Ren'Py・完成形）

AI エージェントと一緒に複数人でゲームを作るための、リポジトリの土台。
Ren'Py の雛形・公開ワークフロー・共同作業の文書が、最初から所定の場所に
配置済みの「完成形」テンプレ。取り込みや配置換えの作業はない。

## 使い方

### テンプレ側（1回だけ）

1. この一式を GitHub のリポジトリに置く。
2. そのリポジトリの Settings → General →「Template repository」にチェックを入れる。

### プロジェクト側（作品ごとに）

1. テンプレリポの「Use this template」→「Create a new repository」で新しいリポを作る
   （ファイルだけがコピーされ、履歴は引き継がれない）。
2. `docs/PROJECT.md` を埋める（プロジェクト名、役割分担、公開 URL）。
   ここが唯一の「プロジェクト固有の入口」で、他の文書はここを参照する。
3. リポの Settings を2か所だけ設定する:
   - Pages → Build and deployment → Source を「Deploy from a branch」にし、
     ブランチは `gh-pages`、フォルダは `/ (root)` を選ぶ
     （`gh-pages` ブランチは初回のワークフロー実行で自動作成されるので、
     その後に選ぶ）。
   - Actions → General → Workflow permissions を「Read and write permissions」にする。
4. main の保護を入れる（Settings → Rules / ブランチ保護）:
   マージには PR が必要、force push 禁止、ブランチ削除禁止、自動チェック必須。
5. 相手を collaborator として招待する。各人のエージェントに GitHub 連携をさせ、
   連携アプリの対象リポジトリにこのリポを追加する
   （詳しくは `docs/START_HERE.md` の「GitHub 連携の前提」）。
6. あとは開始の一言で作業開始（下記）。

### 注意

- テンプレから作ったリポには、テンプレ側の後の更新は自動で入らない。
  必要な変更は手動で取り込む（プロジェクトごとに独立する、という性質でもある）。

## 作業の始め方（どのプロジェクトでも同じ）

自分の AI にこう言うだけ:

> 「このリポジトリを参照して、`docs/START_HERE.md` を見てから作業できる状態にして」

AI は `PROJECT.md` を読み、役割の選択肢を提示し、役割に合ったメモを読んで、
最新の main から作業ブランチを作って「準備できた」と報告する。

## 構成

- `game/` — Ren'Py 標準 GUI が土台の雛形（日本語フォント同梱・指定済み、サンプル台詞入り）
- `.github/workflows/` — Web ビルドとブランチ別公開（main はルート、作業ブランチは `/<ブランチ名>/`）、ブランチ削除時の掃除
- `.github/ISSUE_TEMPLATE/` — リソース依頼・要望の Issue テンプレ（依頼の窓口）
- `docs/PROJECT.md` — プロジェクト固有の設定（最初に埋める）
- `docs/START_HERE.md` — 全チャット共通の開始手順（マスター）。GitHub 連携の前提もここ
- `docs/RULES.md` — 共通ルールの核（プロジェクト固有の追記は PROJECT.md 側へ）
- `docs/NOTES-renpy.md` — Ren'Py 固有の注意（実測ベース）
- `docs/FOR_PARTNER.md` / `docs/GUIDE_FOR_PARTNER.md` — パートナー（演出担当）向けの配慮メモと平易な説明
- `docs/roles/` — 役割メモ（演出担当／リソース・管理担当／雛形）
- `docs/PARTNER_NOTES_TEMPLATE.md` — パートナー向け配慮メモの雛形
- `tools/make_dummies.py` — ダミー素材の生成
- `assets_manifest.txt` — 必要素材リスト（リソース担当の TODO）
