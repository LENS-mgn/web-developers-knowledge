### 直前のコミットを修正

```
$ git commit --amend
```


### 2つ(以上)前のコミットメッセージを変更する

```
$ git rebase --interactive HEAD~2
$ git commit --amend --no-edit
```

対象のコミットを `pick` から `edit` に変更

```
$ git rebase --continue
```


### 一番最初のコミットに戻る

```
$ git log --reverse
```

若しくは

```
$ git rev-list HEAD | tail -n 1
```

にてハッシュ値の確認が可能。


### リモートリポジトリの情報を更新

プルリクマージ後リポジトリをリモート側から削除した際に有効

```
$ git remote update origin --prune
```

### リモートリポジトリのブランチを削除

```
$ git push origin --delete branch_name
```
