FROM plantuml/plantuml AS plantuml 
FROM minlag/mermaid-cli AS mermaid

# FROM alpine 
FROM ubuntu
RUN apt-get update

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get install -y sudo git curl bash 

RUN apt-get install -y texlive-full python3 pip
ADD requirements.txt /python-dependencies.txt 
RUN pip3 install --upgrade pip 
RUN pip3 install -r  /python-dependencies.txt 

RUN apt-get install -y nodejs npm chromium
ENV CHROME_BIN="/usr/bin/chromium-browser" \
    PUPPETEER_SKIP_CHROMIUM_DOWNLOAD="true" \
    PUPPETEER_EXECUTABLE_PATH="/usr/bin/chromium-browser" \
    CHROMIUM_USER_FLAGS="--no-sandbox"

#COPY --from=mermaid /app/install-dependencies.sh /mermaid-dependencies.sh 
#RUN chmod 755 /mermaid-dependencies.sh && /bin/sh /mermaid-dependencies.sh
RUN npm install -g chromium @mermaid-js/mermaid-cli
COPY --from=mermaid /puppeteer-config.json /puppeteer-config.json

RUN apt-get install -y openjdk11-jre-headless
COPY --from=plantuml /opt/plantuml.jar /bin/plantuml.jar 

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME -s /bin/bash \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

USER $USERNAME
