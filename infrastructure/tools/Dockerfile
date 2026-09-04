# ==============================================================
# Understanding how Docker files are constructed is an important
# part of your cloud engineering learning. Follow a long with 
# this file so you can understand the logical flow. 
#
# The first section will work through building our system tools.
# The docker file does not have multi-stage build and will not
# cover them. I recommend reviewing the Docker documentation for 
# them.
# ==============================================================

# Creates the appropriate image based on your device Architecture
# The ARG flag is considered a build time variable, we will use this
# throughout our Dockerfile
ARG TARGETARCH

#The FROM statement is the only required statement witin a Dockerfile
#In this case we are utilizing Ubuntu and we have the version "pinned"
#Pinning is the process of keeping to a strict version and not introducing
#any breaking changes to our files.
FROM ubuntu:26.04

#In order to pin our versions, we are going to utilize the ARG statement
#These variables will then be utilized throughout our RUN statement and
#will append the version. ARG naming convensionts are typically UPPERCASE
ARG GO_VERSION=1.22.1
ARG AWS_CLI_VERSION=2.15.30
ARG KUBECTL_VERSION=v1.29.2
ARG TERRAFORM_VERSION=1.7.4
ARG HELM_VERSION=v3.14.2
ARG NODE_VERSION=20.11.0
ARG PG_CLIENT_VERSION=16


# Update the package lists and install forge-tools dependencies
RUN apt-get update && apt-get install -y\
	gnupg\
	software-properties-common\
	ca-certificates\
	curl\
	git\
	vim\
	wget\
	postgresql-client-${PG_CLIENT_VERSION}\
	&& wget -q https://go.dev{GO_VERSION}.linux-${TARGETARCH}.tar.gz\
	&& tar -C /usr/local -xzf go${GO_VERSION}.linux-${TARGETARCH}.tar.gz\
	&& curl -s "https://amazonaws.com{AWS_CLI_VERSION}.zip" -o "awscliv2.zip"\
	&& unzip -q awscliv2.zip && ./aws/install\
  nodejs=${NODE_VERSION}-1nodesource1 npm\
	&& wget -O- https://apt.releases.hashicorp.com/gpg\
	| gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg\
	&& echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main"\ 
	| tee /etc/apt/sources.list.d/hashicorp.list\
	&& apt-get update && apt-get install -y terraform=${TERRAFORM_VERSION}\
	&& curl -LO "https://dl.k8s.io/${KUBECTL_VERSION}/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/${TARGETARCH}/kubectl"\
	&& install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl\
	&& curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-4\
	&& chmod 700 get_helm.sh\
	&& ./get_helm.sh\
	&& groupadd engineering --gid 1001\
	&& useradd -m -u 1001 -g engineering -s /bin/bash -c "Forge user to conduct training" forge
	 
# =================================================================
# PHASE 2 (Forge User)
# =================================================================

WORKDIR /home/workspace

RUN chown -R :engineering /home/workspace && chmod -R 775 /home/workspace

USER forge
