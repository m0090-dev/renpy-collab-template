# Ren'Py の注意書き

このテンプレに同梱されている Ren'Py まわり一式と、固有の注意（実測ベース）。

## 同梱されているもの

- `.github/workflows/pages.yml` — Web ビルド → GitHub Pages 公開（実証済みの版）。
  main への push はルート `/` へ、作業ブランチへの push は `/<ブランチ名>/` へ公開する。
  公開先は gh-pages ブランチ（Pages の Source を「Deploy from a branch: gh-pages / root」に設定）。
- `.github/workflows/cleanup-preview.yml` — ブランチ削除時に、そのブランチの公開フォルダを消す。
- `game/` — Ren'Py 標準 GUI を土台にした雛形（`gui.rpy` / `screens.rpy` 入り。
  メニュー・セーブ・ロード画面が最初からある）。台詞は日本語のサンプル。
- `tools/make_dummies.py` — 本番と同名・同寸法のダミー画像生成（Pillow 依存）。
- `assets_manifest.txt` — 必要素材リストの雛形（= リソース担当の TODO）。

## ワークフローの注意（実測で踏んだもの）

- SDK と成果物は必ずリポジトリの外（`$RUNNER_TEMP`）に置く。プロジェクトフォルダ内に
  置くと、配布ビルドが SDK ごと `game.zip` に詰め込み、100MB を超えて push できなくなる。
  （`game.zip` が 90MB を超えたら異常として止めるガード入り）
- Web ビルドのプロジェクト指定はリポジトリのルートにする。`game/` フォルダを直接
  指定すると、公開版が `renpyAudio is not defined` で起動しない。
- ブランチ名は英小文字とハイフンのみ（公開 URL のフォルダ名になるため）。

## Ren'Py 固有の注意（実測ベース）

- 日本語フォントはプロジェクトに同梱する。雛形では `game/fonts/` に置き、
  `gui.rpy` の `gui.text_font` / `gui.name_text_font` / `gui.interface_text_font` で指定済み。
  デフォルトフォントには日本語グリフがなく、環境によって豆腐（□）になる。
- Web 版の更新がブラウザで反映されないときは、Service Worker のキャッシュを疑う。
  通常のキャッシュ削除では消えないことがあるので、開発者ツールの
  Application → Storage →「Clear site data」で消す（シークレットウィンドウでは素通りする）。
- Web 版で動画スプライトは使える（Movie displayable）。ただしブラウザのコーデック依存で、
  Safari 向けに同名の .mp4 フォールバックを用意する。透過は side_mask 方式。
- アニメ GIF は非対応（静止画として読まれる）。ATL のフレーム列か WebM に変換する。
- Web 版はブラウザのサンドボックス内なので、実ファイルや OS には触れない。
  メタ演出は作中の仮想環境として再現する形になる。
- CLI: `lint` と `compile` はヘッドレス可。`test`（自動テストケース実行）は
  環境によっては終了しないことがあるので、CI や実機での確認と役割分担する。
- Pages の上限（サイト全体 1GB）が実質の制約。ダミー運用と組み合わせる。
