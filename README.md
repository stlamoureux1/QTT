## Installation notes for xfac on Apple Silicon

On my machine it took some work to the python bindings for xfac to work correctly. This information may be helpful to anyone running the code here.

Machine specs:
- Apple Silicon
- Python 3.13 installed via pyenv
- OpenMP installed via homebrew

I had to modify the CMakeLists.txt provided with xfac. The modified version lives on the cmake-apple-silicon branch of my fork of xfac, a submodule of this project.

To build the xfac .so file that will provide python bindings, I did the following:

1. Set up a venv for this project and activate it
2. Use the flags below with cmake:

```
cmake -S . -B build -D \
	CMAKE_BUILD_TYPE=Release \
	-D XFAC_BUILD_PYTHON=ON \
	-D Python3_EXECUTABLE=/path/to/.venv/bin/python \
	-D Python3_FIND_VIRTUALENV=ONLY \
	-D CMAKE_POLICY_VERSION_MINIMUM=3.5 \
	-D OpenMP_ROOT=$(brew --prefix libomp)
```

3. Create a path file in `.venv/lib/python<VERSION>/site-packages` named `xfacpy.pth`. This file should contain the path to yourpython build of xfacpy, i.e. `../QTT/xfac/build/python`

It's important to run cmake from the activated virtual environment so that the python versions are aligned. The `import` command will be looking for a specific filename that contains the version number.
