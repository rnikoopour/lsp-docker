FROM alpine:edge

RUN apk update \
	&& apk upgrade \
	&& apk add git npm nodejs

# Bash
RUN apk add bash
RUN npm i -g bash-language-server

# Docker
RUN npm i -g dockerfile-language-server-nodejs

# Go
RUN apk add go \ 
	&& go get golang.org/x/tools/gopls
ENV PATH="${PATH}:/root/go/bin"

# Python
RUN apk add python3 python3-dev \
	&& pip3 install python-language-server[all]

# C-family
RUN apk add cmake make clang clang-static alpine-sdk clang-dev llvm-dev llvm-static \
	&& cd ~ \
	&& git clone --depth=1 --recursive https://github.com/MaskRay/ccls \
	&& cd ccls \
	&& cmake -H. -BRelease -DCMAKE_BUILD_TYPE=Release \
	&& cmake --build Release --target install \
	&& cd .. \
	&& rm -rf ccls

# Download "LLVM source code" and rename the extracted directory to llvm
# Download "Clang source code" and rename the extracted directory to llvm/tools/clang
# cd llvm
# cmake -H. -BRelease -G Ninja -DCMAKE_BUILD_TYPE=Release -DLLVM_TARGETS_TO_BUILD=X86
# ninja -C Release clangFormat clangFrontendTool clangIndex clangTooling clang

