Wrapper project to deploy MediaThread
=====================================

Get the MediaThread code and submodules::

	$ git clone --recursive git://github.com/natea/mediathread_deploy.git
	$ cd mediathread_deploy/mediathread
	$ git submodule init
	$ git submodule update

Deploy it::

	$ stackato target api.stackato.local
	$ stackato login   (you will have had to register a user first)
	$ stackato push -n

Subsequent deploys::

	$ stackato update -n

Login to MediaThread::

	Point browser at http://mediathread.stackato.local
	un: admin
	pw: secret123
