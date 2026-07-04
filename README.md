# はないわーるど まるごと記録帖

「はないわーるど」の配信ノート（スプレッドシート）を集計・可視化した、単一HTMLファイルのファンダッシュボードです。

- グラフ、五十音リアクションマップ、全話アーカイブ（検索・絞り込み・詳細モーダル）を収録。
- **Excelファイル連携**: `はないわーるど.xlsx` を `index.html` と同じ場所（このリポジトリのルート）に置いておくと、ページを開くたびにそのExcelファイルを読み込んで内容を反映します。**Excelファイルを更新してpushするだけで、サイトの表示も最新化されます。**
- 読み込みに失敗した場合（ファイルが見つからない、ローカルで直接開いた場合など）は、`index.html` 生成時点のデータにフォールバックするので、壊れることはありません。

## Excel連携について

ページ最上部に取得状況が表示されます。

- `✅ 最新のExcelデータを反映しました` … `はないわーるど.xlsx` を正しく読み込んで表示しています
- `⚠️ Excelファイルを取得できなかったため、保存済みのデータを表示しています` … ファイルが見つからない、または読み込みに失敗したため、`index.html` 生成時点のスナップショットを表示しています

### 更新の仕方

1. `はないわーるど.xlsx` を編集する（**行を追加するだけならこれで完了**）
2. リポジトリの同じファイルを上書きしてpush
3. Cloudflare Pagesが自動で再デプロイし、サイトを開くと最新のExcel内容が反映されます

### 注意点

- ファイル名は **`はないわーるど.xlsx`** から変更しないでください（変更する場合はClaudeに伝えてください。`index.html` 内の設定を1箇所直すだけで対応できます）
- 6つのシート名（`全まとめ` / `今週のアイスまとめ` / `叶えたい！キャラ一覧` / `リアクション統計` / `知ってるカナ点数計算` / `コーナー出現分布`）は変更しないでください
- `全まとめ`シートは**列の並び順を変えても大丈夫**です（列名で読み取っているため）。ただし列名自体を変える場合はClaudeに伝えてください
- それ以外の5シートは列の位置で読み取っているため、列の並び順を変えると自動反映が失敗します（その場合も壊れず、保存済みデータの表示にフォールバックします）
- **ローカルで `index.html` を直接ダブルクリックして開いた場合、ブラウザの制限によりExcelファイルの読み込みはできません**（保存済みデータが表示されます）。これはCloudflare Pagesなど実際にサーバー経由で配信されている場合は問題なく動作します

## ローカルで確認する

`index.html` をブラウザで直接開くだけで表示できます。ビルドは不要です。

```
open index.html      # macOS
start index.html      # Windows
```

## GitHubに置く

まだGitHubリポジトリを作っていない場合は、[github.com/new](https://github.com/new) で新しいリポジトリを作成してください（Public / Private どちらでも可）。

このフォルダ（`index.html`、`はないわーるど.xlsx`、`README.md`）をリポジトリのルートに置いて、以下のコマンドでpushします。

```bash
git init
git add .
git commit -m "はないわーるど記録帖を追加"
git branch -M main
git remote add origin https://github.com/<あなたのユーザー名>/<リポジトリ名>.git
git push -u origin main
```

すでにリポジトリがある場合は、`index.html` と `はないわーるど.xlsx` を**同じフォルダ**にコピーして、通常どおり `git add` → `git commit` → `git push` してください（2つのファイルは必ず同じ場所に置いてください）。

## Cloudflare Pagesで公開する

1. [Cloudflare Dashboard](https://dash.cloudflare.com/) にログイン
2. 左メニューの **Workers & Pages** → **Create** → **Pages** タブ → **Connect to Git**
3. さきほどpushしたGitHubリポジトリを選択
4. ビルド設定は以下の通り（静的HTMLなのでビルド不要）
   - **Framework preset**: `None`
   - **Build command**: 空欄のまま
   - **Build output directory**: `/` （`index.html` を置いたフォルダ。サブフォルダに置いた場合はそのパスを指定）
5. **Save and Deploy** をクリック

数十秒でデプロイが完了し、`https://<プロジェクト名>.pages.dev` のようなURLが発行されます。以降はmainブランチにpushするたびに自動で再デプロイされます。

### 独自ドメインを使いたい場合

Cloudflareでドメインを管理していれば、Pagesプロジェクトの **Custom domains** タブから追加するだけで、DNS設定も自動で行われます。

## OGP（Twitter/LINE/Discordなどでのリンクカード表示）

`index.html` にOGP・Twitter Card用のメタタグを追加済みです。リンクを貼った際に `hanaiworld_database.jpg` の画像付きカードが表示されます。

**デプロイ後、必ず以下の対応をしてください:**

`index.html` 内の `https://REPLACE-WITH-YOUR-DOMAIN/` を、実際に公開されるURL（例: `https://your-project.pages.dev/`）に置き換えてください。対象は以下の4箇所です。

```html
<meta property="og:image" content="https://REPLACE-WITH-YOUR-DOMAIN/hanaiworld_database.jpg">
<meta property="og:url" content="https://REPLACE-WITH-YOUR-DOMAIN/">
...
<meta name="twitter:image" content="https://REPLACE-WITH-YOUR-DOMAIN/hanaiworld_database.jpg">
```

OGP画像はURLが絶対パスでないと多くのSNSで正しく表示されないため、この置き換えは必須です。ドメインが決まったら教えていただければ、こちらで直したものをお渡しすることもできます。

置き換えたら、[Twitter Card Validator](https://cards-dev.twitter.com/validator) や [Facebookシェアデバッガー](https://developers.facebook.com/tools/debug/) で表示確認ができます。

## ファイル構成

```
.
├── index.html                 # HTML構造のみ
├── style.css                  # 全スタイル
├── app.js                     # データ・グラフ描画・検索などのロジック
├── vendor/
│   ├── chart.min.js           # グラフ描画ライブラリ（Chart.js）
│   └── xlsx.min.js            # Excel読み込みライブラリ（SheetJS）
├── はないわーるど.xlsx         # データ本体（更新はこのファイルを編集するだけ）
├── hanaiworld_database.jpg    # OGP用のカード画像
└── README.md                  # このファイル
```

すべてのファイルを**同じ構成のまま**リポジトリに置いてください。`index.html`は`style.css`・`app.js`・`vendor/`内の2ファイルを相対パスで読み込んでいます。

## 構造的な変更をしたい場合

シート構成やデザインそのものを変えたい場合は、更新後のxlsxをClaudeに渡して `index.html` を作り直してもらってください。

