PYPI Test mssql-scripter upload
========================================

### Requirements:
1. Create and Register a user account on testpypi at [test pypi](https://testpypi.python.org/pypi?%3Aaction=register_form)

2.  Add `<clone_root>` to your PYTHONPATH environment variable:
    ##### Windows
    ```
    set PYTHONPATH=<clone_root>;%PYTHONPATH%
    ```
    ##### OSX/Ubuntu (bash)
    ```
    export PYTHONPATH=<clone_root>:${PYTHONPATH}
    ```
3.	Install the dependencies:
    ```
    python <clone_root>/dev_setup.py clean
    ```


## Bump Version

	Versioning schema: {major}.{minor}.{patch}{release}{release_version}	
    Example: 1.0.0a0
To bump a particular segment of the version, from `<clone_root>` execute:
<pre>
bumpversion major              ->  <b>2</b>.0.0a0
bumpversion minor              ->  1.<b>1</b>.0a0
bumpversion patch              ->  1.0.<b>1</b>a0
bumpversion release            ->  1.0.0<b>rc</b>0
bumpversion release_version    ->  1.0.0a<b>1</b>
</pre>

**Note**: bumpversion does not allow version bumping if your workspace has pending changes.This is to protect against any manual updates that may have been made which can lead to inconsistent versions across files. If you know what you are doing you can override this by appending `--allow-dirty` to the bumpversion command.
	
## Build
1. Clean distribution folders:

    ##### Windows
      ```
      rmdir /s dist
      rmdir /s mssqltoolsservice\dist
      ```
  
    ##### OSX/Ubuntu (bash)
      ```
      rm -rf dist
      rm -rf mssqltoolsservice/dist
      ```
2. Build mssql-scripter source distribution and verify readme.rst, From `<clone_root>` execute:
    ```
    python setup.py check -r -s sdist
    ```

3. Build mssqltoolsservice wheels for each supported platform, From `<clone_root>/mssqltoolsservice` execute:
    ```
	python buildwheels.py
	```

	Build a OS-Specific wheel:
	```
    python buildwheels.py CentOS_7
    python buildwheels.py Debian_8
    python buildwheels.py Fedora_23
    python buildwheels.py openSUSE_13_2
    python buildwheels.py OSX_10_11_64
    python buildwheels.py RHEL_7
    python buildwheels.py Ubuntu_14
    python buildwheels.py Ubuntu_16
    python buildwheels.py Windows_7_64
    python buildwheels.py Windows_7_86
	```
4. Add a .pypirc configuration file:

    - Create a .pypirc file in your user directory:
        #### Windows: 
            Example: C:\Users\bob\.pypirc
		#### MacOS/Linux: 
            Example: /Users/bob/.pypirc
    
    - Add the following content to the .pypirc file, replace `your_username` and `your_passsword` with your account information created from step 1:
        ```
		[distutils]
		index-servers=
		    pypitest
		 
		[pypitest]
		repository = https://testpypi.python.org/pypi
		username = your_username
		password = your_password
        ```
4. Register and upload to pypi test server:
    
    ```
    python register_upload.py register pypitest
    ```
    
    ```
    python register_upload.py upload pypitest
    ```

5. Test install locally

	To install the local mssql-scripter pip package, from `<clone_root>` execute:
    ```
    sudo pip install --no-index -i ./mssqltoolsservice/dist/* ./dist/mssql-scripter-1.0.0a1.tar.gz
    ```

6. Test install via pypi server:

	**Note**: Specifying the test pypi server as the index to search for, pip will attempt to search for mssql-scripter's dependencies from the same server. This can result in a requirement not found error, but should not be a problem if dev_setup.py was ran during developer setup. If the error does occur, manually pip install the dependencies that are listed in setup.py and ensure the versions are correct.
	
	Install the mssql-scripter package that was just uploaded:
    ```
	pip install -i https://testpypi.python.org/pypi mssql-scripter
	```

	Upgrade to the mssql-scripter that was uploaded:
	```
    pip install --upgrade -i https://testpypi.python.org/pypi mssql-scripter
    ```
