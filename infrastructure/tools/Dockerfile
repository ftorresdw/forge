FROM ubuntu:26.04

# Set the working directory
WORKDIR /

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
	&& wget -O- https://apt.releases.hashicorp.com/gpg \ 
	| gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg \
	&& echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" \ 
	| tee /etc/apt/sources.list.d/hashicorp.list \
	&& apt-get update && apt-get install -y \ 
	terraform \

