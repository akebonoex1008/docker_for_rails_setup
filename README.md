# docker　でRailsの簡単環境設定

### 1
アプリケーションを作るディレクトリにREADME.md以外のファイルをコピーして作業するディレクトリで 'docker-compose run web rails new . --force --database=postgresql --skip-bundle' を実行。

### ２
Railsのファイルが出来上がったら、Gemfileに好きなgemを入れたりして編集する。

### 3
下記のように 'config/database.yml' を書き換える。 

        default: &default
        adapter: postgresql
        encoding: unicode
        host: db
        username: postgres
        password: password
        pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
        
        development:
        <<: *default
        database: myapp_development

        test:
        <<: *default
        database: myapp_test

        production:
        <<: *default
        database: myapp_production
        username: myapp
        password: <%= ENV['MYAPP_DATABASE_PASSWORD'] %>

### 4
 'docker-compose build' でimageを作成。

 ### 5
 'docker-compose run web rake db:create' でDBを作成

 ### 6
 最後に 'docker-compose up -d' をして 'http://localhost:3000/'　にアクセスしてRailsのデフォルト画面が表示されればOK
