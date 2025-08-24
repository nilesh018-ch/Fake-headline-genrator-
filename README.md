# Fake-headline-genrator-
"""
📢 FAKE NEWS HEADLINE GENERATOR
🛠️ Built using Python + Tkinter + pyttsx3
📸 Perfect for content creators, bloggers & YouTubers
"""

import tkinter as tk
from tkinter import ttk
import random
import datetime
import pyttsx3

# 🎙️ Initialize Text-to-Speech engine
engine = pyttsx3.init()
engine.setProperty('rate', 150)

def speak_text(text):
    engine.say(text)
    engine.runAndWait()

# 📚 Categories (Funny removed)
categories = {
    "Politics": {
        "description": "News about leaders, policies, and Parliament.",
        "subjects": ["Prime Minister Modi", "Nirmala Sitharaman", "Rahul Gandhi", "the Parliament Speaker"],
        "actions": ["passes", "rejects", "debates", "declares", "launches"],
        "places_or_things": ["a new bill", "emergency in Parliament", "war on inflation", "a surprise tax cut"]
    },
    "Sports": {
        "description": "Updates from cricket, tournaments, and athletes.",
        "subjects": ["Virat Kohli", "MS Dhoni", "Rohit Sharma", "Team India"],
        "actions": ["smashes", "cancels", "wins", "loses", "announces"],
        "places_or_things": ["a new stadium", "the IPL trophy", "a cricket academy", "tennis match challenge"]
    },
    "Entertainment": {
        "description": "Bollywood buzz and celebrity scoops.",
        "subjects": ["Shah Rukh Khan", "Deepika Padukone", "a Bollywood director", "Karan Johar"],
        "actions": ["dances with", "launches", "stars in", "leaks"],
        "places_or_things": ["a sci-fi film", "a remake of DDLJ", "secret Netflix project", "new music video"]
    },
    "Animals": {
        "description": "Hilarious and wild animal happenings.",
        "subjects": ["a Mumbai cat", "a group of monkeys", "a street dog", "a pigeon in Delhi"],
        "actions": ["attacks", "joins", "saves", "eats"],
        "places_or_things": ["a samosa vendor", "a wedding procession", "a metro ride", "a yoga class"]
    },
    "Gaming": {
        "description": "Gaming news, memes, and virtual drama.",
        "subjects": ["a noob gamer", "a sweaty pro", "an AI NPC", "a Fortnite llama"],
        "actions": ["rage quits", "teabags", "hacks", "accidentally deletes"],
        "places_or_things": ["the final boss", "Minecraft world", "GTA car chase", "an eSports championship"]
    }
}

# 🔮 Generate headline
def generate_headline():
    category = category_var.get()
    if category not in categories:
        result_label.config(text="⚠️ Please select a valid category.")
        return

    subject = random.choice(categories[category]["subjects"])
    action = random.choice(categories[category]["actions"])
    thing = random.choice(categories[category]["places_or_things"])
    headline = f"🚨 BREAKING: {subject} {action} {thing} 😱 #FakeNews"

    result_label.config(text=headline)
    speak_text(headline)

    # ✅ Save with UTF-8 to handle emojis
    timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    with open("fake_news_log.txt", "a", encoding="utf-8") as file:
        file.write(f"{timestamp} - {headline}\n")

    print(f"[📰 Logged] {headline}")

# 📋 Copy headline
def copy_to_clipboard():
    root.clipboard_clear()
    root.clipboard_append(result_label.cget("text"))
    root.update()

# 🧹 Clear headline
def clear_headline():
    result_label.config(text="")

# ℹ️ Show tooltip description
def show_description(event):
    category = category_var.get()
    desc = categories.get(category, {}).get("description", "")
    tooltip_label.config(text=desc)

# 🎨 GUI Setup
root = tk.Tk()
root.title("Fake News Generator for Content Creators")
root.geometry("700x420")
root.configure(bg="white")

category_var = tk.StringVar(value="Entertainment")

# 🧾 UI Layout
tk.Label(root, text="Choose News Category:", bg="white", font=("Arial", 13)).pack(pady=10)

category_dropdown = ttk.Combobox(
    root, textvariable=category_var, values=list(categories.keys()), 
    font=("Arial", 12), state="readonly", width=30
)
category_dropdown.pack()
category_dropdown.bind("<<ComboboxSelected>>", show_description)

tooltip_label = tk.Label(root, text="", font=("Arial", 10), fg="gray", bg="white")
tooltip_label.pack(pady=4)

tk.Button(
    root, text="🎲 Generate Fake Headline", font=("Arial", 14), 
    bg="#FF5733", fg="white", command=generate_headline
).pack(pady=10)

result_label = tk.Label(
    root, text="", wraplength=600, bg="white", 
    font=("Arial", 14), fg="#2c3e50", justify="center"
)
result_label.pack(pady=10)

# 🧰 Buttons
btn_frame = tk.Frame(root, bg="white")
btn_frame.pack(pady=10)

tk.Button(btn_frame, text="📋 Copy", font=("Arial", 12), bg="green", fg="white", command=copy_to_clipboard).grid(row=0, column=0, padx=5)
tk.Button(btn_frame, text="🧹 Clear", font=("Arial", 12), bg="orange", fg="white", command=clear_headline).grid(row=0, column=1, padx=5)
tk.Button(btn_frame, text="🔊 Speak Again", font=("Arial", 12), bg="blue", fg="white", command=lambda: speak_text(result_label.cget("text"))).grid(row=0, column=2, padx=5)

tk.Button(root, text="🚪 Exit", font=("Arial", 12), bg="gray", fg="white", command=root.quit).pack(pady=10)

# 🚀 Launch
print("✅ Fake News Headline Generator is running. Hello YouTubers! 🎥📰")
root.mainloop()


