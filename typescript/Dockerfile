FROM argoproj/argocd:latest

# Switch to root for the ability to perform install
USER root

# Install tools needed for our CDK8s Typescript server
RUN apt-get update && apt-get install -y \
        curl \
        nodejs && \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt-get update && apt-get install -y yarn && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    curl -o /usr/local/bin/sops -L https://github.com/mozilla/sops/releases/download/3.2.0/sops-3.2.0.linux && \
    chmod +x /usr/local/bin/sops

RUN yarn global add cdk8s-cli

# Switch back to non-root user
USER argocd
