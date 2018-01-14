# Setting up on macOS with Anaconda (for Jupyter Notebooks)

My MacBook Air is running High Sierra Version 10.13.3.

## Installing specific Python version and needed dependencies

### Python 2.7.12

Since we're trying to mimick what's installed on `buffet01.cc.gatech.edu` (accessing via ssh), it's important to know what version of Python is installed there. We can check by using `python --version`:

```
xxxxx@buffet01:~$ python --version
Python 2.7.12
```
So let's install install [Python 2.7.12](https://www.python.org/downloads/release/python-2712/). I installed using the "Mac OS X 64-bit/32-bit installer". After the installation completed, this version of Python was accessible via `pythonw` in my terminal.

### Installing virtualenv(wrapper)

Next, we need to install virtualenv and virtualenvwrapper via `pip`. First issue I had was that virtualenvwrapper wouldn't install:

```
sudo pip install virtualenvwrapper
...
...
OSError: [Errno 1] Operation not permitted: '/tmp/pip-wP0Vmr-uninstall/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/six-1.4.1-py2.7.egg-info'
```
Apparently there's an [issue with the six package](https://github.com/pypa/pip/issues/3165), which I got around by doing this:

```
sudo -H pip install --ignore-installed six
```

Now installing virtualenvwrapper (`sudo pip install virtualenvwrapper`) worked fine. 

### Setting up virtualenv

First, there was an issue with the virtualenvwrapper functions finding `virtualenvwrapper.sh`. It ended up in `/usr/bin/virtualenvwrapper.sh`, instead of under `/usr/local/bin`. To [fix this](https://stackoverflow.com/questions/13855463/bash-mkvirtualenv-command-not-found), I ran:

```
source `which virtualenvwrapper.sh`
```

This appeared to work, so finally, using this file (called `ml4t_lib.txt`) containing the dependencies:

```
 cycler==0.10.0
 functools32==3.2.3.post2
 matplotlib==2.0.2
 numpy==1.13.1
 pandas==0.20.3
 py==1.4.34
 pyparsing==2.2.0
 pytest==3.2.1
 python-dateutil==2.6.1
 pytz==2017.2
 scipy==0.19.1
 six==1.10.0
 subprocess32==3.2.7

```

I ran `mkvirtualenv -r ml4t_lib.txt -p pythonw jupyter_kernel_ml4t`. 

After this runs, and everything installs, you can tell you're in your virtual environment by seeing `(jupyter_kernel_ml4t)` in your terminal.

Next, I installed the ipython kernel `pip install ipykernel`, and ran:

```
 python -m ipykernel install --user --name jupyter_kernel_ml4t --display-name "ML4T"

Installed kernelspec jupyter_kernel_ml4t in /Users/snd/Library/Jupyter/kernels/jupyter_kernel_ml4t
```

Then `deactivate` to deactivate the environment.


## Jupyter Notebook

Now, finally, you should have a new Jupyter Notebook Kernel, so just start Jupyter Notebook. First, go to your environment that you've created. Mine is in `/Users/snd/.virtualenvs/`. Next cd to `jupyter_kernel_ml4t` and activate your virtualenv by running `source bin/activate`. Finally, run:


```
jupyter notebook
```

But my system came back with an error: 

```
(jupyter_kernel_ml4t) snd:jupyter_kernel_ml4t snd$ jupyter notebook
Error executing Jupyter command 'notebook': [Errno 2] No such file or directory
```

To resolve this, I ran `pip install --upgrade --force-reinstall --no-cache-dir jupyter`.

_Now_ running `jupyter notebook` worked correctly. When you go to `http://localhost:8888/tree` in your browser, you'll see that a kernel called "ML4T" is an option.
