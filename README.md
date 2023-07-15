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
```
5. Create a button with the GIF image
```python
button = ttk.Button(root, image=gif_frames[0], style="RoundedButton.TButton", command=create_new_frame)
button.place(x=196, y=196.5)
```
6. Add the GIF frames to the button
```python
def animate_gif(frame_index=0):
    button.config(image=gif_frames[frame_index])
    root.after(100, animate_gif, (frame_index + 1) % len(gif_frames))
animate_gif()
```
7. Save the PhotoImage object in a persistent variable
```python
button.photo = photo
```
8. A function to resize the image to fit the frame
```python
def resize_background(e):
    # Get the size of the frame
    width = e.width
    height = e.height
    # Resize the image to fit the frame
    image = Image.open("background(1).png")
    resized_image = image.resize((width, height), Image.ANTIALIAS)
    photo = ImageTk.PhotoImage(resized_image)
    background_label.config(image=photo)
    background_label.image = photo
frame.bind('<Configure>', resize_background)
```
* ## Create a new frame.
After finishing the main frame let's go to the second one. First, this frame will be hidden until we click the button on the main frame,
after putting the background of the frame we created two buttons, one to go back to the main frame, and the other one to upload an image
```python
def create_new_frame():
    # Hide the current frame
    frame.pack_forget()

    # Create a new frame with the same background image as the first frame
    new_frame = tk.Frame(root, bg="white", background="#000000")
    new_frame.pack(fill="both", expand=True)
    background_label = tk.Label(new_frame, bg="white", background="#000000")
    background_label.place(x=0, y=0, relwidth=1, relheight=1)
    image = Image.open("background.png")
    resized_image = image.resize((root.winfo_width(), root.winfo_height()), Image.ANTIALIAS)
    photo = ImageTk.PhotoImage(resized_image)
    background_label.config(image=photo)
    background_label.image = photo

    # Create a button to go back to the previous frame
    back_button = ttk.Button(new_frame, text="Go back", style="RoundedButton.TButton", 
                            command=lambda: switch_frame(new_frame, frame))
    back_button.place(x=10, y=10)
    # Create a button to upload an image
    upload_button = ttk.Button(new_frame, text="Upload Image", style="RoundedButton.TButton", 
                            command=lambda: upload_image(new_frame))
    upload_button.place(relx=0.5, rely=0.15, anchor="center")
```
* ## Switch frame function.
This frame will switch between the main frame and the second frame.
```python
def switch_frame(current_frame, new_frame):
    # Hide the current frame
    current_frame.pack_forget()
    # Show the new frame
    new_frame.pack(fill="both", expand=True)
```
* ## Upload image funciton
This function is all about the uploaded image, so first we upload the image, open it in a new window, switch to a new frame,
adding the checkbox, the scales for edits, and the go back button.
```python
def upload_image(frame):
    # Open a file dialog to select an image file
    filetypes = (("JPEG files", "*.jpg"), ("JPEG files", "*.jpeg"), ("PNG files", "*.png"), ("All files", "*.*"))
    filename = filedialog.askopenfilename(title="Select an image file", filetypes=filetypes)
    # Save the original image as a PhotoImage object
    original_photo = ImageTk.PhotoImage(Image.open(filename))
    # Save the original image as a PIL Image object
    original_pil_image = Image.open(filename)
    
    # Load the selected image
    if filename:
        image = cv2.imread(filename)
        photo = ImageTk.PhotoImage(Image.fromarray(cv2.cvtColor(image, cv2.COLOR_BGR2RGB)))
        # Switch to the new frame
        new_frame = tk.Frame(root, bg="white", background="#000000")
        new_frame.pack(fill="both", expand=True)
        background_label = tk.Label(new_frame, bg="white", background="#000000")
        background_label.place(x=0, y=0, relwidth=1, relheight=1)
        image = Image.open("background.png")
        resized_image = image.resize((root.winfo_width(), root.winfo_height()), Image.ANTIALIAS)
        photo2 = ImageTk.PhotoImage(resized_image)
        background_label.config(image=photo2)
        background_label.image = photo2
        switch_frame(frame, new_frame)
        # Create a checkbox to convert the image to binary
        binary_var = tk.BooleanVar()
        binary_checkbox = ttk.Checkbutton(new_frame, text="Binary", variable=binary_var, style="RoundedButton.TCheckbutton",
                                           command=lambda: on_binary_checkbox_click(binary_var, image_label))
        binary_checkbox.place(relx=0.5, rely=0.1, anchor="center")
        # Create a scale to rotate the image
        rotate_var = tk.IntVar()
        rotate_scale = tk.Scale(new_frame, from_=0, to=360, orient="horizontal", label="Rotate", variable=rotate_var,
                                length=300, showvalue=0, tickinterval=90, resolution=10,
                                command=lambda value: on_rotate_scale_change(value, image_label, original_pil_image))
        rotate_scale.place(relx=0.5, rely=0.2, anchor="center")
        
        # Create a scale to resize the image
        resize_var = tk.DoubleVar()
        resize_scale = tk.Scale(new_frame, from_=0.5, to=1.5, orient="horizontal", label="Resize", variable=resize_var,
                                length=300, showvalue=0, resolution=0.1,
                                command=lambda value: on_resize_scale_change(value, image_label, original_pil_image))
        resize_scale.place(relx=0.5, rely=0.3, anchor="center")
        
        # Create a scale for blurring the image
        blur_scale = tk.Scale(new_frame, from_=0, to=10, orient="horizontal", length=200,
                              label="Blur Scale", command=lambda x: on_blur_scale_change(int(x), image_label, original_pil_image))
        blur_scale.place(relx=0.5, rely=0.4, anchor="center")
        
        # Create a scale for adjusting the grayscale level of the image
        grayscale_var = tk.IntVar()
        grayscale_scale = tk.Scale(new_frame, from_=0, to=255, orient="horizontal", length=200,
                                   label="Grayscale Level", variable=grayscale_var,
                                   command=lambda value: on_grayscale_scale_change(value, image_label, original_pil_image))
        grayscale_scale.place(relx=0.5, rely=0.5, anchor="center")
        
        # Create a "Go back" button to go back to the upload image frame
        back_button = ttk.Button(new_frame, text="Go back", style="RoundedButton.TButton", 
                                command=lambda: switch_frame(new_frame, frame))
        back_button.place(relx=0.5, rely=0.8, anchor="center")
        image_window = tk.Toplevel(root)
        image_window.title("Uploaded Image")
        # Create a label to display the original image in the new window
        image_label = tk.Label(image_window, image=photo)
        image_label.pack()
        # Save the PhotoImage object in a persistent variable
        image_label.photo = photo
```
* ## Image edit.
Now it's showtime, finally, we are going to apply the image processing filter to the uploaded image, so let's start
1. ### Convert to binary
In this function, we converted the image to a NumPy array and then converted to  a binary image. 
```python
def on_binary_checkbox_click(var, image_label):
    if var.get():
        # Get the current image from the label
        image = image_label.photo
        # Save the current image as the original photo
        image_label.original_photo = image
        # Convert the PhotoImage object to a PIL Image object
        pil_image = ImageTk.getimage(image)
        # Convert the PIL Image object to a NumPy array of the correct type
        image_array = numpy.array(pil_image, dtype=numpy.uint8)
        # Convert the image to binary
        binary = convert_to_binary(cv2.cvtColor(image_array, cv2.COLOR_RGB2BGR))
        # Create a PhotoImage object from the binary image
        binary_photo = ImageTk.PhotoImage(image=Image.fromarray(binary))
        # Update the image label with the binary image
        image_label.config(image=binary_photo)
        image_label.photo = binary_photo
    else:
        # If the checkbox is unchecked, display the original image
        image_label.config(image=image_label.original_photo)
        image_label.photo = image_label.original_photo
```
2. ### Rotate the image.
The function will take the image and rotate it on a scale of 0 to 360 and the user has the freedom to change the scale however he/she 
want it to be.
```python
def on_rotate_scale_change(value, image_label, original_pil_image):
    # Convert the current image to a PIL Image object
    pil_image = ImageTk.getimage(image_label.photo)
    # Rotate the image
    rotated_pil_image = original_pil_image.rotate(int(value))
    # Resize the rotated image to fit in the frame
    width, height = image_label.winfo_width(), image_label.winfo_height()
    resized_image = rotated_pil_image.resize((width, height), Image.ANTIALIAS)
    # Convert the rotated and resized image to a PhotoImage object
    rotated_photo = ImageTk.PhotoImage(resized_image)
    # Update the image label with the rotated and resized PhotoImage object
    image_label.config(image=rotated_photo)
    image_label.photo = rotated_photo
```
3. ### Resize the image
This function will resize the image from a scale of 0.5 of the image to 1.5 of the image.
```python
def on_resize_scale_change(value, image_label, original_pil_image):
    # Convert the current image to a PIL Image object
    pil_image = ImageTk.getimage(image_label.photo)
    # Calculate the new dimensions
    width, height = original_pil_image.size
    new_width, new_height = int(width * float(value)), int(height * float(value))
    # Resize the image
    resized_pil_image = original_pil_image.resize((new_width, new_height), Image.ANTIALIAS)
    # Convert the resized image to a PhotoImage object
    resized_photo = ImageTk.PhotoImage(resized_pil_image)
    # Update the image label with the resized PhotoImage object
    image_label.config(image=resized_photo)
    image_label.photo = resized_photo
```
4. ### Blurring the image
This function will blur the image on a scale of 0 to 10, 0 means it has no blurring, and the maximum blurring we set is 10.
```python
def on_blur_scale_change(scale_value, image_label, original_pil_image):
    # Convert the current image to a NumPy array
    image_array = cv2.cvtColor(numpy.array(original_pil_image), cv2.COLOR_RGB2BGR)

    # Blur the image
    blurred_array = cv2.GaussianBlur(image_array, (scale_value*2+1, scale_value*2+1), 0)

    # Convert the blurred array back to a PIL Image object
    blurred_pil_image = Image.fromarray(cv2.cvtColor(blurred_array, cv2.COLOR_BGR2RGB))

    # Convert the blurred PIL Image object to a PhotoImage object
    blurred_photo = ImageTk.PhotoImage(blurred_pil_image)

    # Update the image label with the blurred PhotoImage object
    image_label.config(image=blurred_photo)
    image_label.photo = blurred_photo
```
5. ### Grayscale
Finally, it is time for the last function which will convert the image from a colored or RGB image to a Grayscale image on a scale of 0 to 255.
```python
def on_grayscale_scale_change(value, image_label, original_pil_image):
    value = int(value)
    # Convert the original image to grayscale
    grayscale_image = original_pil_image.convert("L")
    # Adjust the grayscale level based on the value of the scale
    adjusted_image = ImageEnhance.Brightness(grayscale_image).enhance(value / 255)
    # Update the image label with the adjusted image
    photo = ImageTk.PhotoImage(adjusted_image)
    image_label.config(image=photo)
    image_label.image = photo
```
* ## Final step.
now it's time to put all of this in a loop to keep the frame working.
```python
# Schedule the display_image() function to be called when the mainloop is idle
root.after_idle(display_image)

root.mainloop()
```
* ## Video
Now let's see how it works.
(https://github.com/liva-2/Aziz-Edit/assets/83817004/5f22407d-b8f3-4602-9bfa-9177682550cb)
