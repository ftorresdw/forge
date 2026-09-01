# ==============================================================
# PHASE 1 (Builds all system tools)
# ==============================================================

# Creates the appropriate image based on your device Architecture
ARG TARGETARCH

FROM ubuntu:26.04 AS builder

# Add the argument for the architecture
ARG TARGETARCH

# Update the package lists and install forge-tools dependencies
RUN apt-get update && apt-get install -y \
	gnupg \
	software-properties-common \
	ca-certificates \
	curl \
	git \
	vim \
	awscli \
	golang \
	wget \
	postgresql-client \
	nodejs npm \
	&& wget -O- https://apt.releases.hashicorp.com/gpg \
	| gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg \
	&& echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" \ 
	| tee /etc/apt/sources.list.d/hashicorp.list \
	&& apt-get update && apt-get install -y terraform \
	&& curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/${TARGETARCH}/kubectl" \
	&& install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl \
	&& curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-4 \
	&& chmod 700 get_helm.sh \
	&& ./get_helm.sh \
	&& groupadd engineering --gid 1001 \
	&& useradd -m -u 1001 -g engineering -s /bin/bash -c "Forge user to conduct training" forge
	 
# =================================================================
# PHASE 2 (Forge User)
# =================================================================

WORKDIR /home/workspace

RUN chown -R :engineering /home/workspace && chmod -R 775 /home/workspace

USER forge
