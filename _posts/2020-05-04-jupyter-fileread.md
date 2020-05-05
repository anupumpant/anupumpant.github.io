---
title: "Reading CSV in Jupyter Simplified with GUI"
date: 2020-05-04
comments: true
categories:
  - Blog
tags:
  - Python
  - Jupyter
---

```python
import traitlets
import os
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
from tkinter import Tk, filedialog
from ipywidgets import *
from IPython.display import clear_output, display, Javascript
from IPython.core.display import display, HTML

# Function for creating select files dialogue
def select_files(b):
    root = Tk()
    root.withdraw()                                        # Hide the main window
    root.call('wm', 'attributes', '.', '-topmost', True)   # Raise the root to the top of all windows.
    b.files = filedialog.askopenfilename(multiple=True)    # List of selected fileswill be set to b.value
    f = b.files
    b.description = "Files Selected"
    b.icon = "check-square-o"
    b.style.button_color = "lightgreen"
    #clear_output()
    # Adding the dot variable helps you access the variable value outside the function.
    select_files.selectfs = SelectMultiple(options=f, value=[f[0]], disabled=False, layout=Layout(width='100%', height='300px'))
    print("Selected files:")
    display(select_files.selectfs)
    display(H1)

# From Stack exchange. Answer by Anton Golubev
#(https://stackoverflow.com/questions/38783027/jupyter-notebook-display-two-pandas-tables-side-by-side)
def display_side_by_side(dfs:list, captions:list):
    """Display tables side by side to save vertical space
    Input:
        dfs: list of pandas.DataFrame
        captions: list of table captions
    """
    output = ""
    combined = dict(zip(captions, dfs))
    for caption, df in combined.items():
        output += df.style.set_table_attributes("style='display:inline'").set_caption(caption)._repr_html_()
        output += "\xa0\xa0\xa0"
    display(HTML(output))

# Will read all the selected names from the "MultiSelect" box displayed above
def displayheads(c):
    #clear_output()
    f = select_files.selectfs.value
    displayheads.dataheads = []
    displayheads.data = []
    # Feed the data into the list.
    for i in range(0,len(f)):
        displayheads.dataheads.append(pd.read_csv(f[i], sep=None, engine='python').head())
        displayheads.data.append(pd.read_csv(f[i], sep=None, engine='python'))
    display_side_by_side(displayheads.dataheads,f)
    print("===========================================================================================================")

def plotXY(d, xlabel="Wavelength (nm)"):
    f = select_files.selectfs.value
    for i in range(0,len(f)):
        x = pd.read_csv(f[i], sep=None, engine='python').values[:,0]
        y = pd.read_csv(f[i], sep=None, engine='python').values[:,1]
        plt.title(os.path.basename(f[i]))
        plt.xlabel(xlabel)
        plt.plot(x,y)
        plt.show()
    print("===========================================================================================================")

fileselect = Button(description="File Select")
fileselect.icon = "square-o"
fileselect.style.button_color = "orange"
fileselect.on_click(select_files)

dispheads = Button(description="Display heads")
dispheads.on_click(displayheads)

plotb = Button(description="Plot")
plotb.style.button_color = "lightgreen"
plotb.on_click(plotXY)

display(fileselect)
H1 = HBox([dispheads,plotb])

```
![Output-part-1](/assets/images/20200504_O1.JPG)
![Output-part-2](/assets/images/20200504_O2.JPG)
