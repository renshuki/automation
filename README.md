## This Tutorial is work in progress
#### Latest Update 2015/07/31

## Requirements
* Mac
* Homebrew ( It's very useful because we can install packages with Command Line Interface ! )
* Virtualbox ( Install via Homebrew ( I say later. ) )
* Vagrant ( Install via Homebrew ( I say later. ) )
* Ansible ( Install via Homebrew ( I say later. ) )

## Basic Setup
### ( Tools, Packages )
#### Install Homebrew
```sh
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

#### Update Homebrew
```sh
$ brew update && brew upgrade
```

#### Install Brewdler
```sh
$ brew tap Homebrew/brewdler
```

#### Edit ~/.brewfile
```sh
# Inside of ~/.brewfile

# for installing Applications. such as Chrome, Virtualbox, etc.
tap 'caskroom/cask'
brew 'caskroom/cask/brew-cask'

# for managing packages you want with ~/.brewfile file
tap 'homebrew/brewdler'

# for installing Nokogiri.
brew 'libiconv'
brew 'libxml2'

# for installing multiple Ruby versions.
brew 'rbenv'
brew 'ruby-build'

# for managing virtual envronment. ( Infrastracture as Code )
brew 'ansible'
cask 'vagrant'
cask 'virtualbox'

# for Automating browser's behavior. ( WIP )
cask 'firefox'

# for Git Flow
brew 'git'
brew 'git-flow'
```

#### Install tools & packages
```sh
$ cd
$ brew brewdle
```
![Screen Shot 2015-07-28 at 4.01.46 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/homebrew.png)


### ( Ruby )
#### Edit ~/.bashrc
```sh
# Add this line at the bottom of ~/.bashrc
eval "$(rbenv init -)"
```

#### Enable rbenv commands
```sh
$ exec $SHELL -l
```

#### Check if rbenv commands are enabled
```sh
$ rbenv
```
![Screen Shot 2015-07-28 at 4.18.26 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/rbenv.png)

#### Install Latest Ruby version ( It was 2.1.6 when this document was created. )
```sh
$ rbenv install 2.1.6
```

#### Enable to use Ruby 2.1.6 everywhere in mac
```sh
$ rbenv global 2.1.6
$ rbenv rehash
```

#### Check if Ruby 2.1.6 is enabled to use everywhere in mac
```sh
$ ruby -v
```

### ( Gem ) - Ruby libraries
#### Update gem system
```sh
$ gem update --system
$ rbenv rehash
```

### ( NodeJS ) - Rails needs NodeJS
#### Install latest Node.js
```sh
$ git clone https://github.com/riywo/ndenv ~/.ndenv
$ echo 'export PATH="$HOME/.ndenv/bin:$PATH"' >> ~/.bashrc
$ echo 'eval "$(ndenv init -)"' >> ~/.bashrc
$ exec $SHELL -l
$ git clone https://github.com/riywo/node-build.git ~/.ndenv/plugins/node-build
$ ndenv install v0.12.0
$ ndenv global v0.12.0
$ ndenv rehash
```

### ( GitHub )
#### Create Repository at GitHub ( I named 'taskapp' for this tutorial )
![Screen Shot 2015-07-28 at 5.24.33 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/create_repo_on_gh.png)


### Setting up for this project
```sh
# Create Directory for this project.
$ mkdir ~/recipe
$ cd ~/recipe
# Install gems for this project.
$ bundle init # Gemfile will be created automatically.
$ vim ~/recipe/Gemfile
```
```ruby
# Inside of Gemfile
source 'https://rubygems.org'
gem 'rails'
```
```sh
# Install latest Rails
$ bundle install --path .bundle
```
#### Setup is done. Let's take a break ! ( ^_^)／□☆□＼(^-^ )


## Rails アプリ(taskapp)の開発
### 準備
#### rails new
```sh
$ cd ~/recipe
$ bundle exec rails new taskapp -T -B
# -T オプションはユニットテストを入れないために付けます
# -B オプションはひな形生成時にgemをインストールしないために付けます
```

#### git-flowを導入する
```sh
$ git-flow init # 質問はすべてEnterで大丈夫です
```

#### 最初のコミット
```sh
$ git add .
$ git commit -am 'Initial Commit'
```
#### GitHubへプッシュする
```sh
$ git remote add origin https://github.com/GitHubのアカウント名/taskapp.git
$ git push --all
```
![Screen Shot 2015-07-28 at 5.32.51 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/push_to_gh.png)

### 開発手順
今回のレシピでは基本的に以下の流れで開発します
1. 「Issueを作る」
2. 「featureブランチを作る」
3. 「テストを書く」（Gemの編集など、テストする必要がない時はテストは省略します）
4. 「テストが失敗する」
5. 「ソースコードを編集する」
6. 「テストが通過する」
7. 「featureブランチをコミットする」
8. 「featureブランチをGitHubにプッシュする」
9. 「featureブランチからdevelopブランチへプルリクエストを作る」
10. 「featureブランチをdevelopブランチにマージする」
11. 「developブランチをmasterブランチにマージする」

### Gemfile（最小の構成）
#### GitHubにissueを追加
![create_issue.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/add_issue_on_gh.png)

#### git-flow でfeatureブランチを作る
```sh
$ git-flow feature start setup
```

#### Gemfileを編集する
```ruby
# ~/recipe/taskapp/Gemfileの中身
source 'https://rubygems.org'
gem 'rails'
gem 'sqlite3'
gem 'sass-rails'
gem 'uglifier'
gem 'coffee-rails'
gem 'jquery-rails'
gem 'turbolinks'
```

#### Gemをインストールする
```sh
# ~/recipe/taskapp/.bundle にインストールします
$ bundle install --path .bundle
```
![Screen Shot 2015-07-28 at 5.02.06 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/bundle.png)

#### 変更をコミットする
```sh
$ git add .
$ git commit -am 'close #1'
```

#### GitHubにプッシュする ( git-flow形式で)
```sh
$ git flow feature publish setup
```

#### プルリクエストを作る
![create pull request.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/create_pr.png)
###### 「base」がマージする先のブランチで「compare」がマージする元のブランチです
###### ここでは「base」がdevelopブランチで「compare」がfeatureブランチです

#### プルリクエストをdevelopブランチにマージする
###### マージ前
![merge pull request from feature:setup.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/merge_pr.png)

###### マージ後
![merged pullrequest from feature:setup.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/merged_pr.png)

#### developブランチをmasterブランチにマージする
##### 上記と同様にdevelopブランチをmasterブランチにマージします

#### Issueがclosedになっていることを確認する
![Screen Shot 2015-07-28 at 6.04.42 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/issue_closed.png)
###### masterブランチへのマージが完了するとIssueが自動的にクローズされます( closed が 1 になっています)

### テスト駆動開発に必要なGemを入れる
#### 最新のdevelopブランチをGitHubから取得する
```sh
$ cd ~/recipe/taskapp
$ git checkout develop
$ git pull origin develop
```
![Screen Shot 2015-07-28 at 6.36.37 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/pull_latest.png)

#### Issueを作る
title: テスト駆動開発に必要なGemを入れる
というように、一目でわかるのが良いでしょう。

#### featureブランチを作る
```sh
$ git flow feature start install_tdd_gems
```

#### Gemfileを編集する
```ruby
# Gemfileに以下を追記
group :development, :test do
  gem 'guard-rspec' # ファイルの変更を監視して自動的にテストを実行します
  gem 'rspec-rails' # Ruby on Railsで非常に人気のあるテストフレームワークです
end

group :test do
  gem 'factory_girl_rails' # テスト用のデータを作ります
  gem 'database_cleaner'   # テストを実行するたびにデータベースをクリーンアップします
  gem 'faker'              # サンプルデータを自動的に生成してくれます(名前やメールアドレス等)
  gem 'capybara'           # ブラウザでの動作確認を自動化するときに使います
  gem 'launchy'            # テスト実行時に好きなタイミングで現在の画面表示をブラウザで開いてくれます
  gem 'shoulda-matchers'   # データベースのバリデーション(名前がからの時はエラーを返すなど)をカンタンにテストできます
  gem 'selenium-webdriver' # Firefox や Chrome など、テスト対象のブラウザをカンタンに設定できます
  gem 'simplecov', require: false # どのソースコードがテスト済みかを綺麗に表示してくれます
end
```

#### Gemをインストールする
```sh
$ bundle install --path .bundle
```

#### コミットする
```sh
$ git add .
$ git commit -am 'close #2' # 番号はIssueの番号を入れます
```

#### GitHubにプッシュする
```sh
$ git flow feature publish install_tdd_gems
```

#### develop, masterブランチにマージしてIssueを閉じる
省略します

#### 以下の手順ではfeatureブランチを作ったりGitHubとのやりとりの説明は省略します。
### RSpecの設定
#### RSpecをインストールする
```sh
$ bundle exec rails g rspec:install
```
![Screen Shot 2015-07-28 at 7.20.54 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/install_rspec.png)

#### .rspecを編集
```sh
# .rspecの中身
--color
--format documentation
--require spec_helper
```

### RSpecを実行する
```sh
$ bundle exec rspec spec
```
![Screen Shot 2015-07-28 at 7.26.33 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/exec_rspec.png)

### Guardの設定
#### Guardをインストールする
```sh
$ bundle exec guard init rspec
```
![Screen Shot 2015-07-28 at 7.31.40 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/install_guard.png)

### Guardを実行する
```sh
$ bundle exec guard
```
![Screen Shot 2015-07-28 at 7.34.52 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/exec_guard.png)

この状態でEnterを押すとテストが実行されます（まだ何もないです）

![Screen Shot 2015-07-28 at 7.37.27 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/no_result.png)

### テスト自動化の設定
#### spec/rails_helper.rbを編集
```ruby
# require 'rspec/rails'の下に追加
require 'capybara/rspec'
Selenium::WebDriver::Firefox::Binary.path = '/opt/homebrew-cask/Caskroom/Firefox/38.0.1/Firefox.app/Contents/MacOS/firefox' # Homebrew経由でFirefoxをインストールした場合はこの行を追加

RSpec.configure do |config|
  config.fixture_path = "#{::Rails.root}/spec/factories" # fixtures を factories へ変更
...
  # ここから
  config.before(:suite) do
    begin
      DatabaseCleaner.start
      FactoryGirl.lint
    ensure
      DatabaseCleaner.clean
    end
  end

  config.before(:each) do
    DatabaseCleaner.start
    FactoryGirl.reload
  end

  config.after(:each) do
    DatabaseCleaner.clean
  end
  config.include Capybara::DSL
  config.include FactoryGirl::Syntax::Methods
  # ここまでを追加
end
```
```sh
$ bundle exec rake db:migrate
```
#### ブラウザ動作確認用テストのディレクトリを作る
```sh
$ mkdir spec/features
```

#### いよいよアプリを実際に作っていきます

### Home画面を作る
#### Guardを起動する
```sh
# 別のウィンドウで起動してください
$ bundle exec guard
```

#### Homeコントローラーを作る
```sh
$ bundle exec rails g controller Home
```

### ブラウザでの動作を書く
##### 「/」にアクセスしたら「welcome」と表示される
ということをテストします。
```sh
$ vim ~/recipe/taskapp/spec/features/toppage_spec.rb
```
```sh
# toppage_spec.rbの中身
require 'rails_helper'

RSpec.describe 'TOP', type: :feature, js: true do
  before(:all) do
    Capybara.current_driver = :selenium # Firefoxを使用
  end

  context 'shows' do
    it 'welcome message' do
      visit root_path # 「/」にアクセスしたら
      expect(page).to have_text('welcome') # 「welcome」と表示される
    end
  end

  after(:all) do
    Capybara.use_default_driver
  end
end
```
#### toppage_spec.rbを保存するとこのようになります(左側がGuardで右側がソースコードです)
![Screen Shot 2015-07-28 at 8.36.16 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/test_result.png)

##### config/routes.rbを編集します
```ruby
Rails.application.routes.draw do
  root 'home#welcome'
end
```
##### app/controllers/home_controller.rbを編集します
```ruby
class HomeController < ApplicationController
  def welcome
    @msg = 'welcome'
  end
end
```
##### app/views/home/welcome.html.erbを編集します
```ruby
<p><%= @msg %></p>
```
toppage_spec.rbを再度保存します(中身は変わってないです)
#### Guardの画面を見るとテストを通過していることが確認できます
![Screen Shot 2015-07-28 at 8.52.43 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/check_result.png)

##### (追記) spec/controllers/home_controller.rbのテストはこんなイメージです。
```ruby
require 'rails_helper'

RSpec.describe HomeController, type: :controller do
  context 'GET #welcome' do
    it 'shows welcome message' do
      get :welcome
      expect(assigns(:msg)).to eq('welcome') # @msg ＝ 'welcome' をテストしています
    end
  end
end
```

#### ソースコードをテストがカバーしているか表示する
```ruby
# ~/recipe/taskapp/spec/spec_helper.rbを編集
# ここから
require 'simplecov'

SimpleCov.profiles.define 'filtering' do
  load_profile 'rails'
  add_filter '.bundle'
end
SimpleCov.start 'filtering'
# ここまでを追加

RSpec.configure do |config|
...
end
```

#### テストを実行する度にcoverageディレクトリの中身が更新されます
```sh
$ bundle exec rspec spec
$ open coverage/index.html
```
![Screen Shot 2015-07-28 at 9.09.03 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/coverage.png)

### Production環境で実行する
#### シークレットキーを作成
```sh
$ bundle exec rake secret
```
![Screen Shot 2015-07-28 at 10.12.58 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/secret.png)
#### ~/.bashrcの末尾にシークレットキーを追加
```sh
# ~/.bashrc
...
export SECRET_KEY_BASE='生成された文字列'
```

#### 静的ファイルをコンパイルします
```sh
$ bundle exec rake assets:precompile
$ echo '/public/assets' >> .gitignore # assetsをgitに入れないようにします
```

#### Production環境で実行します。
```sh
$ RAILS_SERVE_STATIC_FILES=true bundle exec rails s -e production
```

### 仮想環境での実行までの事前準備 ( Rails側 )

#### Unicornを導入
##### Gemfileを編集します
```ruby
group :production do
  gem 'unicorn'
end
```
##### bundle install
```sh
$ bundle install --path .bundle
```

##### config/unicorn.rbを編集
```ruby
# unicorn.rbの中身
preload_app true
timeout 60
worker_processes 4

app_path = File.dirname(File.dirname(Dir.pwd))
working_directory "#{app_path}/current"

listen "#{app_path}/shared/tmp/sockets/unicorn.sock", backlog: 64
pid "#{app_path}/shared/tmp/pids/unicorn.pid"

stderr_path "#{app_path}/shared/log/unicorn.stderr.log"
stdout_path "#{app_path}/shared/log/unicorn.stdout.log"

GC.respond_to?(:copy_on_write_friendly=) &&
  GC.copy_on_write_friendly = true
check_client_connection false

run_once = true

before_exec do |server|
  ENV['BUNDLE_GEMFILE'] = "#{app_path}/current/Gemfile"
end

before_fork do |server, worker|
  defined?(ActiveRecord::Base) &&
    ActiveRecord::Base.connection.disconnect!

  run_once = false if run_once

  old_pid = "#{server.config[:pid]}.oldbin"
  if File.exist?(old_pid) && server.pid != old_pid
    begin
      sig = (worker.nr + 1) >= server.worker_processes ? :QUIT : :TTOU
      Process.kill(sig, File.read(old_pid).to_i)
    rescue Errno::ENOENT, Errno::ESRCH => e
      logger.error e
    end
  end
end

after_fork do |server, worker|
  defined?(ActiveRecord::Base) &&
    ActiveRecord::Base.establish_connection
end
```

#### Capistranoを導入
##### Gemfile を編集します
```ruby
group :development do
  gem 'capistrano'
  gem 'capistrano-rails',   require: false
  gem 'capistrano-bundler', require: false
  gem 'capistrano-rbenv',   require: false
  gem 'capistrano3-unicorn'
end
```

##### bundle install
```sh
$ bundle install --path .bundle
```

##### Capistranoをインストールする
```sh
$ bundle exec cap install
```
![Screen Shot 2015-07-28 at 10.45.05 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/install_capistrano.png)

##### Capfileを以下のように編集します
```ruby
require 'capistrano/setup'
require 'capistrano/deploy'
require 'capistrano/rails'
require 'capistrano/rbenv'
require 'capistrano3/unicorn'

set :rbenv_custom_path, '/home/vagrant/.rbenv'
set :rbenv_ruby, '2.1.6'
set :deploy_to, '/home/vagrant/taskapp'
set :scm, :git

Dir.glob('lib/capistrano/tasks/*.rake').each { |r| import r }
```

##### config/deploy.rbを編集します
```ruby
set :application, 'taskapp'
set :repo_url, 'https://github.com/GitHubのアカウント名/taskapp.git'
set :branch, 'develop'
set :deploy_to, '/home/vagrant/taskapp'

set :keep_releases, 1
set :rbenv_type, :user
set :rails_env, 'production'
set :rbenv_ruby, '2.1.6'
set :rbenv_map_bins, %w(rake gem bundle ruby rails)
set :rbenv_roles, :all
set :linked_dirs, %w(bin log tmp/pids tmp/cache tmp/sockets vendor/bundle public/system public/assets)

set :unicorn_pid, -> { "#{shared_path}/tmp/pids/unicorn.pid" }
set :unicorn_config_path, 'config/unicorn.rb'
set :unicorn_rack_env, 'production'
```

##### config/deploy/production.rbを編集します
```ruby
set :stage, :production
set :rails_env, 'production'
set :default_env, 'SECRET_KEY_BASE' => ENV['SECRET_KEY_BASE']
key_path = '~/.ssh/taskapp'
server '127.0.0.1', port: 2201,
                    roles: %w(web app db),
                    user: 'vagrant',
                    ssh_options: {
                      user: 'vagrant',
                      keys: key_path,
                      auth_methods: %w(publickey)
                    }

after 'deploy:publishing', 'deploy:restart'
namespace :deploy do
  task :restart do
    invoke 'unicorn:restart'
  end
end
```
Rails側は以上です。
次はVagrant + Ansibleで設定していきます。

## 仮想環境構築
### 準備
#### 仮想環境のためのディレクトリを用意する
```sh
$ mkdir ~/recipe/taskapp_vm
```

#### 仮想環境専用の公開鍵と秘密鍵を用意する
```sh
$ ssh-keygen -t rsa
# パスフレーズには何も入力せずEnterを押してください
# 名前は「taskapp」とします
```
![Screen Shot 2015-07-28 at 11.42.02 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/keys.png)

#### 秘密鍵の権限を400に設定します
```sh
$ chmod 400 ~/.ssh/taskapp
```

### Nginx
#### Nginx設定ファイルを用意する
```sh
$ vim ~/recipe/nginx
```
```c
upstream unicorn {
  server unix:/home/vagrant/taskapp/shared/tmp/sockets/unicorn.sock;
}

server {
  listen 80;
  server_name localhost;

  access_log /var/log/nginx/taskapp_access.log;
  error_log /var/log/nginx/taskapp_error.log;

  root /home/vagrant/taskapp/current/public;

  client_max_body_size 4G;
  error_page 404 /404.html;
  error_page 500 502 503 504 /500.html;
  try_files $uri/index.html $uri @unicorn;

  location @unicorn {
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_pass http://unicorn;
  }
}
```

### Vagrant
#### Ubuntu最新版をダウンロードする
```sh
$ vagrant box add ubuntu/trusty64
```

#### Vagrantfileを作成する
```sh
$ cd ~/recipe/taskapp_vm
$ vagrant init # ~/recipe/taskapp_vm/Vagrantfile が生成されます
```

#### Vagrantfileを編集します
```sh
$ vim ~/recipe/taskapp_vm/Vagrantfile
```
```ruby
Vagrant.configure(2) do |config|
  config.vm.box = 'ubuntu/trusty64'

  config.vm.network 'forwarded_port', guest: 80, host: 8080
  config.vm.network 'public_network', bridge: 'en0: Wi-Fi (AirPort)'

  config.vm.provider 'virtualbox' do |vb|
    vb.gui = false
    vb.memory = '2048'
  end

  config.vm.provision 'ansible' do |ansible|
    ansible.playbook = 'provisioning/playbook.yml'
  end
end
```

### Ansible
#### ansible.cfgを編集します
```sh
$ vim ~/recipe/taskapp_vm/ansible.cfg
```
```sh
# ansible.cfgの中身
[ssh_connection]
scp_if_ssh=True
```
#### playbook.ymlを編集します
```sh
$ mkdir ~/recipe/taskapp_vm/provisioning
$ vim ~/recipe/taskapp_vm/provisioning/playbook.yml
```
```yml
---
- hosts: all
  user: vagrant
  sudo: true

  vars:
    ruby_version: 2.1.6
    home: /home/vagrant

  tasks:
    - name: update apt cache
      apt: update_cache=yes # パッケージをアップデートします

    - name: install required packages
      apt: name={{ item }} state=present # パッケージをインストールします
      with_items:
        - build-essential
        - git
        - nodejs
        - language-pack-ja-base
        - language-pack-ja

    - name: use Japanese
      sudo: yes
      lineinfile: >
        dest='{{ home }}/.profile'
        line="{{ item.line }}" # /home/vagrant/.profile に以下の内容を追加します
      with_items:
        - { line: 'export LC_MESSAGES=ja_JP.UTF-8' }
        - { line: 'export LC_IDENTIFICATION=ja_JP.UTF-8' }
        - { line: 'export LC_COLLATE=ja_JP.UTF-8' }
        - { line: 'export LANG=ja_JP.UTF-8' }
        - { line: 'export LC_MEASUREMENT=ja_JP.UTF-8' }
        - { line: 'export LC_CTYPE=ja_JP.UTF-8' }
        - { line: 'export LC_TIME=ja_JP.UTF-8' }
        - { line: 'export LC_NAME=ja_JP.UTF-8' }

    - name: install nginx
      apt: pkg=nginx state=installed update_cache=yes # Nginx をインストールします
      notify:
        - start nginx

    - name: Setup Nginx Conf for unicorn
      copy: src=~/recipe/nginx dest=/etc/nginx/sites-available/default # Mac内の~/recipe/nginxファイルをUbuntu内の/etc/nginx/sites-available/defaultにコピーします

    - name: Restart Nginx
      service: name=nginx state=restarted # Nginx を再起動します

    - name: Check Public Key
      stat: path=/home/vagrant/.ssh/taskapp.pub # /home/vagrant/.ssh/taskapp.pub ファイルが存在するかチェックします
      register: deploy_key # ファイルが存在すれば True, 存在しなければ False を代入します

    - name: Send Public Key to VM
      sudo: yes
      copy: src=~/.ssh/taskapp.pub dest={{ home }}/.ssh/ # Mac内の~/.ssh/taskapp.pub ファイルをUbuntu内の/home/vagrant/.ssh/にコピーします
      when: deploy_key.stat.exists == False # deploy_key がFalseの時にこのタスクを実行します

    - name: Copy Public Key to authorized_keys for vagrant
      sudo: no
      shell: bash -lc "cat {{ home }}/.ssh/taskapp.pub >> {{ home }}/.ssh/authorized_keys" # vagrantユーザーでログインして公開鍵の内容をauthorized_keysに追加します
      when: deploy_key.stat.exists == False # deploy_key がFalseの時にこのタスクを実行します

    - name: Check rbenv exists
      sudo: no
      stat: path={{ home }}/.rbenv
      register: rbenv

    - name: Install rbenv
      sudo: yes
      git: repo=https://github.com/sstephenson/rbenv.git dest={{ home }}/.rbenv
      when: rbenv.stat.exists == False

    - name: Export PATH
      sudo: yes
      lineinfile: >
        dest='{{ home }}/.profile'
        line='export PATH=$HOME/.rbenv/bin:$PATH'
      when: rbenv.stat.exists == False

    - name: Eval rbenv init
      sudo: yes
      lineinfile: >
        dest='{{ home }}/.profile'
        line='eval "$(rbenv init -)"'
      when: rbenv.stat.exists == False

    - name: Install dependencies
      apt: name={{ item }} state=latest
      with_items:
        - libssl-dev
        - libyaml-dev
        - libreadline6-dev
        - zlib1g-dev
        - libsqlite3-dev
        - sqlite3
      when: rbenv.stat.exists == False

    - name: change owner, group of .rbenv dir
      file: path='{{ home }}/.rbenv' owner=vagrant group=vagrant
      when: rbenv.stat.exists == False

    - name: Check ruby-build
      stat: path={{ home }}/.rbenv/plugins/ruby-build
      register: ruby_build

    - name: Install ruby-build
      sudo: yes
      git: repo=https://github.com/sstephenson/ruby-build.git dest={{ home }}/.rbenv/plugins/ruby-build
      when: ruby_build.stat.exists == False

    - name: source bashrc
      sudo: no
      shell: bash -lc "source {{ home }}/.profile"
      when: rbenv.stat.exists == False

    - name: Installed rubies
      sudo: no
      shell: bash -lc "rbenv versions | grep {{ ruby_version }}"
      register: installed
      failed_when: installed.rc not in [0, 1]

    - name: Install Ruby specific version
      sudo: no
      shell: bash -lc "rbenv install {{ ruby_version }}"
      when: installed|failed

    - name: Set Ruby specific version as global
      sudo: no
      shell: bash -lc "rbenv global {{ ruby_version }}"

    - name: Update Gem System
      sudo: no
      shell: bash -lc "gem update --system"

    - name: Install Bundler
      sudo: no
      shell: bash -lc "gem install bundler"

  handlers:
    - name: start nginx
      service: name=nginx state=started
```

### 仮想環境を立ち上げます
```sh
$ vagrant up
```

## Capistranoを使ってデプロイ
```sh
$ cd ~/recipe/taskapp
$ bundle exec cap production deploy
```
![Screen Shot 2015-07-31 at 6.50.25 AM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/deploy.png)

### 確認

```
ブラウザでこのURLを開きます
http://127.0.0.1:8080
```
![Screen Shot 2015-07-31 at 7.10.18 AM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/browser_result.png)

## Serverspec
### インストール
```sh
$ cd ~/recipe/taskapp_vm
$ bundle init
$ vim Gemfile
```
```ruby
source 'https://rubygems.org'
gem 'rake'
gem 'serverspec'
```
```sh
$ bundle install
```
### Serverspec 初期化
```sh
$ bundle exec serverspec-init
```
![Screen Shot 2015-07-31 at 4.07.25 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/init_serverspec.png)

### spec_helper.rbを編集
```sh
$ vim ~/recipe/taskapp_vm/spec/spec_helper.rb
```
```ruby
...
# Disable sudo
set :disable_sudo, true # コメントアウトを外します
...
```
### テストコードを書く
```sh
$ vim ~/recipe/taskapp_vm/spec/default/sample_spec.rb
```
```ruby
# sample_spec.rbの中身
require 'spec_helper'

# vagrantとしてログイン
describe command('whoami'), if: os[:family] == 'ubuntu' do
  its(:stdout) { should match(/vagrant/) }
end

# Ruby のバージョンが2.1.6であること
describe command('/home/vagrant/.rbenv/bin/rbenv version'), if: os[:family] == 'ubuntu' do
  its(:stdout) { should match(/2.1.6/) }
end

# bundler がインストールされていること
describe command('/home/vagrant/.rbenv/shims/gem list'), if: os[:family] == 'ubuntu' do
  its(:stdout) { should match(/bundler/) }
end

# git がインストールされていること
describe package('git'), if: os[:family] == 'ubuntu' do
  it { should be_installed }
end

# Node.js がインストールされていること
describe package('nodejs'), if: os[:family] == 'ubuntu' do
  it { should be_installed }
end

# Nginx がインストールされていること
describe package('nginx'), if: os[:family] == 'ubuntu' do
  it { should be_installed }
end

# Nginx が起動していること
describe service('nginx'), if: os[:family] == 'ubuntu' do
  it { should be_enabled }
  it { should be_running }
end

# 80番ポートが開いていること
describe port(80) do
  it { should be_listening }
end
```

### テストを実行する
```sh
$ bundle exec rake spec:_default
```
![Screen Shot 2015-07-31 at 4.26.01 PM.png](https://raw.githubusercontent.com/yhoshino11/Automation/master/images/serverspec_result.png)
