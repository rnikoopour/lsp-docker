FROM alpine:edge AS ccls
RUN apk add alpine-sdk cmake make clang clang-static clang-dev llvm-dev llvm-static \
	&& git clone --depth=1 --recursive https://github.com/MaskRay/ccls \
	&& cd ccls \
	&& cmake -H. -BRelease -DCMAKE_BUILD_TYPE=Release \
	&& cmake --build Release --target install

FROM alpine:edge
RUN apk update \
	&& apk upgrade \
	&& apk add git npm nodejs

# Bash
RUN apk --no-cache add bash
RUN npm i -g bash-language-server

# Docker
RUN npm i -g dockerfile-language-server-nodejs

# Go
RUN apk --no-cache add go \ 
	&& go get golang.org/x/tools/gopls
ENV PATH="${PATH}:/root/go/bin"

# Python
RUN apk add python3 python3-dev \
	&& pip3 install python-language-server[all]

# C-family
RUN apk add alpine-sdk clang llvm
COPY --from=ccls /usr/local/bin/ccls /usr/local/bin/ccls

# Web Tools
RUN npm install --global vscode-css-languageserver-bin \
	vscode-html-languageserver-bin


# Download "LLVM source code" and rename the extracted directory to llvm
# Download "Clang source code" and rename the extracted directory to llvm/tools/clang
# cd llvm
# cmake -H. -BRelease -G Ninja -DCMAKE_BUILD_TYPE=Release -DLLVM_TARGETS_TO_BUILD=X86
# ninja -C Release clangFormat clangFrontendTool clangIndex clangTooling clang

## Prev 1.8 gigs
