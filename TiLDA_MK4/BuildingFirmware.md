MacOS and Linux, Windows Builds unknown

git clone --recurse-submodules
<https://github.com/micropython/micropython.git>

cd micropython

git submodule add git@github.com:emfcamp/Mk4-micropython-board.git
ports/ti

cd ports/ti

./inst_tools

make