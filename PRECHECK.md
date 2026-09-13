# 準備チェック

## 1. 使える道具を確認する

WindowsはGit Bash、macOSはターミナルで、1行ずつ実行します。Git Bash自体が見つからない場合は、講師・サポートを呼んでください。

```bash
git --version
gh --version
gh auth status
```

GitとGitHub CLIのバージョンが表示され、GitHub.comで **自分のアカウントが有効** になっていることを確認します。バージョン番号が資料の画像と完全に同じである必要はありません。

| 状況 | 対応 |
|---|---|
| Gitまたはghが見つからない | 既存Zenn Bookの12章「事前準備」を使い、サポートと導入する |
| 導入済みなのに見つからない | ターミナルとVS Codeを終了して開き直す |
| ghにログインしていない | `gh auth login`でGitHub.com、HTTPS、ブラウザでのログインを選ぶ。Gitの認証を尋ねられたらYes |
| 違うアカウントになっている | サポートと一緒に、ブラウザとghのアカウントを確認する |
| インストールに管理者の承認が必要 | 学校のPC管理ルールに従い、先生・サポートに相談する |

認証コードやトークンは、リポジトリや班の共有画面へ貼り付けません。

## 2. 班のリポジトリを手元に用意する

講師が共有する一覧から **自分の班** を開きます。Organizationへの招待を承諾し、自分の班への参加処理が済んでいるか確認します。

GitHubの **Code → HTTPS** からURLをコピーします。作業を置きたいフォルダに移動し、次のURL部分を自分の班のものに置き換えます。

```bash
git clone https://github.com/ORGANIZATION/team-01.git
cd team-01
git remote -v
git status
```

上のOrganization名と班番号は例です。フォルダ名も、自分がcloneしたリポジトリ名に合わせます。`git remote -v`に自分の班のURLが表示されることを確認してください。

**publicリポジトリをcloneできても、pushできる権限があるとは限りません。** 最初の課題で自分のブランチをpushしたところまで確認します。権限エラーはサポートに知らせてください。

## 3. この演習の作者名とメールを設定する

cloneしたフォルダ内で行います。GitHubの **Settings → Emails** でメール非公開設定を確認し、GitHubが表示する自分用の `noreply` メールアドレスをコピーしてください。例のアドレスをそのまま使わないでください。

```bash
git config --local user.name "公開用のニックネーム"
git config --local user.email "GitHubが表示する自分用のnoreplyメール"
git config user.name
git config user.email
```

この設定は演習用リポジトリだけに適用されます。設定を変更しても、過去のcommitの作者情報は変わりません。

最後にVS Codeの「フォルダーを開く」でこのフォルダを開きます。Windowsの内蔵ターミナルはGit Bashを選びます。Zenn Bookの12章に画面付きの案内があります。

参考：[GitHub公式・commitメールの設定](https://docs.github.com/en/account-and-profile/how-tos/email-preferences/setting-your-commit-email-address)
