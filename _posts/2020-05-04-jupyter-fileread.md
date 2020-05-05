---
title: "Reading CSV in Jupyter Simplified with GUI"
date: 2020-05-04
comments: true
categories:
  - Blog
tags:
  - Jupyter
---
I use Jupyter notebooks to open and analyze spectral data in my lab. I find it inefficient to manually copy paste file locations into a string or form a list of strings for multiple files.

A much more elegant way in my opinion is to somehow use the windows file select dialogue. From a bit of searching, I found that `tkinter` GUI package can be used to produce the file select dialog. This can even be used to do a multi file selection, and quickly produce a list of strings with all the file locations.

It is then as simple as passing the string to `pandas.read_csv` function to read the data out of the CSV or ASCII files.

I like using `pandas` to read files because I've found that it is incredibly fast. It reads a million row file within seconds! And then its versatility is unmatched. For instance, specifying `sep=None, engine='python'` within the `read_csv` function ensures that all sorts of CSVs, tab or space separated ASCII files are read without specifying the specific delimiter. I use it below.

Finally, I want to mention that I have never received formal education for coding. I rely heavily on stack exchange and google to create a patchwork of code that helps me get my work done more efficiently. So forgive me for not paying heed to best practices that may be second nature to computer scientists or IT engineers.

### What does this code do?

The full code block can be copy pasted into one of the cells of a Jupyter notebook. Upon running the cell, it produces a file select button which is initially orange in color. Clicking this button starts the `select_files` function which opens up the usual system's file select dialogue.

 The file select button, after it has done its job is set to `clear_output()` and erase itself. This is done within the `select_files` function. If the `clear_output()` is commented out, the button simply shows a tick mark and turns green to indicate that the files have already been selected.

The idea is to navigate to your data folder and select the CSV or ASCII data files.

The location strings of selected files are stored in the `select_files.selectfs` list. And the list is displayed in an `ipywidget` `SelectMultiple` widget. Two buttons, `Display heads` and `Plot` are displayed in an `Hbox` below this box. I use the Hbox in order to stack the buttons one beside the another to save the vertical space.

In the next step, the user holds either `ctrl` or `shift` to select multiple files. Clicking the `Display heads` button then uses the `display_side_by_side` function to display selected files' data heads (see pandas.read_csv documentation) one beside the another (again, to save the vertical real estate).

Clicking the plot button plots the first and second column as x and y. I convert the pandas data frame to a simple `numpy` array by using the `.values` (see inside the `plotXY` function). Plot's xlabels and ylabels etc can be modified within the function to plot the way you want it for your files. I threw in the plotting just as a demonstration. Meanwhile I'll work on a more elegant plotting method.

Also, note that inside the `plotXY` function I use the `os.path.basename(filelocation-string)` to convert the full file path to filename for the title of the plot.

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
    clear_output()
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

def plotXY(d):
    f = select_files.selectfs.value
    for i in range(0,len(f)):
        x = pd.read_csv(f[i], sep=None, engine='python').values[:,0]
        y = pd.read_csv(f[i], sep=None, engine='python').values[:,1]
        plt.title(os.path.basename(f[i]))
        plt.xlabel("Wavelength (nm)")
        plt.ylabel("Intensity (arb. units)")
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
