FROM python:3-alpine

RUN apk add git curl jq yq doas bind-tools vim aws-cli terraform
RUN apk --no-cache add go && go install code.cloudfoundry.org/uaa-cli@latest && mv /root/go/bin/uaa-cli /usr/local/bin/uaa && apk del go && rm -rf /root/go
RUN adduser -D vscode && echo "permit nopass vscode" >> /etc/doas.d/doas.conf
RUN latest_tag=$(curl -s https://api.github.com/repositories/57993310/releases/latest | sed -Ene '/^\s*"tag_name": \s*"(.+)",$/s//\1/p') && \
    curl -Lf https://github.com/cloudfoundry-incubator/credhub-cli/releases/latest/download/credhub-linux-${latest_tag}.tgz | tar -zx -C /usr/local/bin
RUN latest_tag=$(curl -s https://api.github.com/repos/pivotal-cf/om/releases/latest | sed -Ene '/^\s*"tag_name": \s*"(.+)",$/s//\1/p') && \
    curl -Lf https://github.com/pivotal-cf/om/releases/latest/download/om-linux-${latest_tag}.tar.gz | tar -zx -C /usr/local/bin om
RUN latest_tag=$(curl -s https://api.github.com/repos/pivotal-cf/pivnet-cli/releases/latest | sed -Ene '/^\s*"tag_name": \s*"v(.+)",$/s//\1/p') && \
    curl -Lf https://github.com/pivotal-cf/pivnet-cli/releases/latest/download/pivnet-linux-amd64-${latest_tag} -o /usr/local/bin/pivnet && \
    chmod +x /usr/local/bin/pivnet
RUN latest_tag=$(curl -s https://api.github.com/repos/cloudfoundry/bosh-cli/releases/latest | sed -Ene '/^\s*"tag_name": \s*"v(.+)",$/s//\1/p') && \
    curl -Lf https://github.com/cloudfoundry/bosh-cli/releases/latest/download/bosh-cli-${latest_tag}-linux-amd64 -o /usr/local/bin/bosh && \
    chmod +x /usr/local/bin/bosh
RUN latest_tag=$(curl -s https://api.github.com/repos/cloudfoundry/cli/releases/latest| sed -Ene '/^\s*"tag_name": \s*"v(.+)",$/s//\1/p') && \
    curl -Lf "https://packages.cloudfoundry.org/stable?release=linux64-binary&version=$latest_tag&source=github-rel" | tar xz -C /usr/local/bin

ENTRYPOINT /bin/sh
