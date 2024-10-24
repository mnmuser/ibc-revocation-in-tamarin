# Identity-based Encryption Key Revocation Tamarin Model

The [Tamarin](https://tamarin-prover.github.io/) prover is a verification tool
for security protocols. (Please also refer to the [Tamarin documentation](https://tamarin-prover.github.io/manual/tex/tamarin-manual.pdf)). 

This project contains symbolic models for key revocation techniques in Identity-based Encryption (IBE), which can be used with Tamarin to verify the approaches'
security properties.
It is the basis for the PhD thesis "Formal Verification
of Revocation Approaches
in Identity-Based Cryptography" submitted and currently under review at [University of Munich, Computer Science Department](https://www.ifi.uni-muenchen.de/index.html).

There are one blueprint ("vorlage.txt") and four models (sorted alphabetically):
- _ind-token-rerand.spthy_ for the **Individual Token with Rerandomized Keys** approach
- _ind-token-sep.spthy_ for the **Individual Token with Separate Keys** approach
- _key-renewal.spthy_ for the **Key Renewal** approach
- _universal-token.spthy_ for the **Universal Token** approach


## How do I run this?

First you need to obtain the tamarin-prover binary from the package manager
of your choice:
* Arch Linux: `pacman -S tamarin-prover`
* Nixpkgs: `nix-env -i tamarin-prover`

More infos [here](https://tamarin-prover.github.io/manual/book/002_installation.html).

Then run the model via the command line tool:

	tamarin-prover interactive ikev2.spthy
or:

	tamarin-prover interactive pq-ikev2.spthy

And point your browser to:

	http://localhost:3001

### Hints for installing Tamarin on Ubuntu
We used Ubuntu 20.04, as 18.04 did not support the glibc 2.29, which is required by Tamarin.
Tamarin das not provide a package for Ubuntu/Debian by using apt/dpkg, hence we installed Tamarin with the homebrew package manager.

#### Installing Hombrew
If the package manager includes homebrew:

	sudo apt install linuxbrew-wrapper
	
Otherwise manual installation is required (as it is currently the case in Ubuntu 20.04 LTS):

	sudo apt install build-essential curl file git
	sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)"
	sudo brew install tamarin-prover/tap/tamarin-prover
	test -d ~/.linuxbrew && eval $(~/.linuxbrew/bin/brew shellenv)
	test -d /home/linuxbrew/.linuxbrew && eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)
	test -r ~/.bash_profile && echo eval" ($(brew --prefix)/bin/brew shellenv)" >>~/.bash_profile
	echo "eval $($(brew --prefix)/bin/brew shellenv)" >>~/.profile
	echo 'eval "$(/home/ubuntu/.linuxbrew/bin/brew shellenv)"' >> /home/ubuntu/.profile
	eval "$(/home/ubuntu/.linuxbrew/bin/brew shellenv)"
	
#### Installing Tamarin and its dependencies
	brew install gcc
	brew install font-util
	brew install tamarin-prover/tap/tamarin-prover
	brew install tamarin-prover/tap/maude graphviz haskell-stack
	export PATH=$PATH:$HOME/.linuxbrew/var/homebrew/linked/tamarin-prover/bin
	tamarin-prover

## Environment

The code ran in an virtual environment with 40 vCPU cores, 180 GB RAM and with standard Ubuntu 20.04. 
The models took the following times to complete:
- _ind-token-rerand.spthy_ : 26 minutes
- _ind-token-sep.spthy_ : 31 minutes
- _key-renewal.spthy_ : 14 minutes
- _universal-token.spthy_ : 1 hour 44 minutes

Please note that the (purely academic) minimal example took the vast majority of time to complete in each of these models. It is best commented out for much faster results (well below 15 mins in all cases, well below 2 minutes for key-renewal.spthy and ind-token-sep.sphty). 

Smaller setups may work; the bottleneck is memory, not CPU power.

## Results

To check the model, choose the correct file in the Tamarin GUI. In the tab that opens, the model code is on the left. 

Click "sorry" at the foot of whichever lemma you want to prove. This opens the "Visualization display". All lemmas can be checked by clicking "a. autoprove".

Depending on whether a lemma makes an "exists trace"-statement or an "all traces"-statements, different results are considered a success.
To prove an "exists trace"-statement, Tamarin will output a trace (as a picture) which shows a fulfilling trace for the lemma.
Proving an "all traces"-statement means that Tamarin finds no trace that contradicts the statement.

Lemmas that can be proven yield a green trace on the left and, in the case of "exists-trace"-lemmas, a picture of a fulfilling trace.
Lemmas that can be disproven yield a red trace on the left and, in the case of "all-traces"-lemmas, a picture of a contradicting trace.
