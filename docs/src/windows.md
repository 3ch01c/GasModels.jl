# Installing GasModels on Windows
These instructions were last tested with [GasModels version 0.4.1](https://github.com/lanl-ansi/GasModels.jl/releases/tag/v0.4.1).

Either download and run the [Julia 1.1.1 installer](https://julialang-s3.julialang.org/bin/winnt/x64/1.1/julia-1.1.1-win64.exe) or [build Julia from source](#build-julia).

<a name="build-julia"></a>
## Build Julia from source
Skip to [installing GasModels](#install-gasmodels) if Julia is already installed.

### Download Julia source
Install [Git](https://git-scm.com/download/win). Download [Julia source](https://github.com/JuliaLang/julia).
```
git clone https://github.com/JuliaLang/julia.git
```

### Build Julia with Cygwin
These instructions follow the [official build instructions](https://github.com/JuliaLang/julia/blob/master/doc/build/windows.md).

Download and install [Cygwin](https://www.cygwin.com/install.html) with the necessary packages.
```
setup-x86_64.exe -s http://mirror.cs.vt.edu -q -P cmake,gcc-g++,git,make,patch,curl,m4,python,p7zip,mingw64-i686-gcc-g++,mingw64-i686-gcc-fortran,mingw64-x86_64-gcc-g++,mingw64-x86_64-gcc-fortran
```
Set the `XC_HOST` variable.
```
echo 'XC_HOST = i686-w64-mingw32' > Make.user     # for 32 bit Julia
# or
echo 'XC_HOST = x86_64-w64-mingw32' > Make.user   # for 64 bit Julia
```
Build the Julia core. The binaries will be built in `usr\bin`.
```
make -j 4 # Adjust the number of threads (4) to match your build environment.
```
Build additional tools for Julia on Windows. These will be built in `dist-extras`.
```
make win-extras
```
Package the binaries from `usr\bin` and `dist-extras`.

## Optional: Add Julia to system path
Add the Julia binary path (e.g., `%LOCALAPPDATA%\Julia-1.1.1\bin`) to the system path.
```
Path=%Path%;%LOCALAPPDATA%\Julia-1.1.1\bin
```

<a name="install-gasmodels"></a>
## Install GasModels
In Julia, install the GasModels package and its dependencies.
```
add GasModels # or `add https://github.com/lanl-ansi/GasModels.jl` for the latest, potentially unstable, version
add InfrastructureModels
add Memento
add JuMP
add Ipopt
add Cbc
add Juniper
```

<a name="install-extra"></a>
## Optional: Install extra packages
Some packages require installation of extra binaries.

### Optional: Install Gurobi
Download and install [Gurobi 8.1.1](https://packages.gurobi.com/8.1/Gurobi-8.1.1-win64.msi). Specify a [license token server](https://www.gurobi.com/documentation/8.1/quickstart_mac/creating_a_token_server_cl.html#subsection:clientlicensetoken) in a configuration file (e.g., `%USERPROFILE/gurobi.lic`).
```
TOKENSERVER=license.example.com
```
Set the `GUROBI_HOME` and `GRB_LICENSE_FILE` environment variables.
```
GUROBI_HOME=C:\gurobi811\win64
GRB_LICENSE_FILE=%USERPROFILE%\gurobi.lic
```
In Julia, install the Gurobi package.
```
add Gurobi
```

### Optional: Install SCIP
Download and install [SCIP 6.0.1](https://scip.zib.de/download.php?fname=SCIPOptSuite-6.0.1-win64-VS15.exe). Add SCIPOptSuite to the system path when prompted, or else manually add `%ProgramFiles%\SCIPOptSuite 6.0.1\bin` to the system path.
```
Path=%Path%;%ProgramFiles%\SCIPOptSuite 6.0.1\bin
```
Set the `SCIPOPTDIR` environment variable.
```
SCIPOPTDIR=%ProgramFiles%\SCIPOptSuite 6.0.1\bin
```
In Julia, install the SCIP package.
```
add SCIP
```

### Optional: Install ECOS
In Julia, install the ECOS package.
```
add ECOS
```

### Optional: Install SCS
In Julia, install the SCS package.
```
add SCS
```

### Optional: Install CPLEX
Download and install [Visual C++ Redistributable 2015](https://www.microsoft.com/en-us/download/details.aspx?id=48145) and [CPLEX 12.9.0](https://www.ibm.com/products/ilog-cplex-optimization-studio/pricing). Add the environment variables when prompted during the installation, or else manually set the `CPLEX_STUDIO_BINARIES129` environment variable.
```
CPLEX_STUDIO_BINARIES129=%ProgramFiles%\IBM\ILOG\CPLEX_Studio_Community129\cplex\bin\x64_win64
```
In Julia, install the CPLEX package.
```
add CPLEX
```
If using a Windows 7 environment, download and install [.NET Framework 4.5.2](https://www.microsoft.com/en-us/download/details.aspx?id=42642).

### Optional: Install Couenne
Download and extract [Couenne 0.3.2](https://www.coin-or.org/download/binary/Couenne/Couenne-0.3.2-win32-msvc9.zip). Add Couenne (e.g., `%LOCALAPPDATA%\Couenne-0.3.2-win32-msvc9\bin`) to the `Path` environment variable.
```
Path=%Path%;%LOCALAPPDATA%\Couenne-0.3.2-win32-msvc9\bin
```
### AmplNLWriter
In Julia, install the AmplNLWriter package.
```
add AmplNLWriter
```

## Test installation
Open a command prompt and run the GasModels test script.
```
julia.exe %USERPROFILE\.julia\packages\GasModels\lytJG\test\runtests.jl
```
The subdirectory `lytJG` is unique to GasModels version 0.4.1. Different versions will have different subdirectories. These tests take a while. If the tests fail, check the [Troubleshooting](#troubleshooting) section for help.

To test additional packages, use the long test script.
```
julia.exe %USERPROFILE\.julia\packages\GasModels\lytJG\test\runtests_long.jl
```

## Packaging for offline distribution
After successfully testing, make a copy the Julia package directory (e.g., `%USERPROFILE%\.julia`).
```
cp -r .julia dist/
```
Remove the `compiled` directory from the copy.
```
rm -rf dist/.julia/compiled
```
Optional: add instructions to install additional packages (e.g., Gurobi, SCIP, etc.).

Package the copy with the Julia installation executable.
```
cd dist
zip -r julia_win64.zip .julia julia-1.1.1-win64.exe
```
Distribute the package file (e.g. `julia_win64.zip`).

<a name="troubleshooting"></a>
## Troubleshooting

### Reinstalling Julia packages
If Julia environment winds up in an unclean state, it may be necessary to delete `%USERPROFILE%\.julia` entirely before reinstalling packages.

### Missing Julia core libraries
If Julia is built in a path that does not exist in the runtime environment, some library paths will not resolve correctly. To avoid this, build and install Julia in the same path (e.g., `C:\julia`).

### Cannot download Git repository
If the environment uses a proxy, add the proxy configuration to the Git configuration file (e.g., `%USERPROFILE%\.gitconfig`).
```
[http]
proxy = http://user:pass@proxyhost:port

[https]
proxy = http://user:pass@proxyhost:port
```
Other tools may require the `http_proxy` and `https_proxy` environment variables to be set. The [`TimeZones` Julia package currently doesn't support proxies](https://github.com/JuliaTime/TimeZones.jl/issues/206).

### Invalid Gurobi license
Configure the Gurobi configuration file (e.g., `%USERPROFILE%\gurobi.lic`).
```
TOKENSERVER=license.example.com
```
Set the `GRB_LICENSE_FILE` environment variable.
```
GRB_LICENSE_FILE=%USERPROFILE%\gurobi.lic
```

### No permission to read packages
Delete the compiled packages directory (e.g., `%USERPROFILE%\.julia\compiled`).
