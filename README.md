import os
import json
import tkinter as tk
from tkinter import messagebox
def load_contacts():
    if os.path.exists("contacts.json"):
        with open("contacts.json", "r", encoding="utf-8") as file:
            return json.load(file)
    return []

def save_contacts():
    with open("contacts.json", "w", encoding="utf-8") as file:
        json.dump(contacts, file, ensure_ascii=False, indent=4)

contacts = load_contacts()

def linear_search(data, target):
    result=[]
    for contact in data:
        if "name" in contact:
            if (target.lower() in contact["name"].lower() or
                target.lower() in contact["surname"].lower() or
                target in contact["number"]):
                result.append(contact)
    return result

def contact_add():
    name=nameinfo.get().strip()
    surname=surnameinfo.get().strip()
    noumber=numberinfo.get().strip()
    if name=='' or surname=='' or number=='':
        messagebox.showwarning('error','name and surname cannot be empty')
        return
    contacts.append({"name":name,"surname":surname,"number":number})
    save_contacts()
    show_all()
    clear_entries()
    
def contact_search():
    target=axtarinfo.get()
    result=linear_search(contacts, target)
    box.delete(0, tk.END)
    if not result:
        box.insert(tk.END, 'nothing has found')
    else:
        for c in result:
            box.insert(tk.END, f'{c["name"]} {c["surname"]} - {c["number"]}')
            
def show_all():
    box.delete(0, tk.END)
    for c in contacts:
        box.insert(tk.END, f'{c["name"]} {c["surname"]} - {c["number"]}')


def clear_entries():
    adinfo.delete(0, tk.END)
    soyadinfo.delete(0, tk.END)
    nomreinfo.delete(0, tk.END)

gui = tk.Tk()
gui.geometry("450x550")
gui.title("contact list and search system")

tk.Label(gui, text='contacts',font=("Arial", 16, "bold")).pack(pady=4)

tk.Label(gui, text="name").pack(pady=10)
nameinfo = tk.Entry(gui, width= 45)
nameinfo.pack(pady=1)

tk.Label(gui, text="surname").pack(pady=10)
surnameinfo = tk.Entry(gui, width= 45)
surnameinfo.pack(pady=1)

tk.Label(gui, text="number").pack(pady=10)
numberinfo = tk.Entry(gui, width= 45)
numberinfo.pack(pady=1)

tk.Button(gui, text="add", command=contact_add).pack(pady=10)

tk.Label(gui, text="search for name").pack(pady=10)
searchinfo = tk.Entry(gui, width= 45)
searchinfo.pack(pady=1)

tk.Button(gui, text="search", command=contact_search).pack(pady=10)

tk.Button(gui, text="show all", command=show_all).pack(pady=10)


box = tk.Listbox(gui, width=60, height=10)
box.pack(pady=10)
gui.mainloop()    
