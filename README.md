# WASI-pyperformance

## Build:

### Clone this repository:

    git clone --recursive https://github.com/julenbhy/WASI-python-benchmarks

### Install WASI-SDK at the deffault path:
    curl -sL https://github.com/WebAssembly/wasi-sdk/releases/download/wasi-sdk-22/wasi-sdk-22.0-linux.tar.gz | sudo tar -xz -C /opt/ && sudo mv /opt/wasi-sdk-22.0 /opt/wasi-sdk

### Install latest Wasmtime:
    curl https://wasmtime.dev/install.sh -sSf | bash


### Build the repository:
    ./build.sh


## Run

WASI uses a capability-based security model. This means that the WASI host does not give full access to your machine. Files clould be mapped to a different path inside the WASI host. So, if you try passing a file path to python.wasm/ python.sh, it needs to match the path inside the WASI host, not the path on your machine (much like using a container).

### Run Python interactive shell:

    ./run_python.sh 

#### Run test code

    ./run_python.sh benchmarks/test_codes/test.py

## Pyperformace benchmark

The [pyperformance benchmark](https://github.com/python/pyperformance) employs the [Pyperf library](https://github.com/psf/pyperf). This library spawns subprocesses to execute the target code and ultimately display the results. Currently, WASI does not support process creation, prompting the adoption of the following approach:

Initially, the benchmark is launched using standard Python. Subsequently, the Pyperf library is modified by substituting the file _manager.py with _manager_wasm.py, which is a modified version that directs subprocess execution to the CPython interpreter compiled to WebAssembly (Wasm). Consequently, a standard CPython instance handles the benchmark management, while a CPython.wasm instance executes the function under evaluation.

    ./run_pyperformance.py

Currently running a single benchmark

## Adding additional libraries:
Download the Python code of the required library and add it to the directory cpython/Lib. You can follow the example of pyperf library.

