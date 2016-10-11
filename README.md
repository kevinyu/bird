# bird

## Setup

Clone repository and initialize submodules

```
$ git clone https://github.com/kevinyu/bird.git
$ git submodule update --init
```

Setup virtualenv. Note: Our project uses Python 2.7 due to hdnet dependency, but we should try to keep the code Python 3 compatible by importing from `__future__`.

```
$ virtualenv env -p python2.7     # initialize virtualenv
$ source env/bin/activate         # activate virtualenv
```

Install tensorflow from source (installing tensorflow with pip has issues with CUDA 8.0). [Instructions here for Ubuntu 16.04, CUDA 8.0](https://alliseesolutions.wordpress.com/2016/09/08/install-gpu-tensorflow-from-sources-w-ubuntu-16-04-and-cuda-8-0-rc/). If not installing with GPU support can probably just install tensorflow using pip and skip all following steps: `pip install tensorflow`

```
$ cd ~/tensorflow
$ ./configure       # Here specify GPU support, CUDA versions
                    # Make sure you run from within virtualenv
```

Next build tensorflow, this step will take a while (20 minutes on i5-6600K, 16GB RAM)
```
$ bazel build -c opt --config=cuda //tensorflow/tools/pip_package:build_pip_package
```

Finally, build the pip package and install the newly built tensorflow using pip. Feel free to replace `~/tmp/tensorflow_pkg` with a different destination in your filesystem if you think you might want to reinstall it without rebuildilng (in other projects for example).

```
$ bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
```

```
$ pip install /tmp/tensorflow_pkg/tensorflow-0.11.0rc0-py2-none-any.whl
```
