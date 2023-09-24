# RISC-V Set up script on Ubuntu

```
  sudo apt-get install git vim -y  
  sudo apt-get install autoconf automake autotools-dev curl libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev git libexpat1-dev gtkwave -y  
  cd  
  pwd=$PWD  
  mkdir riscv_toolchain  
  cd riscv_toolchain  
  wget "https://static.dev.sifive.com/dev-tools/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz"  
  tar -xvzf riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz   
  export PATH=$pwd/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH  
  sudo apt-get install device-tree-compiler -y  
  git clone https://github.com/riscv/riscv-isa-sim.git  
  cd riscv-isa-sim/  
  mkdir build  
  cd build  
  ../configure --prefix=$pwd/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14  
  make  
  sudo make install  
  cd $pwd/riscv_toolchain  
  git clone https://github.com/riscv/riscv-pk.git  
  cd riscv-pk/  
  mkdir build  
  cd build/  
  ../configure --prefix=$pwd/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14 --host=riscv64-unknown-elf  
  make  
  sudo make install  
  export PATH=$pwd/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/riscv64-unknown-elf/bin:$PATH  
  cd $pwd/riscv_toolchain  
  git clone https://github.com/steveicarus/iverilog.git  
  cd iverilog/  
  git checkout --track -b v10-branch origin/v10-branch  
  git pull   
  chmod 777 autoconf.sh   
  ./autoconf.sh   
  ./configure   
  make  
  sudo make install 　
```
  # Compile with RISC-V Compiler
```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```
-O1: optimization (e.g. -O1 or -Ofast)   
-mabi: application binary interface (e.g. lp64 = long integer pointer 64)    
-march: architecture (e.g. rv64i = risc-v 64 integer) 

# Disassmeble RISC-V object file
```
riscv64-unknown-elf-objdump -d sum1ton.o
```
<img width="1059" alt="image" src="https://github.com/RISCV-MYTH-WORKSHOP/riscv-myth-workshop-sep23-kohshi54/assets/80312261/6b20889d-ab4c-440b-b57b-99453e062669">


# Debug RISC-V object file with spike
```
spike pk sum1ton.o
```
<img width="1058" alt="image" src="https://github.com/RISCV-MYTH-WORKSHOP/riscv-myth-workshop-sep23-kohshi54/assets/80312261/33e2e15d-4de2-4964-bf38-4f5c97b235e3">
From the sp value being decremented by 16, it can be seen that the stack pointer is actually increasing forward from a higher to a lower address!!



