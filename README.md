# repo_template

リポジトリのテンプレートです。
日本語の文書の検査、日本語のラベル、Dependabot、GitFlowに沿ったリリースのワークフローが最初から入っています。

## 何が入っているか

| 位置 | 中身 |
| ---- | ---- |
| `.textlintrc.js`、`.markdownlint-cli2.jsonc` | 日本語の文書の検査設定です。規則は公開されている共有設定`@223n/lint-config-ja`を使います |
| `package.json` | 検査に使う道具の依存です。版もここで管理します |
| `.github/labels.yml` | IssueとPull Requestのラベルの定義です。すべて日本語です |
| `.github/labeler.yml` | Pull Requestに、変えたファイルやブランチ名からラベルを付ける規則です |
| `.github/dependabot.yml` | Dependabotの設定です。npmとGitHub Actionsを毎週まとめて更新します |
| `.github/release.yml` | GitHub Releaseの本文を自動で作るときの分類です |
| `.github/ISSUE_TEMPLATE/` | Issueのフォームです。バグ報告、機能の要望、質問の3つがあります |
| `.github/pull_request_template.md` | Pull Requestのテンプレートです |
| `.github/CODEOWNERS` | 変更の確認を求める相手です |
| `.github/workflows/` | CI、ラベルの同期、ラベル付け、リリースのワークフローです |
| `CONTRIBUTING.md` | 貢献の手引きです。ブランチの運用と文書の書き方があります |
| `SECURITY.md` | 脆弱性の報告先です |

## テンプレートから作る

1. GitHubでこのリポジトリを開き、「Use this template」から「Create a new repository」を選びます
1. 「Include all branches」にチェックを入れます。`develop`ブランチも一緒にできます
1. 作ったリポジトリで、次の「作った直後にやること」を順に行います

### 作った直後にやること

- `develop`ブランチが無ければ`main`から作ります。リリースのワークフローは`develop`を起点にします
- 「Settings」→「Actions」→「General」の「Workflow permissions」で、「Allow GitHub Actions to create and approve pull requests」を有効にします
  - リリースのワークフローがPull Requestを開くために要ります
- Actionsの「ラベルを同期する」を一度手で実行します。既定の英語のラベルが日本語に置き換わります。Dependabotのラベルもこれで付くようになります
- `.github/CODEOWNERS`の`@223n`を、自分のアカウントかチームに書き換えます
- `.github/ISSUE_TEMPLATE/config.yml`のURLにある`223n/repo_template`を、自分のリポジトリに書き換えます
- `package.json`の`name`と`description`、このREADMEを書き換えます
- 「Settings」→「Code security」で、Dependabot alertsとDependabot security updatesを有効にします（任意）
- セルフホストのランナーで動かすなら、変数`RUNS_ON`を設定します（後述）
- `main`と`develop`にブランチ保護をかけます（任意）

このリポジトリ自身をテンプレートとして使えるようにするには、「Settings」→「General」の「Template repository」にチェックを入れます。

## 日本語の文書を検査する

Markdownの書式を`markdownlint`で、日本語の書き方を`textlint`で検査します。
規則は公開されている共有設定[@223n/lint-config-ja](https://www.npmjs.com/package/@223n/lint-config-ja)にあり、このリポジトリには「何を検査するか」だけを書いてあります。
規則の理由は[node_japanese_lint_template](https://github.com/223n/node_japanese_lint_template)にあります。

```bash
npm install
npm run lint          # 書式と日本語をまとめて検査する
npm run lint:md:fix   # 書式の指摘を直す
npm run lint:ja:fix   # 日本語の指摘のうち、機械的に直せるものを直す
```

Node 22以上が要ります。

文体は「ですます調」です。
「である調」にしたい場合や、規則を一部だけ変えたい場合は、`.textlintrc.js`のコメントに書き方があります。

`main`と`develop`への`push`と、すべてのPull Requestで、CIが同じ検査をします。

## ラベル

IssueとPull Requestのラベルはすべて日本語です。
`.github/labels.yml`が定義で、「ラベルを同期する」ワークフローがリポジトリのラベルをこの内容に揃えます。
ラベルを足したり変えたりするときは、GitHubの画面ではなくこのファイルを変えてください。
ファイルに無いラベルは消えます。

| ラベル | 用途 | 誰が付けるか |
| ---- | ---- | ---- |
| バグ | 期待どおりに動かない | Issueフォーム |
| 機能追加 | 新しい機能や改善の要望 | Issueフォーム |
| ドキュメント | 文書の追加や修正 | Issueフォーム、ラベラー |
| 質問 | 使い方や仕様についての質問 | Issueフォーム |
| アクセシビリティ | 障害のある人の利用を妨げるもの | 人 |
| 重複 | すでにあるIssueやPull Requestと同じ内容 | 人 |
| 無効 | 内容が正しくない、または対象外 | 人 |
| 対応しない | 対応しないと判断したもの | 人 |
| 初心者向け | はじめて貢献する人に向く課題 | 人 |
| 助けが必要 | 手を貸してほしい課題 | 人 |
| 依存関係 | 依存パッケージやアクションの更新 | Dependabot、ラベラー |
| npm | npmパッケージの更新 | Dependabot |
| GitHub Actions | GitHub Actionsの更新 | Dependabot、ラベラー |
| リリース | リリースの準備と公開 | リリースのワークフロー、ラベラー |
| セキュリティ | 脆弱性やセキュリティに関わる修正 | 人、ラベラー |
| 破壊的変更 | 後方互換性を壊す変更 | 人 |

GitHubが最初から用意する英語のラベル（`bug`や`enhancement`など）は、付いているIssueを保ったまま日本語のラベルに改名されます。
Dependabotが作る既定のラベル（`dependencies`、`javascript`、`github_actions`）も同じように改名されます。
対応は`.github/labels.yml`の`from_name`にあります。

Pull Requestには、変えたファイルとブランチ名から`.github/labeler.yml`の規則でラベルが自動で付きます。

## Dependabot

`.github/dependabot.yml`で、npmの依存とGitHub Actionsのアクションを毎週月曜の朝に確かめます。
Pull Requestは`develop`に向けて開かれ、「依存関係」と「npm」または「GitHub Actions」のラベルが付きます。
minorとpatchの更新は1つのPull Requestにまとまり、majorの更新は個別に開かれます。

セキュリティ更新は常に既定ブランチに向けて開かれ、そこには`labels`などの設定が効きません。
すべてを`develop`に揃えたい場合は、既定ブランチを`develop`にします。

## ブランチとリリース

GitFlowに沿って運用します。
ブランチの役割は[CONTRIBUTING.md](CONTRIBUTING.md)にあります。

```text
develop ──▶ release/vX.Y.Z ──(Pull Request)──▶ main ──▶ タグ vX.Y.Z と GitHub Release ──▶ develop へ戻す
```

### リリースする

1. Actionsの「リリース」を開き、「Run workflow」を選びます
1. `version`にリリースする版を入れます。`v`は付けません（例: `1.2.0`、`1.2.0-rc.1`）
1. ワークフローが`develop`から`release/vX.Y.Z`ブランチを切り、`package.json`の版を上げ、`main`へのPull Requestを開きます
1. Pull Requestの内容を確かめてマージします
1. 「リリースを公開する」ワークフローが動き、タグ`vX.Y.Z`を打ち、GitHub Releaseを作り、`main`を`develop`に戻します

版は`package.json`の`version`で管理します。
今の版より小さい版や、すでにあるタグは受け付けません。
`-rc.1`のようなプレリリースの版は、GitHub Releaseでもプレリリースになります。

`auto_merge`を有効にして実行すると、Pull Requestを人手で確かめずにマージし、公開まで一気に進めます。
ブランチ保護で承認が要る場合はマージで止まります。その場合は人がマージすれば、公開のワークフローが続きを行います。

GitHub Releaseの本文は、マージしたPull Requestのタイトルとラベルから自動で作られます。
分類は`.github/release.yml`にあります。

## GitHub Actionsのランナー

ワークフローは既定でGitHubがホストする`ubuntu-latest`で動きます。
セルフホストのランナーがある場合は、リポジトリまたは組織の変数`RUNS_ON`に、ランナーのラベル（例: `self-hosted`）を設定します。
設定は「Settings」→「Secrets and variables」→「Actions」の「Variables」にあります。
変数が無いときは`ubuntu-latest`に倒れるため、設定しなくても動きます。

セルフホストのランナーには、`git`と`gh`（GitHub CLI）が要ります。
Nodeはワークフローが用意します。
公開リポジトリでセルフホストのランナーを使うと、フォークからのPull Requestで任意のコードが動くため、非公開のリポジトリで使ってください。

## ワークフローの一覧

| ファイル | いつ動くか | 何をするか |
| ---- | ---- | ---- |
| `ci.yml` | `main`と`develop`への`push`、Pull Request | 日本語の文書を検査します |
| `labels.yml` | `.github/labels.yml`の変更、手動 | リポジトリのラベルを定義に揃えます。Pull Requestでは差分の表示だけです |
| `labeler.yml` | Pull Requestを開いたとき、更新したとき | 変えたファイルとブランチ名からラベルを付けます |
| `release.yml` | 手動 | `develop`からリリースブランチを切り、版を上げ、`main`へのPull Requestを開きます |
| `release-publish.yml` | `release/*`のPull Requestが`main`にマージされたとき | タグを打ち、GitHub Releaseを作り、`main`を`develop`に戻します |

## ライセンス

Apache License 2.0です。[LICENSE](LICENSE)を見てください。
