# Environment for Otus-DevOps course.
# Docker, Packer, Terraform, Ansible, YandexCloud
#
# Build image:  
#  docker build . -t devops:latest
# Run image: 
#  docker run -it --rm -v "$PWD":/app -w /app  -e USER_ID=$(id -u) -e USER_GRP=$(id -g) devops:latest

ARG DEBIAN_FRONTEND=noninteractive

FROM ubuntu:20.04
RUN apt-get update \
	&& apt-get install -y \
		apt-transport-https \
		ca-certificates \
		curl \
		gnupg-agent \
		software-properties-common \
	&& rm -rf /var/lib/apt/lists/*

RUN apt-get update \
	&& apt-get install -y \
		vim-tiny indent \
		git \
		htop \
		dnsutils \
		sudo \
		iproute2 \
	&& rm -rf /var/lib/apt/lists/*

# Docker
RUN 	export APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=true \
	&& curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - \
	&& echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
		$(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list \
	&& apt-get update -o Dir::Etc::SourceList=/etc/apt/sources.list.d/docker.list \
	&& apt-get install -y docker-ce-cli \
	&& rm -rf /var/lib/apt/lists/*

# Terraform & Packer
RUN	export APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=true \
	&& curl -fsSL https://apt.releases.hashicorp.com/gpg | apt-key add - \
	&& echo "deb [arch=amd64] https://apt.releases.hashicorp.com \
		$(lsb_release -cs) main" > /etc/apt/sources.list.d/hashicorp.list \
	&& apt-get update -o Dir::Etc::SourceList=/etc/apt/sources.list.d/hashicorp.list \
	&& apt-get install -y terraform packer \
	&& terraform -help \
	&& packer -help \
	&& terraform -install-autocomplete \
	&& rm -rf /var/lib/apt/lists/*
	
# Ansible
#RUN apt-add-repository --yes ppa:ansible/ansible \
RUN apt-get update && apt-get install --yes ansible   \
	&& rm -rf /var/lib/apt/lists/*

# Yandex-Cloud tools
RUN curl https://storage.yandexcloud.net/yandexcloud-yc/install.sh | \
    bash -s -- -i /opt/yandex-cloud -n \
	&& echo "if [ -f '/opt/yandex-cloud/path.bash.inc' ]; then source '/opt/yandex-cloud/path.bash.inc'; fi" \
		>> /etc/bash.bashrc \
	&& echo "if [ -f '/opt/yandex-cloud/completion.bash.inc' ]; then source '/opt/yandex-cloud/completion.bash.inc'; fi" \
		>> /etc/bash.bashrc

RUN apt update && apt install locales \
	&& locale-gen ru_RU.UTF-8


# Username is a name of the user, who runs the container
CMD export USER=${USER:-devop}; \
	export LANG=ru_RU.UTF-8; addgroup --gid $USER_GRP $USER && adduser --shell /bin/bash --system --uid $USER_ID --gid $USER_GRP --disabled-password $USER && su $USER

LABEL com.example-version="0.0.7-beta"
LABEL com.example-author="Sergey Ryzhikov <sergey-werk@ya.ru>"
