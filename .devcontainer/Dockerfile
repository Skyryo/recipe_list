# ① ベースとする Docker イメージ
FROM dart:latest

# Maintainer
LABEL maintainer="Skyryo"

# ユーザーを作成
RUN groupadd --gid 1000 vscode \
    && useradd --uid 1000 --gid 1000 -m vscode

# Workdir
WORKDIR /workspace/recipe_list

# Flutter のインストール先を明示的に指定
ARG FLUTTER_HOME=/opt/flutter
ENV FLUTTER_HOME=${FLUTTER_HOME}
ENV PATH=${FLUTTER_HOME}/bin:$PATH

# ② FVM による Flutter のインストール
# FVM で Flutter をインストールし、指定したディレクトリに移動
RUN dart pub global activate fvm --verbose && \
    fvm doctor --verbose && \
    fvm install ${FLUTTER_VERSION} --verbose && \
    fvm use --force ${FLUTTER_VERSION} --verbose && \
    mkdir -p ${FLUTTER_HOME} && \
    cp -R $(fvm flutter sdk-path)/* ${FLUTTER_HOME}/ && \
    chown -R vscode:vscode ${FLUTTER_HOME}