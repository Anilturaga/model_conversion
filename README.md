<br>
<br>
<h1 align="center"> Model conversion</h1>


<div align="center">Tutorial repository for the model conversion from Python to C++ using TorchScript

· <a href="https://code.visualstudio.com/docs/remote/containers">Project made using remote containers</a> ·

<br>
</div>

## About the project

**Motivation**

* Python - dynamism and ease of iteration(Ideal for development)
* C++ - Multi threaded, precision based(Ideal for deployment)

### TorchScript

TorchScript is a way to create serializable and optimizable models from PyTorch code

Steps to convert a model

* First convert your model to Torch Script
    * Tracing
    * Converting via Annotation
* Serialize the script module to a file
* Loading Your Script Module in C++

**Lookouts**

* Since TorchScript is not dependent on python global interpreter lock, we can run inference of a model using multi-threading.
    * torch.jit.\_fork and torch.jit.\_wait
* Mixing tracing and scripting when a small part of a model requires some control-flow even though most of the model is just a feed-forward network. Control-flow inside of a script function called by a traced function is preserved correctly.
* Trace models seperately for GPU and CPU
* If you’re using Sequential, there is a known issue of inference errors. The solution is to subclass nn.Sequential and redeclare forward with the input typed correctly.

## Getting Started

Spin up a dev container in vs code using our .devcontainer/devcontainer.json

### Prerequisites

This is an example of how to list things you need to use the software and how to install them.

* Python 3.6
* Pip
* CMAKE -  - https://askubuntu.com/questions/355565/how-do-i-install-the-latest-version-of-cmake-from-the-command-line

### Installation

1. Install PyTorch

``` sh
pip3 install torch torchvision torchaudio
```

2. INSTALLING C++ DISTRIBUTIONS OF PYTORCH

``` sh
wget https://download.pytorch.org/libtorch/nightly/cpu/libtorch-shared-with-deps-latest.zip
```

3. Unzip the zip file

``` sh
unzip libtorch-shared-with-deps-latest.zip
```

## Usage

* main.ipynb
* example-app.cpp
* CMakeLists.txt

<br>
Commands for executing the script module in C++(Path meant for remote container environment)

```
mkdir build
cd build
cmake -DCMAKE_PREFIX_PATH=/workspaces/model_conversion/libtorch ..
cmake --build . --config Release
make
```

Finally

```
root@4b5a67132e81:/example-app/build# ./example-app ./traced_resnet_model.pt
ok
```

*For more examples, please refer to the [Documentation](https://pytorch.org/tutorials/advanced/cpp_export.html)*

## Contact

Naresh Shah - naresh@untangle.ai

Anil Turaga - anil@untangle.ai

Project Link: [https://github.com/github\_username/repo\_name](https://github.com/github_username/repo_name)