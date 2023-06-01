FROM plantuml/plantuml AS plantuml
FROM minlag/mermaid-cli AS mermaid

FROM ubuntu
RUN apt-get update

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get install -y --no-install-recommends \
    ssh sudo git git-lfs curl bash software-properties-common

RUN apt-get install -y texlive-full python3 pip
ADD requirements.txt /python-dependencies.txt
RUN pip3 install --upgrade pip
RUN pip3 install -r  /python-dependencies.txt

RUN curl -fsSL https://deb.nodesource.com/setup_lts.x | bash
RUN apt-get install -y nodejs 
RUN npm install -g npm@latest
ENV CHROME_BIN="/usr/bin/chromium-browser" \
    PUPPETEER_SKIP_CHROMIUM_DOWNLOAD="true" \
    PUPPETEER_EXECUTABLE_PATH="/usr/bin/chromium-browser" \
    CHROMIUM_USER_FLAGS="--no-sandbox"

RUN add-apt-repository ppa:savoury1/chromium
RUN apt install chromium-browser
RUN npm install -g chromium @mermaid-js/mermaid-cli
COPY --from=mermaid /puppeteer-config.json /puppeteer-config.json

RUN apt-get install -y openjdk-19-jre-headless graphviz
COPY --from=plantuml /opt/plantuml.jar /bin/plantuml.jar

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME -s /bin/bash \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

USER $USERNAME