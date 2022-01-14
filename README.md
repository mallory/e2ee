# Building draft

Install dependencies:

      # apt install ruby-kramdown-rfc2629 xml2rfc

On OS X:

Use brew to install python and ruby - use pyenv tailor your
environment to a consistent python environment.

Once that is done:

	# pip install xml2rfc
	# gem install kramdown-rfc2629
	
Draft editing workflow:

      # $EDITOR draft-name.md
      # make
