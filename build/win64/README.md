# GasModels for Windows

These are instructions for installing and packaging GasModels for 64-bit Windows 7/10.

## Installing GasModels

1. Download and install [Julia](https://julialang-s3.julialang.org/bin/winnt/x64/1.1/julia-1.1.1-win64.exe), [Gurobi](https://packages.gurobi.com/8.1/Gurobi-8.1.1-win64.msi) and [SCIP](https://scip.zib.de/download.php?fname=SCIPOptSuite-6.0.1-win64-VS15.exe).
1. Set `GUROBI_HOME` environment variable.
```
GUROBI_HOME=C:\gurobi811\win64
```
1. Set `SCIPOPTDIR` environment variable.
```
SCIPOPTDIR=%ProgramFiles%\SCIPOptSuite 6.0.1\
```
1. Open Julia (`%LOCALAPPDATA%/Julia-1.1.1/julia.lnk`) and install the required packages.
```
using Pkg
Pkg.add("GasModels")
Pkg.add("JuMP")
Pkg.add("Ipopt")
Pkg.add("Cbc")
Pkg.add("Juniper")
Pkg.add("Gurobi")
Pkg.add("SCIP")
```
**NOTE: I haven't figured out how to build the SCIP package yet. [`Libdl.dlopen_e()`](https://github.com/SCIP-Interfaces/SCIP.jl/blob/121a1eaeeb1b8ff9bb848a57db3362843de7260d/deps/build.jl#L39) doesn't seem to recognize `scip.dll` as a library. These instructions won't work until that part gets figured out.**
1. Open a command prompt and run the test scripts in `%USERPROFILE/.julia/packages/GasModels/lv2oh/test`.
```
%USERPROFILE/.julia/packages/GasModels/lv2oh/test/runtests.jl
%USERPROFILE/.julia/packages/GasModels/lv2oh/test/runtests_long.jl
```
Note `lv2oh` might be a different value on your install. This will take a while to run, but shouldn't have any errors. If you have errors, check the [Troubleshooting](#troubleshooting) section.

<a name="troubleshooting"></a>
## Troubleshooting

### ERROR: failed to clone from https://github.com/JuliaRegistries/General.git
You might need to set up proxy for git. Open up Powershell and create a new file `.gitconfig` in your home directory.
```
New-Item -Path '%USERPROFILE%/.gitconfig' -ItemType File
```
Add your proxy info to the file.
```
[http]
proxy = http://user:pass@proxyhost:port

[https]
proxy = http://user:pass@proxyhost:port
```
Now you should be able to install packages.
