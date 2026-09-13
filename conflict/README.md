# 発展課題：コンフリクトを解消する

2つの変更をrebaseで組み合わせ、両方のIssueの目的を満たすページにします。講師の開始案内後に取り組んでください。

## 役割を決める【全員】

4〜6人のteamで、実装する2人とレビューする人を決めます。**担当A・担当Bは、この演習内の役割名です。**

| 役割 | 作業 |
|---|---|
| **担当A：講座名** | [講座名と説明を追加する](https://github.com/soft03-git-workshop/workshop-template/issues/4)。先にPRをマージする。 |
| **担当B：team名** | [team名と紹介を追加する](https://github.com/soft03-git-workshop/workshop-template/issues/5)。その後、rebaseで解消する。 |
| **レビュー担当：残りの人** | 差分・プレビューの確認と、解消時の相談に参加する。 |

**A・Bはそれぞれ自分のPCで操作します。** ブランチを更新するのは本人だけです。他の人はそのブランチへpushせず、そこから別の作業も始めません。

## 1. 同じmainから始める【担当A・担当B】

リポジトリのフォルダーで実行します。WindowsはGit Bash、macOSはターミナルを使ってください。

```bash
git status
```

変更が残っていないことを確認してから、mainを更新します。

```bash
git switch main
git pull --ff-only
git rev-parse HEAD
```

**A・BのcommitのIDが同じ**で、`conflict/practice.md`の先頭行が `# 自己紹介` なら準備完了です。異なる場合や変更が残っている場合は、講師・サポートに相談してください。

## 2. それぞれPRを作る【担当A・担当B】

自分のIssueを読み、`feature/そのIssueの番号` でブランチを作ります。編集・commit・push後、PRを作り、Discordのteamチャンネルでレビューを依頼します。

**両方のPRができるまで、どちらもマージしません。** この演習中は、他のPRのマージも待ちます。

## 3. AのPRを先にマージする【レビュー担当 → 担当A】

**レビュー担当**がAのPRを確認・承認し、**担当A**がマージします。

**担当B**は自分のPRを開き、コンフリクトの表示を確認します。表示が変わらなければ再読み込みしてください。

## 4. rebaseを始める【担当B】

**ここから手順7までは、担当BのPCで操作します。** Aとレビュー担当は相談・確認に参加してください。

```bash
git branch --show-current
git status
```

ブランチが `feature/5` で、変更が残っておらず、すべてpush済みであることを確認します。

```bash
git fetch origin
git rev-parse HEAD
git rev-parse origin/feature/5
```

最後の2つのIDが同じことを確認します。違う場合は止めて相談してください。

書き換え前のIDを控えてrebaseします。**pushまで同じターミナルを使います。**

```bash
workshop_before_rebase=$(git rev-parse HEAD)
git rebase origin/main
```

`CONFLICT` や `could not apply` は、この演習では想定した表示です。状態を確認します。

```bash
git status
```

## 5. 内容を相談して直す【操作：担当B／相談：全員】

`conflict/practice.md`を開き、`<<<<<<<`、`=======`、`>>>>>>>` で囲まれた箇所を読みます。

rebaseの`HEAD`側は最新mainと適用済みの変更、もう片方は今適用中の自分のcommitです。今回の最初の衝突では、**HEAD側がAの講座名、もう片方がBのteam名**です。エディターの「現在／入力側」という表示だけで選ばず、内容を確認しましょう。

- それぞれのIssueは、読む人に何を伝えたい？
- 両方の目的を満たすには、どんな見出しと本文にすればよい？

相談した内容に書き直し、区切りの記号を削除します。

## 6. rebaseを続ける【担当B】

```bash
git add conflict/practice.md
git diff --cached --check
git diff --cached
```

`--check`で指摘が出たら、修正・`git add`・確認をやり直します。差分表示は `q` で閉じられます。

```bash
git -c core.editor=true rebase --continue
git status
```

元のcommitメッセージを使い、編集画面を開かずに続行します。**再び衝突したら手順5〜6を繰り返し、rebaseが完了してから次へ進みます。**

## 7. 確認してpushする【担当B】

ファイル全体をプレビューし、**1つの見出しで講座名とteam名が分かり、両方の説明が残っているか**確認します。ローカルでプレビューできない場合は、push後にGitHubの作業ブランチで確認してください。

```bash
git branch --show-current
git diff origin/main...HEAD
git rev-parse HEAD
echo "$workshop_before_rebase"
```

ブランチが `feature/5` であることと差分を確認し、新旧のcommitのIDを見比べます。rebaseでcommitを作り直すため、IDが変わります。

次のコマンドは、GitHub上のブランチが手順4で控えたIDのままである場合だけ、**Bの作業ブランチ**を更新します。

```bash
git push --force-with-lease="refs/heads/feature/5:$workshop_before_rebase" origin HEAD:refs/heads/feature/5
```

**IDが空欄、pushが拒否されたなどの場合は、止めて相談してください。** `--force`への変更や、IDの取り直しで押し通さないでください。

## 8. 再レビューしてマージする【担当B → レビュー担当 → 担当B】

**担当B**は同じPRの説明欄を更新し、Discordの元のスレッドで再レビューを依頼します。

- **変更内容**：何を組み合わせ、なぜその形にしたか。
- **動作確認**：プレビューで何を確認し、どう表示されたか。

**レビュー担当**は両方のIssue・差分・プレビューを確認し、必要な修正を終えたら改めて承認します。**担当B**がマージします。

最後に、**全員**がmainを更新して完成したファイルを確認します。

```bash
git switch main
git pull --ff-only
```

## ヒント：別の方法も調べてみる【全員】

コンフリクトは、rebaseのほかに**mergeでも解消できます**。**「merge rebase 違い」**で検索してみてください。

## 困ったとき

`git status`で状態を確認し、画面を残して講師・サポートに相談してください。

**担当B**がrebaseを途中で中止する場合は、次を使います。今回の解消のための編集も取り消されます。

```bash
git rebase --abort
```
