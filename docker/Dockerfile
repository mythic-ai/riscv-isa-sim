FROM ubuntu as mythic-spike

RUN apt-get -y update && DEBIAN_FRONTEND="noninteractive" apt-get -y install autoconf automake autotools-dev curl python3 libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev git
WORKDIR /mythic
RUN git clone --recursive https://github.com/riscv/riscv-gnu-toolchain
RUN mkdir /mythic/riscv-gnu-toolchain/build
WORKDIR /mythic/riscv-gnu-toolchain/build
ENV RISCV=/usr/local
RUN ../configure --help
RUN ../configure --prefix=${RISCV} --with-arch=rv32ia --with-abi=ilp32
RUN make newlib -j6
ENV PATH=${PATH}:${RISCV}/bin

RUN apt-get -y update && DEBIAN_FRONTEND="noninteractive" apt-get -y install autoconf automake autotools-dev curl libmpc-dev libmpfr-dev libgmp-dev libusb-1.0-0-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev device-tree-compiler pkg-config libexpat-dev git
WORKDIR /mythic
RUN git clone --recurse-submodules https://github.com/riscv/riscv-tools.git
WORKDIR /mythic/riscv-tools
RUN rm -rf /mythic/riscv-tools/riscv-isa-sim
COPY . /mythic/riscv-tools/riscv-isa-sim/
RUN mkdir /mythic/riscv-tools/riscv-isa-sim/build
WORKDIR /mythic/riscv-tools/riscv-isa-sim/build
RUN ../configure --prefix=${RISCV} && \
    make -j6 && \
    make install
RUN mkdir /mythic/riscv-tools/riscv-pk/build
WORKDIR /mythic/riscv-tools/riscv-pk/build
RUN ../configure --prefix=${RISCV} --host=riscv32-unknown-elf && \
    make -j6 && \
    make install

ENTRYPOINT [ "bash", "-lc" ]