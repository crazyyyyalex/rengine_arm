FROM ubuntu:20.04

ENV GO111MODULE="on"
ENV GOPROXY="https://goproxy.io,direct"
# Labels and Credits
LABEL \
    name="reNgine" \
    author="Yogesh Ojha and OP" \
    description="reNgine is a automated pipeline of recon process, useful for information gathering during web application penetration testing."

# Environment Variables
ENV DEBIAN_FRONTEND="noninteractive" \
    DATABASE="postgres"
ENV GOROOT="/usr/local/go"
ENV GOPATH=$HOME/go
ENV PATH="${PATH}:${GOROOT}/bin:${GOPATH}/bin"
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# Install essentials
RUN apt update -y && apt install -y  --no-install-recommends \
    build-essential \
    cmake \
    firefox \
    gcc \
    git \
    libpq-dev \
    libpcap-dev \
    netcat \
    postgresql \
    python3 \
	nmap \
    python3-dev \
    python3-pip \
    python3-netaddr \
    wget \
    x11-utils \
    xvfb \
    python3-cffi \
    python3-brotli \
    libpango-1.0-0 \
    libpangoft2-1.0-0 \
    geoip-bin \
    geoip-database \
	software-properties-common \
	unzip \
    firefox-geckodriver  #install Download geckodriver



# Download and install go 1.20

#RUN wget https://go.dev/dl/go1.20.11.linux-arm64.tar.gz
#RUN tar -xvf go1.20.11.linux-arm64.tar.gz
#RUN rm go1.20.11.linux-arm64.tar.gz

# Download and install go 1.21


RUN wget https://go.dev/dl/go1.21.7.linux-arm64.tar.gz
RUN tar -xvf go1.21.7.linux-arm64.tar.gz
RUN rm go1.21.7.linux-arm64.tar.gz
RUN mv go /usr/local





# Make directory for app
WORKDIR /usr/src/app

# set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# Download Go packages
RUN go install -v github.com/hakluke/hakrawler@latest
RUN GO111MODULE=on go install -v -v github.com/bp0lr/gauplus@latest
RUN GO111MODULE=on go install -v github.com/jaeles-project/gospider@latest
RUN GO111MODULE=on go install -v github.com/owasp-amass/amass/v3/...@latest
RUN go install -v github.com/ffuf/ffuf@latest
RUN go install -v github.com/tomnomnom/assetfinder@latest
RUN GO111MODULE=on go install -v github.com/tomnomnom/gf@latest
RUN GO111MODULE=on go install -v github.com/tomnomnom/unfurl@latest
RUN GO111MODULE=on go install -v github.com/tomnomnom/waybackurls@latest

#2.03
RUN GO111MODULE=on go install -v github.com/lc/gau/v2/cmd/gau@latest
RUN GO111MODULE=on go install -v github.com/hahwul/dalfox/v2@latest
RUN GO111MODULE=on go install -v github.com/dwisiswant0/crlfuzz/cmd/crlfuzz@latest
RUN GO111MODULE=on go install -v github.com/sa7mon/s3scanner@latest

# Install httpx
RUN wget https://github.com/projectdiscovery/httpx/releases/download/v1.4.0/httpx_1.4.0_linux_arm64.zip
RUN unzip httpx_1.4.0_linux_arm64.zip
RUN rm httpx_1.4.0_linux_arm64.zip
RUN mv httpx /go/bin
# httpx seems to have issue, use alias instead!!!
RUN echo 'alias httpx="/go/bin/httpx"' >> ~/.bashrc

# Install  subfinder
RUN wget https://github.com/projectdiscovery/subfinder/releases/download/v2.6.5/subfinder_2.6.5_linux_arm64.zip
RUN unzip subfinder_2.6.5_linux_arm64.zip
RUN rm subfinder_2.6.5_linux_arm64.zip
RUN mv subfinder /go/bin

# Install  nuclei
RUN wget https://github.com/projectdiscovery/nuclei/releases/download/v3.1.10/nuclei_3.1.10_linux_arm64.zip
RUN unzip -o nuclei_3.1.10_linux_arm64.zip
RUN rm nuclei_3.1.10_linux_arm64.zip
RUN mv nuclei /go/bin

# Install  naabu
RUN wget https://github.com/projectdiscovery/naabu/releases/download/v2.1.8/naabu_2.1.8_linux_amd64.zip
RUN unzip -o naabu_2.1.8_linux_amd64.zip
RUN rm naabu_2.1.8_linux_amd64.zip
RUN mv naabu /go/bin

# Install  katana
RUN wget https://github.com/projectdiscovery/katana/releases/download/v1.0.5/katana_1.0.5_linux_arm64.zip
RUN unzip -o katana_1.0.5_linux_arm64.zip
RUN rm katana_1.0.5_linux_arm64.zip
RUN mv katana /go/bin

# Update Nuclei and Nuclei-Templates
RUN nuclei -update
RUN nuclei -update-templates

# Copy requirements
COPY ./requirements.txt /tmp/requirements.txt
RUN pip3 install --upgrade setuptools pip && \
    pip3 install -r /tmp/requirements.txt

# install eyewitness

RUN python3 -m pip install fuzzywuzzy \
    selenium \
    python-Levenshtein \
    pyvirtualdisplay \
    netaddr

# Copy source code
COPY . /usr/src/app/
