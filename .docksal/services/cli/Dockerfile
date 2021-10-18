FROM docksal/cli:php7.4-3.0

RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    DEBIAN_FRONTEND=noninteractive apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get -y --force-yes --no-install-recommends install \
    jq \
    apt-utils \
    python-pip \
    default-jre \
    && DEBIAN_FRONTEND=noninteractive apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
# Get latest version of pip.
RUN pip install --upgrade pip

# Include headless chrome for BLT Behat testing.
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && echo 'deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main' | tee /etc/apt/sources.list.d/google-chrome.list \
    && apt-get update \
    && apt-get -y --no-install-recommends install google-chrome-stable \
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

USER docker

# Aliases...
RUN echo 'alias ll="ls -lha"' >> ~/.bashrc

RUN set -e; \
    # Initialize the user environment (this loads nvm)
    . $HOME/.profile; \
    # Install the necessary nodejs version
    nvm install 10; \
    nvm alias default 10; \
    nvm use default; \
    # Install packages
    npm install -g npm@6; \
    # Cleanup
    nvm clear-cache && npm cache clear --force; \
    # Fix npm complaining about permissions and not being able to update
    sudo rm -rf $HOME/.config

USER root
