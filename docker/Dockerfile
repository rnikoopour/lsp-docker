FROM alpine:3.11

RUN apk update \
	&& apk upgrade \
	&& apk add git npm nodejs

# Go
RUN apk add go \ 
	&& go get golang.org/x/tools/gopls
ENV PATH="${PATH}:/root/go/bin"

# Python
RUN apk add python3 python3-dev \
	&& pip3 install python-language-server[all]

# Docker
RUN npm i -g dockerfile-language-server-nodejs

# Bash
RUN apk add bash
RUN npm i -g bash-language-server
