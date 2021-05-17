# Windows
## Building wheel for blspy (PEP 517) error
Make sure you are running python 64bit. You can find out python versions installed by runnning
`py -0a`
You'll see something like this:

    Installed Pythons found by py Launcher for Windows
     (venv) *
     -3.8-64
     -3.8-32
 
If you only see a 32-bit version, install python 64-bit selecting the 'add to path' option. Once installed, run python on cmd/CLI to [check what version runs as default](https://stackoverflow.com/a/10966396/1202124)). If it isn't the 64 bit one, you can
a) uninstall python 32bit and/or delete it from your path
b) for multiple versions of python installed simultaneously (e.g. 3.8-64 and 3.8-32), make sure that the first python path in your path environment variable is the 64bit one, so python 64 runs as default. For more of a detailed explanation see [here](https://stackoverflow.com/questions/57328054/change-default-version-of-python-from-32bit-to-64bit/66102653#66102653)

## brun: unrecognized arguments error
This issue comes up when running ```brun '(+ 1 (q . 3))' '2'``` . The solution is to use double quotes instead of single quotes i.e. ```brun "(+ 1 (q . 3))" "2"```. That should work just fine.

## Debian 10
### ModuleNotFoundError: No module named 'clvm.CLMObject'
This issue shows up when using 'run/brun' commands. Credit to @sgharvey on keybase for the solution:

1. Add backports to your sources.list (or alternatively add a new file with the ".list" extension to /etc/apt/sources.list.d/)
```
deb http://deb.debian.org/debian buster-backports main
```

2. Run ```apt-get update```
3. Install cmake from backports
```
apt-get install cmake/buster-backports
```
4. Clone into clvm
```
git clone https://github.com/Chia-Network/clvm.git
cd clvm
```
5. Create venv
```
python3 -m venv venv
. ./venv/bin/activate
```
7. Install clvm into venv
```
pip install -e '.[dev]'
```
8. If you pay close attention, you'll notice that clvm_tools was installed. Test that by running 'run' or 'brun'.