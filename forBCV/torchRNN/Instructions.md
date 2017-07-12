## Instructions for torch-RNN
This is some code we found about LSTMs, written by jcjohnson, whose git can be found at https://github.com/jcjohnson/torch-rnn
He has quite come instructions in his git, so we'll make a simple summary of that, regarding the code he wrote (in torch) and how we can use it with different inputs to produce different outputs for our purposes.
In this case, the network works with language, so the input is always text.
*The most remarkable thing about all of this is that the network starts from 0, meaning that everything you'll see was learned from scratch. Pay attention to the fact that it divides was it writes into words, then usage of markers like '.',',' and the apostrophe itself. Depending on what we feed it, it can learn what a play looks like, a wikipedia article (even with the citations and everything) or even code!*

