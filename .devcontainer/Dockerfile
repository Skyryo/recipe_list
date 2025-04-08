FROM --platform=linux/arm64 dart:latest

# Maintainer
LABEL maintainer="Skyryo"

# ユーザーを作成
RUN groupadd --gid 1000 vscode \
    && useradd --uid 1000 --gid 1000 -m vscode

# 必要なパッケージをインストール
RUN apt-get update && apt-get install -y \
    curl \
    git \
    unzip \
    xz-utils \
    zip \
    libglu1-mesa \
    wget \
    openjdk-17-jdk \
    chromium \
    libpulse0 \
    libnss3 \
    libgconf-2-4

# Workdir
WORKDIR /workspace/recipe_list

# Flutter のインストール
# Android SDK のセットアップ
ENV ANDROID_HOME=/opt/android-sdk
ENV ANDROID_SDK_ROOT=$ANDROID_HOME
ENV PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/platform-tools

# Flutter のセットアップ
ENV FLUTTER_HOME=/opt/flutter
ENV PATH=$FLUTTER_HOME/bin:$PATH

# Chrome実行パスの設定
ENV CHROME_EXECUTABLE=/usr/bin/chromium

# Flutter のバージョンを指定
ARG FLUTTER_VERSION=3.29.2

# Flutter のインストール（GitHubからクローン）
RUN git clone https://github.com/flutter/flutter.git -b stable $FLUTTER_HOME && \
    cd $FLUTTER_HOME && \
    git remote set-url origin https://github.com/flutter/flutter.git && \
    git fetch && \
    git checkout stable && \
    git pull && \
    $FLUTTER_HOME/bin/flutter precache && \
    $FLUTTER_HOME/bin/flutter config --no-analytics && \
    $FLUTTER_HOME/bin/flutter config --no-enable-linux-desktop && \
    chown -R vscode:vscode $FLUTTER_HOME && \
    mkdir -p $ANDROID_HOME && \
    cd $ANDROID_HOME && \
    wget https://dl.google.com/android/repository/commandlinetools-linux-9477386_latest.zip && \
    unzip commandlinetools-linux-*_latest.zip && \
    rm commandlinetools-linux-*_latest.zip && \
    mkdir -p $ANDROID_HOME/cmdline-tools/latest && \
    mv $ANDROID_HOME/cmdline-tools/* $ANDROID_HOME/cmdline-tools/latest/ 2>/dev/null || true && \
    chown -R vscode:vscode $ANDROID_HOME

# Android SDK コンポーネントのインストール
USER vscode
RUN yes | $ANDROID_HOME/cmdline-tools/latest/bin/sdkmanager --licenses && \
    $ANDROID_HOME/cmdline-tools/latest/bin/sdkmanager "platform-tools" "platforms;android-34" "build-tools;34.0.0"
USER root

# PATH にFlutterを追加
ENV PATH=$PATH:$FLUTTER_HOME/bin