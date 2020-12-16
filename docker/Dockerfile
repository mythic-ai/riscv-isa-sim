FROM ubuntu as mythic-spike-deps

RUN apt-get -y update && DEBIAN_FRONTEND="noninteractive" apt-get -y install autoconf automake autotools-dev curl python3 libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev git
WORKDIR /mythic
RUN git clone --recursive https://github.com/riscv/riscv-gnu-toolchain
RUN mkdir /mythic/riscv-gnu-toolchain/build
WORKDIR /mythic/riscv-gnu-toolchain/build
RUN ../configure --prefix=/opt/riscv32 --with-arch=rv32ia
RUN make linux

RUN apt-get -y update && DEBIAN_FRONTEND="noninteractive" apt-get -y install autoconf automake autotools-dev curl libmpc-dev libmpfr-dev libgmp-dev libusb-1.0-0-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev device-tree-compiler pkg-config libexpat-dev git
WORKDIR /mythic
RUN git clone --recurse-submodules https://github.com/riscv/riscv-tools.git
WORKDIR /mythic/riscv-tools
ENV RISCV=/opt/riscv32
# RUN rm -rf /mythic/riscv-tools/riscv-isa-sim
# RUN cp -r /mythic/riscv-isa-sim/ /mythic/riscv-tools/riscv-isa-sim
RUN ./build.sh

ENTRYPOINT [ "bash" ]

# FROM mythic-spike-deps as mythic-spike-compile



# FROM mythic-spike-compile as mythic-spike