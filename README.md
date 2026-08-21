# BART installation Instructions(MacOS) for arm(M3)

## 1. Xcode CommandLineTools is required

```bash
xcode-select –install
```

## 2. download BART source code

```bash
git clone https://codeberg.org/mrirecon/bart.git
```

## 3. brew install dependency

```bash
brew install –cask gcc-arm-embedded
brew install libpng
brew install fftw
brew install openblas
brew install make
brew install llvm libomp
```

## 4. change some codes for Makefile

### a. for libomp

```c
# LDFLAGS += “-L/usr/local/opt/libomp/lib” -lomp
# CPPFLAGS += “-I/usr/local/opt/libomp/include” -Xclang -fopenmp

LDFLAGS += “-L/opt/homebrew/opt/libomp/lib” -lomp
CPPFLAGS += “-I/opt/homebrew/opt/libomp/include” -Xclang -fopenmp
```

### b. for fftw

```c
# FFTW_BASE ?= /opt/local/
FFTW_BASE ?= /opt/homebrew/opt/fftw/
```

### c. for openblas

```c
BLAS_BASE ?= /usr/local/opt/openblas/
BLAS_BASE ?= /opt/homebrew/opt/openblas/
```

### d. for png

```c
# png
ifeq ($(PNG), 0)
PNG_L :=
CFLAGS += -DNO_PNG
CPPFLAGS += -DNO_PNG
else
PNG_L := -lpng
PNG_L := -L/opt/homebrew/opt/libpng/lib -lpng
CPPFLAGS += -I/opt/homebrew/opt/libpng/include
endif
```

## 5.code changes in src/mobafit.

```c
case SIM:
{
const complex float *b1 = NULL;
……

}
```

### code change in /src/phantom.c

```c
case STL:
{
// prepare phantom sampling grid
struct grid_opts gopts = grid_opts_defaults;
gopts.kspace = kspace;
……
}
```

**Same changes in src/sqpics.c src/grecon/optreg.c src/stl/misc.c src/simu/phantom.c src/stl/misc.c and So On (C language syntax incompatibility in Mac**

## 6.compile for bart

```bash
CC=gcc MACPORTS=0 gmake
```

## 7.add path for bart

```bash
export BART_TOOLBOX_PATH=“Your_path/bart"
export PATH="{BART_TOOLBOX_PATH}:${PATH}”
```