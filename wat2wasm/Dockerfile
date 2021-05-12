# use the red hat universal base image as base image builder
FROM registry.access.redhat.com/ubi8/ubi-minimal as build

# label for maintainer
LABEL maintainer="Chris Hay"

# labels for container catalog
LABEL summary="builder image for the WebAssembly Toolkit (WABT)"
LABEL description="This is a builder docker image of the WABT toolkit running on Red Hat UBI mimimal.  This allows you to use tools such as wat2wasm without downloading a compiling source locally"

# install git, make, gcc
RUN microdnf update -y && microdnf install git gcc-c++ make -y

# git clone wabt
WORKDIR /
RUN git clone --recursive https://github.com/WebAssembly/wabt

# create the build directory
RUN mkdir build
WORKDIR /wabt/build

## install cmake
RUN microdnf install cmake -y
RUN cmake ..
RUN make

# create a working directory to build
WORKDIR /wabt

# use the red hat universal base image minimal as runtime image
FROM registry.access.redhat.com/ubi8/ubi-minimal as runtime

# shadowutils needed for groupadd
RUN microdnf install shadow-utils

# label for maintainer
LABEL maintainer="Chris Hay"

# labels for container catalog
LABEL summary="Provides a docker image for the WebAssembly Toolkit (WABT)"
LABEL description="This is a docker image of the WABT toolkit running on Red Hat UBI mimimal.  This allows you to use tools such as wat2wasm without downloading a compiling source locally"

# create a working directory for WABT
WORKDIR /wabt/
COPY --from=build /wabt/bin/wat2wasm /wabt/bin/wat2wasm

# set working directory as bin
RUN groupadd -r wabt && useradd --no-log-init -r -g wabt wabt
RUN mkdir -p /data ; chown -R wabt:wabt /data
WORKDIR /data/output-wasm

USER wabt

# test a command
ENTRYPOINT [ "/wabt/bin/wat2wasm" ] 
