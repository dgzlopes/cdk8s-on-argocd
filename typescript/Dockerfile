FROM argoproj/argocd:latest

# Switch to root for the ability to perform install
USER root

RUN apt-get update && apt-get install -y curl
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    curl https://baltocdn.com/helm/signing.asc | apt-key add - && \
    echo "deb https://baltocdn.com/helm/stable/debian/ all main" | tee /etc/apt/sources.list.d/helm-stable-debian.list

RUN apt-get update && apt-get install -y \
        nodejs \
        npm \
        apt-transport-https \
        yarn \
        helm && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    curl -o /usr/local/bin/sops -L https://github.com/mozilla/sops/releases/download/3.2.0/sops-3.2.0.linux && \
    chmod +x /usr/local/bin/sops

RUN npm install -g typescript
RUN yarn global add cdk8s-cli

# Switch back to non-root user
USER argocd
