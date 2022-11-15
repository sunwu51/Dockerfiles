FROM codercom/code-server:latest

USER root

COPY ./code-server* /licence/

ENV NODE_VERSION=14.19.1
ENV NVM_DIR=/root/.nvm
ENV SDKMAN_DIR=/root/.sdkman

# install tools
RUN apt -y update && apt -y install ca-certificates openssl mysql-client iputils-ping build-essential gcc wget zip curl vim python3.9 python3-pip

# install language
RUN curl https://sh.rustup.rs -sSf | bash -s -- -y && \
    echo 'source $HOME/.cargo/env' >> $HOME/.bashrc

RUN curl -s "https://get.sdkman.io" | bash && \
    echo "sdkman_auto_answer=true" > $SDKMAN_DIR/etc/config && \
    echo "sdkman_auto_selfupdate=false" >> $SDKMAN_DIR/etc/config

RUN bash -c "source $SDKMAN_DIR/bin/sdkman-init.sh && sdk install java 11.0.17.fx-zulu && sdk install java 17.0.5.fx-zulu"

RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
RUN . "$NVM_DIR/nvm.sh" && nvm install ${NODE_VERSION} && nvm use v${NODE_VERSION} && nvm alias default v${NODE_VERSION}

ENV JAVA_HOME="$SDKMAN_DIR/candidates/java/current"
ENV PATH="$JAVA_HOME/bin:/root/.nvm/versions/node/v${NODE_VERSION}/bin$PATH"

# install vscode extensions
RUN code-server \
        --install-extension bungcip.better-toml \
        --install-extension serayuzgur.crates \
        --install-extension dsznajder.es7-react-js-snippets \
        --install-extension ritwickdey.LiveServer \
        --install-extension pinage404.rust-extension-pack \
        --install-extension vscjava.vscode-spring-initializr \
        --install-extension Pivotal.vscode-spring-boot \
        --install-extension formulahendry.auto-close-tag \
        --install-extension streetsidesoftware.code-spell-checker \
        --install-extension mikestead.dotenv \
        --install-extension eamodio.gitlens \
        --install-extension k--kato.intellij-idea-keybindings \
        --install-extension cweijan.vscode-mysql-client2 \
        --install-extension SonarSource.sonarlint-vscode \
        --install-extension TabNine.tabnine-vscode \
        --install-extension redhat.java \
        --install-extension vscjava.vscode-java-dependency \
        --install-extension vscjava.vscode-java-debug \
        --install-extension vscjava.vscode-java-test \
        --install-extension rangav.vscode-thunder-client 

ENTRYPOINT ["/usr/bin/entrypoint.sh", "--bind-addr", "0.0.0.0:443", ".", "--cert", "/licence/code-server.crt", "--cert-key", "/licence/code-server.key"]