# Jupyter Notebook

A Jupyter Notebook is first of all a file, like a normal file on your computer, a .pdf or a .docx file: its extension is .ipynb (if you don't see file extensions in your computer, this is the right moment to fix it! Just search the web for "how to make extension visible" in your operating system –Windows, MacOS or Linux– and you will find plenty of very easy instructions).

A Jupyter Notebook is a special sort of file, because it can contain text (to be read) and code (to be run, or executed): the text is written in the Markdown markup language and the code in the Python programming language.

In the Run and Kernel drop-down menus in the Jupyter toolbar, you find options to run all the cells or to clear all the outputs of the executable cells.

To open a Jupyter Notebook you will need the right environment, just as to open a .docx file you need Microsoft Word or another program capable of doing it. There are several options listed here below.

## Platforms to run notebooks (no local installation required)

### mybinder

1. Use the button `launch binder` on this repo homepage. If this does not work, go to https://mybinder.org, copy paste the link to this Github repo (not the single notebook, but the full repo: https://github.com/DHBern/text-analysis-course) and click launch.
2. mybinder reads the repo, install all the dependencies listed in the file `requirements.txt` and launch the Jupyter Notebook application in a virtual machine.
3. Browse and select files from the left panel. You are ready to go!

### Noto
1. Go to https://noto.epfl.ch and login with Switch.
2. From the launcher, open the Terminal.
3. Clone the repository by typing the the following command: `git clone https://github.com/DHBern/text-analysis-course.git`
4. Move inside the cloned repo: `cd text-analysis-course/`
5. Create a virtual environment: `python3 -m venv .venv`
6. Activate the virtual environment: `source .venv/bin/activate`
7. Install dependencies: `pip install -r requirements.txt`
8. Register Jupyter kernel, in order for JupyterLab to recognize the environment: `pip install ipykernel`  and `python -m ipykernel install --user --name=.venv --display-name="Python (Repo Env)"`
9. Apply the kernel to your notebook: open your `.ipynb` file, click the kernel name in the top-right corner, and select 'Python (Repo Env)'.

### Download
**When you are working in mybinder or Noto, nothing is saved to your computer. Remember to download the notebook and other files, if you want to save them!**


## Local installation

Requirement: Python

Install: https://jupyter.org/install

Follow steps 2 to 7 from the Noto instructions here above.