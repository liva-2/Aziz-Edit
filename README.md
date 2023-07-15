# Aziz-Edit
A Python app where you can upload your image and do some changes to it using TKinter

* ## Imports:
First I used some libraries to create a GUI app and do some image processing.
```python
import tkinter as tk
import tkinter.ttk as ttk
from PIL import ImageTk, Image, ImageEnhance
from tkinter import filedialog
import cv2
import numpy
```
* ## Creating an object an set some values
After importing the libraries that we are gonna use we need to create an object for TKinter and if you want you can set the title, and the size
and the icon of the frame.
```python
root = tk.Tk()
root.title("aziz edit")
root.iconbitmap("evc.ico")
root.geometry("1000x500+250+150")
```
