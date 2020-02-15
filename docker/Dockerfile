FROM alpine:edge AS builder
RUN apk add alpine-sdk cmake make clang clang-static clang-dev llvm-dev llvm-static \
	&& git clone --depth=1 --recursive https://github.com/MaskRay/ccls \
	&& cd ccls \
	&& cmake -H. -BRelease -DCMAKE_BUILD_TYPE=Release \
	&& cmake --build Release --target install

RUN apk add go make \
	&& git clone https://github.com/juliosueiras/terraform-lsp \
	&& cd terraform-lsp \
	&& make

FROM alpine:edge
RUN apk update \
	&& apk upgrade \
	&& apk add git npm nodejs

# Bash
RUN apk --no-cache add bash \
	&& npm i -g bash-language-server

# C-family
RUN apk add alpine-sdk clang llvm
COPY --from=builder /usr/local/bin/ccls /usr/local/bin/ccls

# Docker
RUN npm i -g dockerfile-language-server-nodejs

# Go
RUN apk --no-cache add go \ 
	&& go get golang.org/x/tools/gopls
ENV PATH="${PATH}:/root/go/bin"

# Python
RUN apk add python3 python3-dev \
	&& pip3 install python-language-server[all]

# Terraform
COPY --from=builder /terraform-lsp/terraform-lsp /usr/local/bin/terraform-lsp

# Web Tools
RUN npm i -g vscode-css-languageserver-bin \
	vscode-html-languageserver-bin \
	javascript-typescript-langserver \
	typescript-language-server \
	typescript
