# Pythonの開発に適したイメージを使用するため、基本イメージをpythonの公式イメージに変更します。
FROM python:3.10-slim

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get -y update \
    && apt-get -y upgrade

# 必要なlinuxのライブラリ
RUN apt-get install -y wget gnupg unzip curl

# seleniumのためにchromeを入れる
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add && \
    echo 'deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main' | tee /etc/apt/sources.list.d/google-chrome.list && \
    apt-get update && \
    apt-get install -y google-chrome-stable
# ビルドするときに手元のchromeのバージョンを最新化し、バージョンを同じものにコピペ(chromeは最新を持ってくるためドライバが対応しているかわからないため)
RUN CHROMEDRIVER_VERSION=`curl -sS https://googlechromelabs.github.io/chrome-for-testing/LATEST_RELEASE_STABLE` \
  && curl -sS -o /tmp/chromedriver_linux64.zip https://edgedl.me.gvt1.com/edgedl/chrome/chrome-for-testing/$CHROMEDRIVER_VERSION/linux64/chromedriver-linux64.zip
RUN cd /tmp && \
    unzip chromedriver_linux64.zip && \
    rm -f chromedriver_linux64.zip && \
    mv chromedriver-linux64/chromedriver /usr/local/bin/

# 必要なpython ライブラリを入れる
WORKDIR /app
ADD requirements.txt /app
RUN pip install -r requirements.txt
