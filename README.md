# Assignment — Altair & JupyterLab Setup
​
This repository is your starting point for the assignment and includes the instructions below.

# Aim of the assignment

[Altair](https://altair-viz.github.io/) is a declarative statistical visualization library for Python, based on [Vega](http://vega.github.io/vega) and [Vega-Lite](http://vega.github.io/vega-lite).
We will use it in class so we want to get set up with it on your laptop.

If you run into problems see the [Troubleshooting section](#troubleshooting) below.and look at the source code and documentation for Altair on [GitHub](http://github.com/altair-viz/altair).

# Instructions

## Setup instructions

1. Clone the repo.
1. `CD` to the repo directory. Create and activate a virtual environment for this project. You may need to modify the code you use depending on what Python you have installed and how your machine is configured.

1. Run the setup commands below.

    * On macOS or Linux, run these three commands *separately* in case there are errors:
        ```
        python3 -m venv env
        ```
        ```
        source env/bin/activate
        ```
        ```
        which python
        ```
    * On Windows, run these three commands *separately* in case there are errors:
        ```
        python -m venv env
        ```
        ```
        .\env\Scripts\activate.bat
        ```
        ```
        where.exe python
        ```
    Check the path(s) provided by `which python` or `where.exe python` — the first one listed *should* be inside the `env` folder you just created.

1. Install necessary packages. Note that you should install the exact versions of the packages.
    ```
    pip install -r requirements.txt
    ```
    This may take a few minutes.

If you have trouble running any of these steps, see the [Troubleshooting section](#troubleshooting) below.

## Run instructions

1. Run `jupyter lab`. It should open your default browser and let you select select any Jupyter Notebook .ipynb file.

1. Open `notebook.ipynb`.

1. Run individual cells with `ctrl+enter`. In the menu you can run all cells and restart the kernel to clear variables.

1. Make sure there are not any errors!
​
## Quit instructions
1. Make sure to save your .ipynb file and shutdown Jupyter Lab properly through the file menu. Otherwise you need to use `jupyter notebook stop`.
​
1. Deactivate the venv to return to your terminal using `deactivate`.

## Commit and push your code (but first...)

1. Only if you have made any changes to the required packages you should export a list of all installed packages and their versions:
   ```
   pip freeze > requirements.txt
   ```

1. **Before you commit a Jupyter Notebook .ipynb file, clear the outputs of all cells.** This decreases file size, removes unnecessary metadata, and makes diffs easier to understand. In Jupyter Lab you can use the GUI: Edit->Clear All Outputs.

1. Finally, commit all your local code and push it to your remote GitHub Classroom-generated repository.

### Optional git setup to automatically clear metadata using JQ

This step is only recommended if you plan to use JupyterLab often.

1. Install JQ by running, e.g.,
    ```
    sudo apt-get install jq
    ```
    For more options including your operating system check [the JQ download site](https://stedolan.github.io/jq/download/).
2. Append the following block of code either in your local repo `.gitconfig` file or your global `.gitconfig`. I would recommend to do it in your global `.gitconfig` so you don't need to redo that for future .ipynb files.<br>
    ```
    [core]
    attributesfile = ~/.gitattributes_global
    [filter "nbstrip_full"]
    clean = "jq --indent 1 \
            '(.cells[] | select(has(\"outputs\")) | .outputs) = []  \
            | (.cells[] | select(has(\"execution_count\")) | .execution_count) = null  \
            | .metadata = {\"language_info\": {\"name\": \"python\", \"pygments_lexer\": \"ipython3\"}} \
            | .cells[].metadata = {} \
            '"
    smudge = cat
    required = true
    ```
    For more details look at this great tutorial [here](http://timstaley.co.uk/posts/making-git-and-jupyter-notebooks-play-nice/).
    You can use this command 
    ```
    git config --list --show-origin --show-scope
    ```
    to find out where your git configuration is stored.
3. Create a global gitattributes named `.gitattributes_global` file (usually placed at the root level, so `~/.gitattributes_global`).
4.  Add the following line of code:
    ```
    *.ipynb filter=nbstrip_full
    ```

# Optional features

* To get markdown section numbering use [jupyterlab-toc](https://github.com/jupyterlab/jupyterlab-toc).
  * To install run:
    ```
    jupyter labextension install @jupyterlab/toc
    ```
  * Then click the enumerated list button on the left strip in JupyterLab to bring up the table of contents. There you can click the itemized list button in the top to add section numbers to the markdown cells.
​
* There is also a useful [Spellchecker](https://github.com/ijmbarr/jupyterlab_spellchecker) extension.

  *  To install run: 
        ```
        jupyter labextension install @ijmbarr/jupyterlab_spellchecker
        ```
  
* To install both of the above wtih one (faster) command run: 
    ```
    jupyter labextension install @jupyterlab/toc @ijmbarr/jupyterlab_spellchecker
    ```


## <a name="troubleshooting"></a> Troubleshooting

### Python issues
* Are you using python 3.6 or newer? Check inside your virtual environment by running `python --version`. If not, [download and install](https://www.python.org/downloads/) the updated version of python for your OS.

* Are you using the [Anaconda Distribution](https://www.anaconda.com/distribution/)? We've had nothing but trouble using Anaconda with Jupyter Lab. See the instructions at the end of the [venv virtual environment section](#anaconda) below.

* If you get a `NotImplementedError` for `asyncio` while running Python 3.8,
edit `/env/Lib/site-packages/tornado/platform/asyncio.py` following the instructions [here](https://stackoverflow.com/questions/58422817/jupyter-notebook-with-python-3-8-notimplementederror/). Right after the line `import asyncio` add these lines:
    ```
    import sys
    if sys.platform == 'win32':
        asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())
    ```

### venv virtual environment issues
* Are you in your virtual environment? Your command prompt / terminal prompt should be prefixed with `(env)` to show you that.

* Are you using the python executable from your virtual environment?
    1. Check it!
        * On macOS or Linux, run:
            ```
            which python
            ```
        * On Windows, run:
            ```
            where.exe python
            ```
            Check the path(s) provided by `which python` or `where.exe python` — the first one listed *should* be inside the `env` folder you just created.
    
    1. If the first listed path is not inside the `env` folder you just created, then find a way to run the correct python executable.
    
        * <a name="anaconda"></a>One common problem is if you have the [Anaconda Distribution](https://www.anaconda.com/distribution/) installed. E.g., your first listed path matches [one of these](https://docs.anaconda.com/anaconda/user-guide/tasks/integration/python-path/). In that case, one fix is to uninstall Anaconda completely ([option B listed here](https://docs.anaconda.com/anaconda/install/uninstall/)) and install basic [Python](https://www.python.org/downloads/) (if you don't have it already).

* Did you rename your `env` folder after creating it? If so, delete it and run the commands to create it again. `venv` uses hard-coded paths so renaming the folder is fraught.

### Other pip install issues

* You may get a warning like `WARNING: You are using pip version 20.1.1; however, version 20.2.3 is available. You should consuder upgrading...` You don't need to worry about fixing this.

* You may run into issues where pip is using a different python than Jupyter Lab is running. E.g., you may install a package but then Jupyter complains it is unavailable. In that case:
    1. instead of `pip install` try running 
        ```
        python -m pip install
        ```
    1. Additionally, check if python is from the same environment as pip:
        * On macOS or linux: `which pip` or `which pip3` and `which python`
        * On Windows: `where.exe pip` or `where.exe pip3` and `where.exe python`

* You can also try installing the required packages without pinning to particular versions like we have done in `requirements.txt`. Do this by running:
    ```
    pip install -r requirements-noversions.txt
    ```
    You can also run the installs one-by-one to see if there are issues. E.g., `pip install altair`.

### Windows issues

* If you are using PowerShell (not the Command Prompt) and you get an error message saying `the execution of scripts is disabled on this system`, follow these steps.
    1. Open a new PowerShell as Administrator.
    1. enable running unsigned scripts by entering 
        ```
        set-executionpolicy remotesigned
        ```
        See [the documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-7) for details.

* If you get this error `numpy.distutils.system_info.NotFoundError: no lapack/blas resources found` try installing it manually. (Instructions modified from [here](https://github.com/scipy/scipy/issues/5995).)

    1. **Open Powershell**, `CD` to your repo folder, and **enter your virtual environment**.
    
    1. Download numpy+mkl wheel from one of the links [here](http://www.lfd.uci.edu/~gohlke/pythonlibs/#numpy). Use the version that is the same as your python version (check using `python --version`). E.g., if your python is 3.6.2, download the wheel which shows cp36. E.g., for python 3.9:
        ```
        wget https://download.lfd.uci.edu/pythonlibs/x2tqcw5k/numpy-1.19.2+mkl-cp39-cp39-win_amd64.whl -OutFile numpy.whl
        ```
    
    1. Install the wheel:
        ```
        pip install numpy.whl
        ```
    
    1. Likewise, install SciPy from one of the links [here](http://www.lfd.uci.edu/~gohlke/pythonlibs/#scipy) using the same version as your python. E.g., for python 3.9:
        ```
        wget https://download.lfd.uci.edu/pythonlibs/x2tqcw5k/scipy-1.5.2-cp39-cp39-win_amd64.whl -OutFile scipy.whl
        ```
        ```
        pip install scipy.whl
        ```

### Mac issues

* When you run `pip install -r requirements.txt`, `pip install numpy`, or `pip install scipy` you may get this error: `RuntimeError: Broken toolchain: cannot link a simple C program`. Note that this error may be in the middle / end of a large error message. It means that [Gcc](https://gcc.gnu.org/) is not available for compiling C programs (which Python is based on). Follow these steps:
    
    1. Try running
        ```
        brew help
        ```
        to see if you have [Homebrew](https://brew.sh/) installed. If you get `command not found`, install Homebrew by running:
        ```
        /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
        ```
    1. Then run 
        ```
        brew install gcc
        ```
    1. Try running this again:
        ```
        pip install -r requirements.txt
        ```

### Altair issues

* Read the relevant part of [the Altair documentation](https://altair-viz.github.io/user_guide/API.html).

* Check [the UW Visualization Curriculum debugging guide](https://github.com/uwdata/visualization-curriculum/blob/master/altair_debugging.ipynb) and the [Altair FAQ](https://altair-viz.github.io/user_guide/faq.html) for answers.

* The developers of Altair sometimes release a new version via pip and update [the documentation](https://altair-viz.github.io/user_guide/API.html) before it is stable.
    Note that the documentation version and Altair version you are using may be out of sync. (See the top-left of the documentation page for the version number, and in Jupyter Lab run `alt.__version__` in a cell to see the Altair version). In this assignment, using the `requirements.txt` file we are asking you to install a particular version to avoid some issues like this.

### General troubleshooting advice

* Googling error messages is the best way to help you sort out weird issues with Python and Jupyter Lab.

* You can also post a discussion topic on Canvas and we and other students will try to help!


# Submission instructions

There is nothing to submit.

# Assignment setup (for teaching staff only)

See https://github.com/NEU-DS-4200-F20-Staff/Assignment_Setup_Instructions.