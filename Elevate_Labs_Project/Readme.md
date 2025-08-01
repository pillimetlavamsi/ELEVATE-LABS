Here, i am updating and installing python3-env
<p align="center">
   <img src="Images/Image1.jpg" width=900 />
</p>

Step 1: Create Project Folder
```bash
mkdir SteganographyTool
cd SteganographyTool
```

<p align="center">
   <img src="Images/Image3.jpg" width=900 />
</p>

Step 2: Set Up a Virtual Environment

```bash
python3 -m venv venv
source venv/bin/activate
```

<p align="center">
   <img src="Images/Image2.jpg" width=900 />
</p>

Step 3: Install Required Python Libraries

```bash
pip install pillow
```

Step 4: Create the GUI Python Script
```bash
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image
import os

def text_to_binary(message):
    return ''.join(format(ord(c), '08b') for c in message)

def binary_to_text(binary):
    chars = [binary[i:i+8] for i in range(0, len(binary), 8)]
    return ''.join(chr(int(char, 2)) for char in chars)


def hide_data(image_path, message, output_path="output.png"):
    img = Image.open(image_path)
    binary_data = text_to_binary(message) + '1111111111111110'  
    data_index = 0
    pixels = list(img.getdata())
    new_pixels = []

    for pixel in pixels:
        r, g, b = pixel[:3]
        new_pixel = []
        for color in (r, g, b):
            if data_index < len(binary_data):
                new_color = (color & ~1) | int(binary_data[data_index])
                data_index += 1
            else:
                new_color = color
            new_pixel.append(new_color)
        new_pixels.append(tuple(new_pixel))

    new_img = Image.new(img.mode, img.size)
    new_img.putdata(new_pixels)
    new_img.save(output_path)
    return output_path

def extract_data(image_path):
    img = Image.open(image_path)
    binary_data = ""
    for pixel in img.getdata():
        for color in pixel[:3]:
            binary_data += str(color & 1)

    end_marker = '1111111111111110'
    end_index = binary_data.find(end_marker)
    if end_index != -1:
        binary_data = binary_data[:end_index]
    else:
        return "No hidden message found."

    return binary_to_text(binary_data)


class SteganographyApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Steganography Tool")
        self.root.geometry("500x400")
        self.image_path = ""
        self.create_widgets()

    def create_widgets(self):
        self.select_button = tk.Button(self.root, text="Select Image", command=self.select_image)
        self.select_button.pack(pady=10)

        self.image_label = tk.Label(self.root, text="No image selected")
        self.image_label.pack()

        self.message_label = tk.Label(self.root, text="Enter Message:")
        self.message_label.pack()

        self.message_entry = tk.Text(self.root, height=5, width=40)
        self.message_entry.pack()

        self.hide_button = tk.Button(self.root, text="Hide Message", command=self.hide_message)
        self.hide_button.pack(pady=10)

        self.extract_button = tk.Button(self.root, text="Extract Message", command=self.extract_message)
        self.extract_button.pack(pady=10)

    def select_image(self):
        self.image_path = filedialog.askopenfilename(filetypes=[("Image Files", "*.png *.bmp")])
        self.image_label.config(text=os.path.basename(self.image_path))

    def hide_message(self):
        if not self.image_path:
            messagebox.showerror("Error", "No image selected")
            return

        message = self.message_entry.get("1.0", tk.END).strip()
        if not message:
            messagebox.showerror("Error", "Message is empty")
            return

        output_path = filedialog.asksaveasfilename(defaultextension=".png")
        if output_path:
            try:
                hide_data(self.image_path, message, output_path)
                messagebox.showinfo("Success", f"Message hidden in {output_path}")
            except Exception as e:
                messagebox.showerror("Error", str(e))

    def extract_message(self):
        if not self.image_path:
            messagebox.showerror("Error", "No image selected")
            return

        try:
            message = extract_data(self.image_path)
            messagebox.showinfo("Extracted Message", message)
        except Exception as e:
            messagebox.showerror("Error", str(e))


if __name__ == "__main__":
    root = tk.Tk()
    app = SteganographyApp(root)
    root.mainloop()
```

Step 5: Run the Application

```bash
python steganography_gui.py
```
<p align="center">
   <img src="Images/image4.jpg" width=900 />
</p>

Step 6: Using the Tool
Click "Select Image" to choose a .png or .bmp image.

Type a message in the textbox.

Click "Hide Message" → it will prompt you to overwrite or save as a new file.

Yes: It will replace the image.

No: You can give a new file name.

To check if the message is hidden, click "Extract Message" and it will display the hidden message from the image.

<p align="center">
   <img src="Images/Image5.jpg" width=900 />
</p>

This is the image i used to hide my text.

<p align="center">
   <img src="Images/Text_hiding_image.jpg" width=900 />
</p>

This is the image i am selecting where i need to hide my data.

<p align="center">
   <img src="Images/Image6.jpg" width=900 />
</p>

Here, I am writing the text which i need to hide.

<p align="center">
   <img src="Images/Image7.jpg" width=900 />
</p>

Clicking on option to hide and giving name of the file.

<p align="center">
   <img src="Images/Image8.jpg" width=900 />
</p>

Here, clicked on option hide message.

<p align="center">
   <img src="Images/Image9.jpg" width=900 />
</p>
To check whether message is hiiden or not.

<p align="center">
   <img src="Images/Image10.jpg" width=900 />
</p>
Click on extract message.
Here, we can see that message is extracted exatly what i entered previously.
<p align="center">
   <img src="Images/Image11.jpg" width=900 />
</p>









