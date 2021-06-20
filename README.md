# React x Rails API モードのDocker環境構築初期設定

## 1. リポジトリのClone

```
$ git clone git@github.com:DaisukeShinoku/setting_react_rails.git
```

## 2. Railsのビルド

```
$ docker-compose run api rails new . --force --no-deps --database=postgresql --api
$ docker-compose build
```

## 3. Reactの作成

```
$ docker-compose run --rm front sh -c "npm install -g create-react-app && create-react-app . --template typescript"
```

## 4. DB作成

### (1) api/config/database.ymlに必要な設定を追記

```
~
~
default: &default
  adapter: postgresql
  encoding: unicode
  # For details on connection pooling, see Rails configuration guide
  # https://guides.rubyonrails.org/configuring.html#database-pooling
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>

  ### 下記を追記
  host: db
  username: postgres
  passowrd:
  ### 上記を追記

development:
  <<: *default
  database: docker_qiita_oniki_development
~
~
```

### (2) Docker立ち上げ(ターミナルを一つ占有する)

```
$ docker-compose up
```

### (3) DBを作成する

上記の作業と別のターミナルを立ち上げて行う

```
$ docker-compose exec api rails db:create
```

この時点で下記のURLでそれぞれのデフォルトページが表示されることを確認する
* Rails : `http://localhost:3000/`
* React : `http://localhost:8000/`
