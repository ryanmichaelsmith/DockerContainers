FROM python:3-bullseye

RUN apt-get update && apt-get install -y git curl unzip jq sudo man less dnsutils
RUN useradd -m -s /bin/bash vscode && echo "vscode ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/vscode
RUN curl -L "https://packages.cloudfoundry.org/stable?release=linux64-binary&version=v7&source=github" | tar -zx -C /usr/local/bin
RUN latest_tag=$(curl -s https://api.github.com/repos/cloudfoundry-incubator/credhub-cli/releases/latest | sed -Ene '/^\s*"tag_name": \s*"(.+)",$/s//\1/p') && \
    curl -Lf https://github.com/cloudfoundry-incubator/credhub-cli/releases/latest/download/credhub-linux-${latest_tag}.tgz | tar -zx -C /usr/local/bin
RUN latest_tag=$(curl -s https://api.github.com/repos/pivotal-cf/om/releases/latest | sed -Ene '/^\s*"tag_name": \s*"(.+)",$/s//\1/p') && \
    curl -Lf https://github.com/pivotal-cf/om/releases/latest/download/om-linux-${latest_tag}.tar.gz | tar -zx -C /usr/local/bin om
RUN latest_tag=$(curl -s https://api.github.com/repos/cloudfoundry/bosh-cli/releases/latest | sed -Ene '/^\s*"tag_name": \s*"v(.+)",$/s//\1/p') && \
    curl -Lf https://github.com/cloudfoundry/bosh-cli/releases/latest/download/bosh-cli-${latest_tag}-linux-amd64 -o /usr/local/bin/bosh && \
    chmod +x /usr/local/bin/bosh
RUN latest_tag=$(curl -s https://api.github.com/repos/cloudfoundry-incubator/uaa-cli/releases/latest | sed -Ene '/^\s*"tag_name": \s*"(.+)",$/s//\1/p') && \
    curl -Lf https://github.com/cloudfoundry-incubator/uaa-cli/releases/latest/download/uaa-linux-amd64-${latest_tag} -o /usr/local/bin/uaa && \
    chmod +x /usr/local/bin/uaa
RUN latest_tag=$(curl -s https://api.github.com/repos/concourse/concourse/releases/latest | sed -Ene '/^\s*"tag_name": \s*"v(.+)",$/s//\1/p') && \
    curl -Lf https://github.com/concourse/concourse/releases/latest/download/fly-${latest_tag}-linux-amd64.tgz | tar -zx -C /usr/local/bin && \
    chmod +x /usr/local/bin/fly
RUN current_terraform=$(curl -s https://checkpoint-api.hashicorp.com/v1/check/terraform | jq -r -M '.current_version')/terraform_$(curl -s https://checkpoint-api.hashicorp.com/v1/check/terraform | jq -r -M '.current_version') && \
    curl -Lf https://releases.hashicorp.com/terraform/${current_terraform}_linux_amd64.zip -o /tmp/terraform.zip && unzip -o /tmp/terraform.zip -d /usr/local/bin
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o /tmp/awscliv2.zip && \
    unzip -q -o /tmp/awscliv2.zip -d /tmp && /tmp/aws/install > /dev/null

USER vscode
WORKDIR /home/vscode

RUN find /home/vscode -maxdepth 0 -empty -exec cp -r /etc/skel/. /home/vscode \;
ENV TERM xterm-256color

ENTRYPOINT /bin/bash
