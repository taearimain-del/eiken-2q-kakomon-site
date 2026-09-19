# CLAUDE.md — 英検2級 過去問プレイヤー

英検2級の過去問（Listening・Speaking音声）を再生し、リスニングをマークシート形式で
自己採点するためのプロジェクト。準1級過去問プレイヤーと同じ構成。
（応答スタイル・環境・エンコーディング等の共通ルールは親の `CLAUDE 作業場所/CLAUDE.md` および
`~/.claude/CLAUDE.md` を参照。ここには本プロジェクト固有の情報のみ書く）

## Gitリポジトリについて（重要）
- このフォルダは `claude-workspace`（親リポジトリ）とは別の**独立したGitリポジトリ**
  （2026-09-19作成。準1級過去問プレイヤーと同じ理由＝音声ファイル・公開範囲の分離）。
  親リポの `.gitignore` にこのフォルダを追加済み。
- GitHub: `taearimain-del/eiken-2q-kakomon-player`（**Private**、コード管理用の本体）。
- `taearimain-del/eiken-2q-kakomon-site`（**Public**、GitHub Pages公開専用）を用意。
  同じローカルフォルダから2つのリモート（`origin`=Private本体、`pages`=Public公開用）に
  push する構成。作業時は両方 `git push origin main` / `git push pages main` を忘れないこと。

## データについて
- 教材：英検2級 過去問（2022年度第3回〜2024年度第2回、計6回分）
- 元データ：`C:\Users\Fort_\Downloads\2q_*.zip`（Listening用・Speaking用(`s`付き)がラウンドごとに2本）
- 構成：`audio/<年度>_<回>/` 配下に `2kyu_<年度>_<回>_Listening_XX_...mp3` /
  `..._Speaking_XX_...mp3` を展開。Listening 32トラック + Speaking 9トラック × 6回。
  2級のListeningは第1部(dai1bu)15問・第2部(dai2bu)15問の2部構成（準1級のPart3に相当するものはない）。
- `tracks.json` / `tracks.js` は `build-tracks.mjs` で自動生成（手編集しない）。将来ラウンドを
  追加する場合は `audio/<年度>_<回>/` に同じ命名でmp3を置き、`build-tracks.mjs` の `ROUNDS`
  配列に追記して `node build-tracks.mjs` を再実行する。

## マークシート機能
- `listening-marksheet.html` … 音声を聞きながら選択肢をクリックしてマーク、正答を
  カンマ区切りで入力すると自己採点できる。ロジックは `common/listening-engine.js`。
- 筆記（リーディング）マークシート・Writing添削プロンプト作成ツールは
  `claude-workspace/英検マーク式演習/2級/` 側にある（このリポジトリとは別管理）。

## 著作権について（要注意）
英検の過去問音声は英検協会が学習者向けに配布しているものだが、第三者への再配布・公開を
どこまで許容しているかの利用規約は未確認。個人の学習目的での非公開URL運用を前提とし、
サイト内で検索エンジンにインデックスされないようにする（`robots.txt` で全体禁止、
`<meta name="robots" content="noindex">` をindex.htmlに設定）。SNS等でURLを公開しない。
もし利用規約上グレーだと分かった場合は速やかに非公開化を検討する。

## デプロイ（Netlify）
- 本番URL：https://eiken-2q-kakomon.netlify.app
- 使用アカウント：`ryu20001208.jp@gmail.com`（チーム `ryu20001208-jp`）。サイト名 `eiken-2q-kakomon`。
- `access-gate.js`（`my-portal-ryu.netlify.app`でホスト）によるパスワード/Googleログインで
  保護済み。`robots.txt` と `<meta name="robots" content="noindex,nofollow">` で検索エンジンからは隠している。
- 再デプロイ手順：このフォルダ（`英検2級過去問プレイヤー/`）で `netlify deploy --prod --dir=.` を実行するだけ。
  `.netlify/state.json` にsiteIdが設定済みなのでログイン・サイト指定は不要
  （ただしCLIのログインアカウントが `ryu20001208.jp@gmail.com` になっている必要がある）。
- 注意：ワークスペースのルート（`CLAUDE 作業場所/`）には別のNetlifyサイト
  （`claude-workspace-ryuao`）がリンクされている。Netlify関連のコマンドは必ずこのフォルダ内で
  実行し、ルートでは実行しないこと。
