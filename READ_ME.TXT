# AI PROMPT ENGINEERING 

CHAT GPT INTERACTION LOG

------------


Default (GPT-3.5)

User
CREATE A PYTHON APP:  
► THE APP SHOULD HAVE A GUI WITH 2 ARROW BUTTONS
► THERE SHOULD BE A BUTTON TO CHOOSE A FOLDER
► THE GUI SHOULD SHOW A SELECTED PICTURE FROM THE FOLDER
► THE 2 ARROW BUTTONS CHANGES BETWEEN PICTURES IN THE FOLDER LIBRARY
ChatGPT
Creating a full-fledged GUI application involves multiple steps and libraries. In this example, I'll guide you through creating a simple Python app using the tkinter library for the GUI and the Pillow library for image handling. You'll need to install the Pillow library if you haven't already:

bash
Copy code
pip install Pillow
Here's a basic implementation of the app you described:

python
Copy code
import os
import tkinter as tk
from tkinter import filedialog
from PIL import Image, ImageTk

class ImageViewerApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Image Viewer App")

        self.image_label = tk.Label(root)
        self.image_label.pack()

        self.prev_button = tk.Button(root, text="Previous", command=self.show_previous_image)
        self.prev_button.pack(side=tk.LEFT, padx=10)

        self.next_button = tk.Button(root, text="Next", command=self.show_next_image)
        self.next_button.pack(side=tk.RIGHT, padx=10)

        self.folder_button = tk.Button(root, text="Choose Folder", command=self.choose_folder)
        self.folder_button.pack(pady=10)

        self.image_paths = []
        self.current_image_index = 0

    def choose_folder(self):
        folder_path = filedialog.askdirectory()
        if folder_path:
            self.image_paths = [os.path.join(folder_path, file) for file in os.listdir(folder_path) if file.lower().endswith(('.png', '.jpg', '.jpeg', '.gif', '.bmp'))]
            if self.image_paths:
                self.current_image_index = 0
                self.show_image()

    def show_image(self):
        if self.image_paths:
            image_path = self.image_paths[self.current_image_index]
            image = Image.open(image_path)
            image = image.resize((400, 400), Image.ANTIALIAS)
            self.photo = ImageTk.PhotoImage(image=image)
            self.image_label.config(image=self.photo)

    def show_previous_image(self):
        if self.image_paths:
            self.current_image_index = (self.current_image_index - 1) % len(self.image_paths)
            self.show_image()

    def show_next_image(self):
        if self.image_paths:
            self.current_image_index = (self.current_image_index + 1) % len(self.image_paths)
            self.show_image()

if __name__ == "__main__":
    root = tk.Tk()
    app = ImageViewerApp(root)
    root.mainloop()
Save this code to a .py file and run it. It will create a simple image viewer app with the features you described. The app will allow you to choose a folder containing images, display the selected image, and navigate through the images using the arrow buttons.
