# ベースになるイメージを選択
FROM ruby:2.6.5

# ruby:2.6.5の内容
# OS : Debian にruby 2.6.5をインストールしてあるイメージ

RUN apt-get update -qq && apt-get install -y build-essential nodejs \
  libpq-dev imagemagick vim yarn locales man nano less \
  && rm -rf /var/lib/apt/lists/* \
  && curl --compressed -o- -L https://yarnpkg.com/install.sh | bash
# RUNは実行するコマンドの意味
# apt-get : パッケージ管理ツール
# update : パッケージリストを更新 -qq でエラー以外は表示しません。
# install : パッケージのインストール -y で途中の確認はすべてyes
# パッケージの詳細はhttps://www.debian.org/distrib/packagesを参照してください。


# bundle installの並列処理の設定 これがあると高速化する
ENV BUNDLE_JOBS=4
# 講義ではオプションとして--jobs=4を指定しているがこれがあると不要になる


# rails new時のGemをインストール
COPY Gemfile Gemfile.lock ./
RUN bundle install
RUN rm -f Gemfile Gemfile.lock

# 日本語環境
RUN locale-gen ja_JP.UTF-8
ENV LANG ja_JP.UTF-8
ENV LC_CTYPE ja_JP.UTF-8
RUN localedef -f UTF-8 -i ja_JP ja_JP.utf8
ENV LC_ALL ja_JP.UTF-8
# 日本語環境が不要であれば不要


# ルート直下にcampという名前で作業ディレクトリを作成（コンテナ内のアプリケーションディレクトリ）
ARG APP_ROOT
RUN echo ${APP_ROOT}
RUN mkdir -p $APP_ROOT
WORKDIR $APP_ROOT


# ターミナル画面を見やすく変更
RUN echo 'export PS1="\n\[\033[35m\]\u@\h\[\033[00m\]:\[\033[36m\]\w\[\033[00m\]\n[app] \$ "' >> /root/.bashrc
# 慣れてきたら不要