# Aziz-Edit
A Python app where you can upload your image and do some changes to it using TKinter

* ## Imports.
First I used some libraries to create a GUI app and do some image processing.
```python
import tkinter as tk
import tkinter.ttk as ttk
from PIL import ImageTk, Image, ImageEnhance
from tkinter import filedialog
import cv2
import numpy
```
* ## Creating an object an set some values.
After importing the libraries we will use, we need to create an object for TKinter and if you want, you can set the title, size,
and icon of the frame.
```python
root = tk.Tk()
root.title("aziz edit")
root.iconbitmap("evc.ico")
root.geometry("1000x500+250+150")
```
* ## Create the first frame.
Now it is time to create the first frame which is the main frame that will appear when we run the code.
```python
frame = tk.Frame(root, bg="white", background="#f72585")
frame.pack(fill="both", expand=True)
```
* ## Create a style for the button.
I used a button with no sharp edges and a GIF, so I set a style for that button.
```python
style = ttk.Style()
style.configure("RoundedButton.TButton", relief="flat", borderwidth=0, background="#000000", 
                foreground="#000000", font=("Arial", 12), bordercolor="#000000", 
                highlightbackground="#000000", highlightcolor="#000000", highlightthickness=0, 
                bd=0, padx=20, pady=10)
style.map("RoundedButton.TButton", background=[("active", "#000000")])
```
* ## First function.
Now it is time to edit the frame, let's do it step by step:
1. load the image
```python
image = Image.open("background.png")
photo = ImageTk.PhotoImage(image)
```
2. Set the image as the background of the frame
```python
background_label = tk.Label(frame, image=photo)
background_label.place(x=0, y=0, relwidth=1, relheight=1)
```
3. Add a widget or a label to the frame
```python
label = tk.Label(frame, text="Image Edit", bg='#000000', fg='#f72585', font=("Bauhaus 93", 50))
label.place(relx=0.646, rely=0.06, anchor="center")
```
4. Load the GIF image for the button
```python
gif_image = Image.open("giphy2.gif")
gif_frames = []
try:
  while True:
    gif_frame = gif_image.copy()
    gif_frames.append(ImageTk.PhotoImage(gif_frame))
    gif_image.seek(len(gif_frames))
except EOFError:
  pass
