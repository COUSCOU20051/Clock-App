import tkinter as tk
from tkinter import colorchooser
from datetime import datetime

class FlipClock:
    def __init__(self, master):
        self.master = master
        master.title("Flip Clock")
        master.geometry("600x300")
        master.configure(bg="black")

        self.show_millis = False
        self.panel_width = 220
        self.panel_visible = False
        self.panel_x = -self.panel_width

        # menu button -> ☰ 
        self.settings_button = tk.Button(master, text="☰", font=("Anton", 24, "bold"),
                                         command=self.toggle_settings, bd=0, fg="white", bg="black",
                                         activebackground="gray20", activeforeground="white")
        self.settings_button.place(x=10, y=10)

        # Date label 
        self.date_label = tk.Label(master, font=("Anton", 28, "bold"), fg="lightgray", bg="black")
        self.date_label.pack(pady=(20, 0))

        # Time label
        self.time_label = tk.Label(master, font=("Anton", 96, "bold"), fg="white", bg="black")
        self.time_label.pack(expand=True, pady=(10, 0))

        # Panel animation
        self.settings_frame = tk.Frame(master, width=self.panel_width, height=300, bg="gray15")
        self.settings_frame.place(x=self.panel_x, y=0)

        # buttons
        self.time_color_btn = self.create_modern_button(self.settings_frame, "Time Color", self.change_time_color)
        self.date_color_btn = self.create_modern_button(self.settings_frame, "Date Color", self.change_date_color)
        self.millis_btn = self.create_modern_button(self.settings_frame, "Show Milliseconds", self.toggle_millis)

        master.bind("<Button-1>", self.close_panel_on_click)

        self.update_clock()

    def create_modern_button(self, parent, text, command):
        btn = tk.Button(parent, text=text, command=command,
                        font=("Anton", 14), fg="white", bg="gray25",
                        activebackground="gray40", relief="flat", bd=0, padx=10, pady=8)
        btn.pack(pady=10, fill="x", padx=10)
        return btn

    def toggle_settings(self):
        self.panel_visible = not self.panel_visible
        self.animate_panel()

    def animate_panel(self):
        target_x = 0 if self.panel_visible else -self.panel_width
        step = 20 if self.panel_visible else -20

        def slide():
            nonlocal step
            if (step > 0 and self.panel_x < target_x) or (step < 0 and self.panel_x > target_x):
                self.panel_x += step
                if step > 0 and self.panel_x > target_x: self.panel_x = target_x
                if step < 0 and self.panel_x < target_x: self.panel_x = target_x
                self.settings_frame.place(x=self.panel_x, y=0)
                self.master.after(10, slide)
            else:
                self.panel_x = target_x
                self.settings_frame.place(x=self.panel_x, y=0)
        slide()

    def close_panel_on_click(self, event):
        # Close only if the panel is open and click only works if it is outside of it
        if self.panel_visible:
            x_click = event.x
            if x_click > self.panel_width:  
                self.panel_visible = False
                self.animate_panel()

    def change_time_color(self):
        color = colorchooser.askcolor()[1]
        if color:
            self.time_label.config(fg=color)

    def change_date_color(self):
        color = colorchooser.askcolor()[1]
        if color:
            self.date_label.config(fg=color)

    def toggle_millis(self):
        self.show_millis = not self.show_millis

    def update_clock(self):
        now = datetime.now()
        if self.show_millis:
            time_str = now.strftime("%H:%M:%S.%f")[:-3]
        else:
            time_str = now.strftime("%H:%M:%S")

        self.time_label.config(text=time_str)
        self.date_label.config(text=now.strftime("%d %B %Y"))
        self.master.after(50, self.update_clock)

if __name__ == "__main__":
    root = tk.Tk()
    app = FlipClock(root)
    root.mainloop()
