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


    Button(description='File Select', icon='square-o', style=ButtonStyle(button_color='orange'))


    Selected files:



    SelectMultiple(index=(0,), layout=Layout(height='300px', width='100%'), options=('D:/OneDrive/PhD/Research/Dat…



    HBox(children=(Button(description='Display heads', style=ButtonStyle()), Button(description='Plot', style=Butt…



<style  type="text/css" >
</style><table id="T_00c107de_8e84_11ea_8a02_5800e3d08aba" style='display:inline'><caption>D:/OneDrive/PhD/Research/Data/Raman_PL/191002 powdep Red3-1/RED3_532ex_00081uW_540-900nm_001p0x1ms_002xgain_t001.csv</caption><thead>    <tr>        <th class="blank level0" ></th>        <th class="col_heading level0 col0" >Wavelength</th>        <th class="col_heading level0 col1" >Intensity</th>    </tr></thead><tbody>
                <tr>
                        <th id="T_00c107de_8e84_11ea_8a02_5800e3d08abalevel0_row0" class="row_heading level0 row0" >0</th>
                        <td id="T_00c107de_8e84_11ea_8a02_5800e3d08abarow0_col0" class="data row0 col0" >540.01</td>
                        <td id="T_00c107de_8e84_11ea_8a02_5800e3d08abarow0_col1" class="data row0 col1" >5</td>
            </tr>
            <tr>
                        <th id="T_00c107de_8e84_11ea_8a02_5800e3d08abalevel0_row1" class="row_heading level0 row1" >1</th>
                        <td id="T_00c107de_8e84_11ea_8a02_5800e3d08abarow1_col0" class="data row1 col0" >540.115</td>
                        <td id="T_00c107de_8e84_11ea_8a02_5800e3d08abarow1_col1" class="data row1 col1" >1</td>
            </tr>
            <tr>
                        <th id="T_00c107de_8e84_11ea_8a02_5800e3d08abalevel0_row2" class="row_heading level0 row2" >2</th>
                        <td id="T_00c107de_8e84_11ea_8a02_5800e3d08abarow2_col0" class="data row2 col0" >540.219</td>
                        <td id="T_00c107de_8e84_11ea_8a02_5800e3d08abarow2_col1" class="data row2 col1" >-6</td>
            </tr>
            <tr>
                        <th id="T_00c107de_8e84_11ea_8a02_5800e3d08abalevel0_row3" class="row_heading level0 row3" >3</th>
                        <td id="T_00c107de_8e84_11ea_8a02_5800e3d08abarow3_col0" class="data row3 col0" >540.324</td>
                        <td id="T_00c107de_8e84_11ea_8a02_5800e3d08abarow3_col1" class="data row3 col1" >-17</td>
            </tr>
            <tr>
                        <th id="T_00c107de_8e84_11ea_8a02_5800e3d08abalevel0_row4" class="row_heading level0 row4" >4</th>
                        <td id="T_00c107de_8e84_11ea_8a02_5800e3d08abarow4_col0" class="data row4 col0" >540.428</td>
                        <td id="T_00c107de_8e84_11ea_8a02_5800e3d08abarow4_col1" class="data row4 col1" >-4</td>
            </tr>
    </tbody></table>   <style  type="text/css" >
</style><table id="T_00c12ed4_8e84_11ea_b80d_5800e3d08aba" style='display:inline'><caption>D:/OneDrive/PhD/Research/Data/Raman_PL/191002 powdep Red3-1/RED3_532ex_01033uW_540-900nm_001p0x1ms_002xgain_t001.csv</caption><thead>    <tr>        <th class="blank level0" ></th>        <th class="col_heading level0 col0" >Wavelength</th>        <th class="col_heading level0 col1" >Intensity</th>    </tr></thead><tbody>
                <tr>
                        <th id="T_00c12ed4_8e84_11ea_b80d_5800e3d08abalevel0_row0" class="row_heading level0 row0" >0</th>
                        <td id="T_00c12ed4_8e84_11ea_b80d_5800e3d08abarow0_col0" class="data row0 col0" >540.01</td>
                        <td id="T_00c12ed4_8e84_11ea_b80d_5800e3d08abarow0_col1" class="data row0 col1" >31</td>
            </tr>
            <tr>
                        <th id="T_00c12ed4_8e84_11ea_b80d_5800e3d08abalevel0_row1" class="row_heading level0 row1" >1</th>
                        <td id="T_00c12ed4_8e84_11ea_b80d_5800e3d08abarow1_col0" class="data row1 col0" >540.115</td>
                        <td id="T_00c12ed4_8e84_11ea_b80d_5800e3d08abarow1_col1" class="data row1 col1" >47</td>
            </tr>
            <tr>
                        <th id="T_00c12ed4_8e84_11ea_b80d_5800e3d08abalevel0_row2" class="row_heading level0 row2" >2</th>
                        <td id="T_00c12ed4_8e84_11ea_b80d_5800e3d08abarow2_col0" class="data row2 col0" >540.219</td>
                        <td id="T_00c12ed4_8e84_11ea_b80d_5800e3d08abarow2_col1" class="data row2 col1" >62</td>
            </tr>
            <tr>
                        <th id="T_00c12ed4_8e84_11ea_b80d_5800e3d08abalevel0_row3" class="row_heading level0 row3" >3</th>
                        <td id="T_00c12ed4_8e84_11ea_b80d_5800e3d08abarow3_col0" class="data row3 col0" >540.324</td>
                        <td id="T_00c12ed4_8e84_11ea_b80d_5800e3d08abarow3_col1" class="data row3 col1" >51</td>
            </tr>
            <tr>
                        <th id="T_00c12ed4_8e84_11ea_b80d_5800e3d08abalevel0_row4" class="row_heading level0 row4" >4</th>
                        <td id="T_00c12ed4_8e84_11ea_b80d_5800e3d08abarow4_col0" class="data row4 col0" >540.428</td>
                        <td id="T_00c12ed4_8e84_11ea_b80d_5800e3d08abarow4_col1" class="data row4 col1" >43</td>
            </tr>
    </tbody></table>   


    ===========================================================================================================



![png](/assets/images/output_0_6.png)



![png](/assets/images/output_0_7.png)


    ===========================================================================================================
