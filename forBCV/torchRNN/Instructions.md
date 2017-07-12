## Instructions for torch-RNN
This is some code we found about LSTMs, written by jcjohnson (thank you jcjohnson!), whose git can be found at https://github.com/jcjohnson/torch-rnn, so, first of all:

```bash
git clone https://github.com/jcjohnson/torch-rnn
cd torch-rnn/
```

He has quite come instructions in his git, so we'll make a simple summary of that, regarding the code he wrote (in torch) and how we can use it with different inputs to produce different outputs for our purposes.
In this case, the network works with language, so the input is always text.

*The most remarkable thing about all of this is that the network starts from 0, meaning that everything you'll see was learned from scratch. Pay attention to the fact that it divides what it writes into words, the usage of markers like '.',',' (and the apostrophe itself) and the syntax of the language. Depending on what we feed it, it can learn what a play looks like, a wikipedia article (even with the citations and everything) or even code!*

The next is, almost word by word, taken from his github:
## System setup
You'll need to install the header files for Python 2.7 and the HDF5 library. On Ubuntu you should be able to install
like this:

```bash
sudo apt-get -y install python2.7-dev
sudo apt-get install libhdf5-dev
```

## Python setup
The preprocessing script is written in Python 2.7; its dependencies are in the file `requirements.txt`.
You can install these dependencies like this:

```bash
pip install -r requirements.txt  # Install Python dependencies
```

## Install torch (taken from http://torch.ch/docs/getting-started.html#_) if you are OSX user, go to the 'OSX Installation' section 
```
git clone https://github.com/torch/distro.git ~/torch --recursive
cd ~/torch; bash install-deps;
./install.sh
```

and try this to verify installation
```
luarocks install image
luarocks list
```

After installing torch, you can install / update these packages by running the following:

```bash
# Install most things using luarocks
luarocks install torch
luarocks install nn
luarocks install optim
luarocks install lua-cjson

# We need to install torch-hdf5 from GitHub
git clone https://github.com/deepmind/torch-hdf5
cd torch-hdf5
luarocks make hdf5-0-0.rockspec
```

### CUDA support (Optional) -> I did not test this since I have OSX :(
To enable GPU acceleration with CUDA, you'll need to install CUDA 6.5 or higher and the following Lua packages:
- [torch/cutorch](https://github.com/torch/cutorch)
- [torch/cunn](https://github.com/torch/cunn)

You can install / update them by running:

```bash
luarocks install cutorch
luarocks install cunn
```
## OSX Installation
Jeff Thompson has written a very detailed installation guide for OSX that you [can find here](http://www.jeffreythompson.org/blog/2016/03/25/torch-rnn-mac-install/).


# Using this thing
We have to:
* Pre-process data
* Train the model
and then we can *Take samples from the model*
