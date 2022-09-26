# CCIG

## Table of contents

 1. [Introduction](#introduction)
 2. [Starting a Notebook](#starting-a-notebook)
 3. [Building the CCIG Grimoire](#building-the-ccig-grimoire)
 4. [Hosting the Grimoire](#hosting-the-grimoire)
 5. [Contributors](#contributors)
 6. [Credits](#credits)

## Introduction

This repo contains the files necessary to render the CCIG Grimoire hosted on jupyter books.

## Starting a Notebook

The ``book`` dir; within the main CCIG folder structure, contains the collection of notebooks currently rendered on the CCIG grimoire jupyterbook. Any new notebooks to be added onto this project can be written as ``.ipynb`` files (containing python or r scripts) or pure ``.md`` files. More information on adding contents to your own notebooks can be found on: <https://jupyterbook.org/en/stable/file-types/markdown.html>.

To make use of this repo:

1. Fiirst create a fork to your own github and then make a clone of your forked repo. 
   
2. Next you can open the repo you added using your chosen IDE . Please follow the relevant instructions for your IDE and Operating System on working with Git and GitHub repo's - for VS code you can find the instructions here: <https://code.visualstudio.com/docs/editor/versioncontrol#_git-support>.
   
3. Once you have set up the repo within your own environment you can start creating and working on your own notebook inside the ``book`` dir. 
   
4. To add a notebook onto the main jupyter book structure open the ``_toc.yml`` file and under the ``chapters`` section add the name of your notebook prefixed by ``-file:`` tag (e.g. - file: book/MyNewNoteBook ). (***NOTE:*** Makesure to name your notebook using non-capital letters as this can sometimes confuse the .toc file)
   
5. Once you are happy with the edits to your notebook and have added the notebook to the .toc file you can push the changes to your local Github repo. 
   
6. Finally, when you are ready to publish your notebook you can send a pull request to the main CCIG branch. (***NOTE:*** Please make sure you have comments on what has changed within your notebook when making a pull request as insufficiently commented pull requests may not be accepted.)
    
7. If the changes are accepted to the main repo you will be able to check out your rendered notebook within the CCIG Grimoire at <https://devasenevi.github.io/CCIG/>.

## Building the CCIG Grimoire

You can follow the steps below to develop and/or build the CCIG Grimoire locally:

1. Clone this repository.
   
2. Run `pip install -r requirements.txt` (Note: It is recommended you do this within a virtual environment such as conda.)
   1. To run the above command you will need to have python installed within your virtual env.
   2. The best way to do this is to create an env with python and pip in the first place. To do that run `conda create -n python=<python_version_number> <name_of_your_env> pip` (e.g. conda create -n python=3.7 yourenv pip).
   3. Once the env is created you can run `conda activate name_of_your_env` to activate your env.
   4. Now you are ready to install the requirments.txt (Make sure your active directory is the directory which houses the CCIG folder e.g. ~/user/TheJupyterBookDir/CCIG/).
   
3. Now you can edit the books source files located in the `CCIG/` directory 
   1. If you only wiah to render the current CCIGG locally then you can proceed to step 4 without any edits to the files.
   2. If you wish to add your own notebook to the Grimoire, then proceed to create a .ipynb or .md file as mentioned within the previous section and edit the .toc file as necessary.
   
4. Next set the active directory to the directory right above the current directory (i.e. instead of ../TheJupyterBookDir/CCIG/ it shoud be pointing to ../TheJupyterBookDir/)
   
5. Run `jupyter-book clean CCIG/` to remove any existing builds.
   
6. Run `jupyter-book build CCIG/` to build the book.
   
7. Once the build is complete you will find a URL at the bottom of the terminal (e.g. 'file://.../CCIG/_build/html/index.html'. Copy and paste this on your browser to see the rendered notebook.

***NOTE: Do not set your cloned CCIG repo as public. Rendering of the CCIGG for local use and edits should only be done locally within your PC (i.e. local build commands as discribed above) and not through the publipublicly available gh-pages version.***

## Hosting the Grimoire

The current CCIG Grimoire uses Github pages to host and track changes to itself.

## Contributors

We welcome and recognize all contributions. You can see a list of current contributors in the [contributors tab](https://github.com/DevaSenevi/CCIG/graphs/contributors).

## Credits

This project is created using the excellent open source [Jupyter Book project](https://jupyterbook.org/) and the [executablebooks/cookiecutter-jupyter-book template](https://github.com/executablebooks/cookiecutter-jupyter-book).
