Wrapper project to deploy MediaThread
=====================================

Get the MediaThread code and submodules::

	$ git clone --recursive git://github.com/natea/mediathread_deploy.git
	$ cd mediathread_deploy/mediathread
	$ git submodule init
	$ git submodule update

Add to your mediathread/deploy_specific/settings.py::

    EXTRA_INSTALLED_APPS = (
            'django_stackato',
            )

    # ## Pull in CloudFoundry's production settings
    if 'VCAP_SERVICES' in os.environ:
        import json
        vcap_services = json.loads(os.environ['VCAP_SERVICES'])
        # XXX: avoid hardcoding here
    #    srv = vcap_services['postgresql-8.4'][0]
        srv = vcap_services['mysql-5.1'][0]
        cred = srv['credentials']
        DATABASES = {
            'default': {
    #            'ENGINE': 'django.db.backends.postgresql_psycopg2', # Add 'postgresql_psycopg2', 'postgresql', 'mysql', 'sqlite3' or 'oracle'.
                'ENGINE': 'django.db.backends.mysql', # Add 'postgresql_psycopg2', 'postgresql', 'mysql', 'sqlite3' or 'oracle'.
                'NAME': cred['name'],                      # Or path to database file if using sqlite3.
                'USER': cred['user'],                      # Not used with sqlite3.
                'PASSWORD': cred['password'],                  # Not used with sqlite3.
                'HOST': cred['hostname'],                      # Set to empty string for localhost. Not used with sqlite3.
                'PORT': cred['port'],                      # Set to empty string for default. Not used with sqlite3.
                }
            } 
    else:   
        DATABASES = {
            'default': {
                'ENGINE': 'django.db.backends.sqlite3', # Add 'postgresql_psycopg2', 'postgresql', 'mysql', 'sqlite3' or 'oracle'.
                'NAME': 'mediathread',                      # Or path to database file if using sqlite3.
                'USER': '',                      # Not used with sqlite3.
                'PASSWORD': '',                  # Not used with sqlite3.
                'HOST': '',                      # Set to empty string for localhost. Not used with sqlite3.
                'PORT': '',                      # Set to empty string for default. Not used with sqlite3.
                }
            }

**Note:** You'll need to make a file `__init__.py` in the `deploy_specific dir` in order for the files to be loaded.    

Try it out!
-----------

Login to MediaThread by pointing your browser at http://mediathread.stackato.local and use these login credentials (can be changed in stackato.yml)::

	un: admin
	pw: secret123
