## Instructions for torch-RNN
This is some code we found about LSTMs, written by jcjohnson, whose git can be found at https://github.com/jcjohnson/torch-rnn, so, first of all:

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

## Install torch
```
git clone https://github.com/torch/distro.git ~/torch --recursive
```
