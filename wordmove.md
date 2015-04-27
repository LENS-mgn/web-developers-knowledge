[WordMove](https://github.com/welaika/wordmove) を利用します。
vccw に WordMove 本体や設定ファイルが純正で含まれているので、デプロイ作業は基本的にゲストマシンから行います。
wordmove コマンドを叩くのは `/vagrant` になります。

# 主要なコマンド 
## wordmove push

    Options:
      -w, [--wordpress], [--no-wordpress]  
      -u, [--uploads], [--no-uploads]      
      -t, [--themes], [--no-themes]        
      -p, [--plugins], [--no-plugins]      
      -l, [--languages], [--no-languages]  
      -d, [--db], [--no-db]                
      -v, [--verbose], [--no-verbose]      
      -s, [--simulate], [--no-simulate]    
      -e, [--environment=ENVIRONMENT]      
      -c, [--config=CONFIG]                
          [--no-adapt], [--no-no-adapt]    
          [--all], [--no-all]              

    Pushes WP data from local machine to remote host

## wordmove pull

    Options:
      -w, [--wordpress], [--no-wordpress]  
      -u, [--uploads], [--no-uploads]      
      -t, [--themes], [--no-themes]        
      -p, [--plugins], [--no-plugins]      
      -l, [--languages], [--no-languages]  
      -d, [--db], [--no-db]                
      -v, [--verbose], [--no-verbose]      
      -s, [--simulate], [--no-simulate]    
      -e, [--environment=ENVIRONMENT]      
      -c, [--config=CONFIG]                
          [--no-adapt], [--no-no-adapt]    
          [--all], [--no-all]              

    Pulls WP data from remote host to the local machine

# メモ
* aws の ec2-user には WordPress ファイルの編集権限が無い為、所有者を ngix から ec2-user に変更、その他実行権限などを変更します。詳しくは以下の関連リンクから。
* siteurl や home は Movefile の記述に従って自動で書き換え
* DB を含めた push / pull で wp-content 内に .sql が生成される
* DB の同期は`ローカルー>ステージ` よりも `ステージ->ローカル` を推奨
* DB の変更を伴う作業の場合はステージング環境で行った後、プロジェクトメンバーが各自 `wordmove pull -d` で同期する

# 関連リンク
* [WordMoveを使ってVagrant内のWordPressと本番環境を同期する！](https://firegoby.jp/archives/5644)
* [ssh-agentを使ってVagrant上のゲストOSからMac側の秘密鍵を使えるようにする](https://firegoby.jp/archives/5694)
* [amimotoのnginxまわりのユーザーとかをごにょごにょ](https://gist.github.com/miya0001/cabf03ef768ba7f9ba7d)

# データベースの pull や push でコケる場合
エラーメッセージの例

    ▬▬ ✓ Pulling Database ▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬
        local | mysqldump --host=localhost --user=wordpress --password=wordpress wordpress > /var/www/wordpress/wp-content/local-backup-0000000000.sql
       remote | mysqldump --host=localhost --user=xxxxxx '--password=xxxxxx' i_xxxxxx > /var/www/vhosts/i-xxxxxx/wp-content/dump.sql
       remote | get: /var/www/vhosts/i-xxxxxx/wp-content/dump.sql /var/www/wordpress/wp-content/dump.sql
       remote | delete: /var/www/vhosts/i-xxxxxx/wp-content/dump.sql
        local | adapt dump
    /usr/local/rbenv/versions/2.1.2/lib/ruby/gems/2.1.0/gems/wordmove-1.2.0/lib/wordmove/sql_adapter.rb:44:in `gsub!': invalid byte sequence in US-ASCII (ArgumentError)
      from /usr/local/rbenv/versions/2.1.2/lib/ruby/gems/2.1.0/gems/wordmove-1.2.0/lib/wordmove/sql_adapter.rb:44:in `serialized_replace!'
      from /usr/local/rbenv/versions/2.1.2/lib/ruby/gems/2.1.0/gems/wordmove-1.2.0/lib/wordmove/sql_adapter.rb:36:in `replace_field!'
      from /usr/local/rbenv/versions/2.1.2/lib/ruby/gems/2.1.0/gems/wordmove-1.2.0/lib/wordmove/sql_adapter.rb:25:in `replace_vhost!'
      from /usr/local/rbenv/versions/2.1.2/lib/ruby/gems/2.1.0/gems/wordmove-1.2.0/lib/wordmove/sql_adapter.rb:17:in `adapt!'
      from /usr/local/rbenv/versions/2.1.2/lib/ruby/gems/2.1.0/gems/wordmove-1.2.0/lib/wordmove/deployer/base.rb:182:in `adapt_sql'
      from /usr/local/rbenv/versions/2.1.2/lib/ruby/gems/2.1.0/gems/wordmove-1.2.0/lib/wordmove/deployer/ssh.rb:35:in `pull_db'
      from /usr/local/rbenv/versions/2.1.2/lib/ruby/gems/2.1.0/gems/wordmove-1.2.0/lib/wordmove/cli.rb:47:in `block in pull'
      from /usr/local/rbenv/versions/2.1.2/lib/ruby/gems/2.1.0/gems/wordmove-1.2.0/lib/wordmove/cli.rb:34:in `block in handle_options'
      from /usr/local/rbenv/versions/2.1.2/lib/ruby/gems/2.1.0/gems/wordmove-1.2.0/lib/wordmove/cli.rb:32:in `each'
      from /usr/local/rbenv/versions/2.1.2/lib/ruby/gems/2.1.0/gems/wordmove-1.2.0/lib/wordmove/cli.rb:32:in `handle_options'
      from /usr/local/rbenv/versions/2.1.2/lib/ruby/gems/2.1.0/gems/wordmove-1.2.0/lib/wordmove/cli.rb:46:in `pull'
      from /usr/local/rbenv/versions/2.1.2/lib/ruby/gems/2.1.0/gems/thor-0.19.1/lib/thor/command.rb:27:in `run'
      from /usr/local/rbenv/versions/2.1.2/lib/ruby/gems/2.1.0/gems/thor-0.19.1/lib/thor/invocation.rb:126:in `invoke_command'
      from /usr/local/rbenv/versions/2.1.2/lib/ruby/gems/2.1.0/gems/thor-0.19.1/lib/thor.rb:359:in `dispatch'
      from /usr/local/rbenv/versions/2.1.2/lib/ruby/gems/2.1.0/gems/thor-0.19.1/lib/thor/base.rb:440:in `start'
      from /usr/local/rbenv/versions/2.1.2/lib/ruby/gems/2.1.0/gems/wordmove-1.2.0/bin/wordmove:6:in `<top (required)>'
      from /usr/local/rbenv/versions/2.1.2/bin/wordmove:23:in `load'
      from /usr/local/rbenv/versions/2.1.2/bin/wordmove:23:in `<main>'

[Problem local database when pull #78](https://github.com/welaika/wordmove/issues/78#issuecomment-55882636)
に従って以下のファイルを修正

    /usr/local/rbenv/versions/2.1.2/lib/ruby/gems/2.1.0/gems/wordmove-1.2.0/lib/wordmove/sql_adapter.sb

Line 44 right before `sql_content.gsub!...`, add `sql_content.force_encoding("UTF-8")`.
