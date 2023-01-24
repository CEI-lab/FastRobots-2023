# Jupyter Lab

## What is JupyterLab?
JupyterLab is a web-based interactive development environment for Jupyter notebooks, code, and data. <br>
You can configure and arrange the user interface to support a wide range of workflows in data science, scientific computing, and machine learning. <br>

## What is a Jupyter notebook?

A Jupyter Notebook is an interactive environment for writing and running code. A notebook is capable of running code in a wide range of languages and may include: 
- Live code 
- Interactive widgets 
- Plots 
- Narrative text
- Equations
- Media: Images/Videos

These documents provide a complete and self-contained record of a computation that can be converted to various formats and shared with others using email, Dropbox, version control systems (like git/GitHub) or nbviewer.jupyter.org.


Here is a [quick visual walkthrough](https://www.youtube.com/watch?v=A5YyoCKxEOU&feature=emb_logo) of the JupyterLab web application interface. <br>

## Instructions:

### Start the Jupyter server
Before you can open, run or modify a Jupyter notebook, you need to start the Jupyter server.
1. Open a new terminal and run the code below:
   > The Jupyter server can only access notebooks from the directory (and its child directories) it was started in.
   ```bash
      jupyter lab 
   ```
   > macOS users may have to use `Jupyter lab` (capital J).
2. The Jupyter lab web application should automatically open as a tab in your default web browser. If not, copy and paste the link from the terminal in your web browser.

### Open a Notebook
We will now open a Jupyter notebook that will guide us through some of its basic functionalities.
1. Download the Jupyter notebook: [Running_code.ipynb](./Running_code.ipynb)
   > Github allows you to view a notebook in the browser. However, you cannot run/modify it.
2. In your Jupyter lab web application, use the file browser (first icon on the left pane) to navigate to the notebook location.
3. Double click on the notebook to open it.
4. Follow the instructions in the notebook.

### Stop the Jupyter server
**Once you have saved your Jupyter notebook**, you may stop the Jupyter server by pressing <CTRL+C> on your keyboard, in the terminal that is running the server.


## References:
1. [Jupyter Lab Overview](https://jupyterlab.readthedocs.io/en/stable/getting_started/overview.html)