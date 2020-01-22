# Xcalc - Simple Calculator with Python GUI (TK)
Simple Calculator with Python GUI (TK) is [my vanity Python project](http://www.eobasi.com/first-python-gui-progam-xcalc). It should enable you do basic calculation such as addition, subtraction, multiplication and division, but should it fail, just know it's my first Python GUI program.

See my motivation here >> http://www.eobasi.com/first-python-gui-progam-xcalc

## Xcalc Source Code
This code documentation is intended to help you understand, use and modify Xcode if you are new to Python programming. The program is written in good will, in the spirit of open source. If you happen to find this program or the source code useful, feel free to use as you see fit with or without attribution.

Xcalc is written with Python3. With a few adjustment to the code, you can have it working on Python 2 as well.

```python
#!/usr/bin/python3
import tkinter
import tkinter.messagebox

WIN_WIDTH = 360
WIN_HEIGHT = 420

NUM_COLUMN_COUNT = 3
OPERATOR_COLUMN_COUNT = 3

BTN_WIDTH = 4
BTN_HEIGTH = 3
BTN_FONT = ("Arial", 14)

DISPLAY_HEIGHT = 3
DISPLAY_WIDTH = 20
DISPLAY_FONT = ("Arial", 18)
DISPLAY_SM_FONT = ("Arial", 14)

DISPLAY_CHAR_LIMIT = 20

numpad = [1,2,3,4,5,6,7,8,9,0, '.']
operators = ['+','-', '/', '*', '=']

box = tkinter.Tk()

def winDocument( params = {}):
    winParams = {
        "title": "Document::Simple Calculator",
        "content": "",
        "minwidth": 400,
        "minheight": 400,
    }
    winParams.update(params)
    winDocument = tkinter.Toplevel(box)
    winDocument.title(winParams["title"])
    winDocument.minsize(width=winParams["minwidth"], height=winParams["minheight"])

    content = tkinter.Message(winDocument, text=winParams["content"], background="#2f4f68", foreground="#fff", font=("Arial", 14))
    content.pack(expand=1,fill=tkinter.BOTH)

def menubar():
    menubar = tkinter.Menu(box)

    helpmenu = tkinter.Menu(menubar, tearoff=0)
    helpmenu.add_command(label="About...", command=lambda: winDocument(params={
        "title": "About...",
        "content": "Simple Calculator (xcalc) is a vanity python project by Ebenezer Obasi (www.eobasi.com). It should enable you do basic calculation such as addition, substraction, multiplication and division, but should it fail, just know it's my first Python GUI program.\
        \n\nTips:\n - Use ** operator to get the power of a number.\n- Use // operator to get the floor of numbers divided.\
        \n\nFeel free to contact me anytime at info@eobasi.com"
    }), background="#2f4f68", foreground="#fff", font=("Arial", 14))
    menubar.add_cascade(label="Help", menu=helpmenu)

    return menubar

def btnCallBack(x):
    if str(x) == '=':
        try:
            calc = eval(equation.get())
        except:
            equation.set('error')
        else:
            equation.set(str(calc))
        return
    elif str(x) == 'clear':
        equation.set("")
        return
    elif str(x) == 'c':
        e = str(equation.get())
        e = e[:-1]
        equation.set(e)
    else:
        equation.set(equation.get()+str(x))

def generateBtn(master, label, grid = {}, params = {}):
    btnParams = {
        "text": label,
        "bg": "#2f4f68",
        "bd": 0,
        "font": BTN_FONT,
        "fg": "#fff",
        "relief":"sunken",
        "activebackground": "#324e64",
        "activeforeground": "#fff",
        "width": BTN_WIDTH,
        "height": BTN_HEIGTH,
        "command": lambda: btnCallBack(label)
    }
    btnParams.update(params)
    
    btn = tkinter.Button(master, btnParams)
    btn.grid(grid)

box.configure(background = "#3c4248")
box.title("Simple Calculator")
box.geometry("{}x{}".format(WIN_WIDTH, WIN_HEIGHT))
box.minsize(width=WIN_WIDTH, height=WIN_HEIGHT)
box.maxsize(width=WIN_WIDTH, height=WIN_HEIGHT)

dispalyArea = tkinter.Frame(box, bg="#333")
dispalyArea.grid(columnspan = 2, row = 0, column = 0)

numArea = tkinter.Frame(box, bg="#201c29")
numArea.grid( row = 1, column = 0, ipady="20")

operatorArea = tkinter.Frame(box, bg="#201c29")
operatorArea.grid( row = 1, column = 1, ipady="20")

equation = tkinter.StringVar()

display = tkinter.Entry(dispalyArea, {
    "textvariable": equation,
    'bg': "#333",
    "fg": "#fff",
    'bd': 0,
    #'wraplength':20,
    #'anchor': "w",
    "font":DISPLAY_FONT,
    "width":DISPLAY_WIDTH,
})
display.grid(columnspan=3,row = 0, ipadx=10, column=0, padx="5")

column = row = count = 0

for x in range(0, len(numpad)):
    params = {}
    grid = {"column":column, "row":row}

    if x == (len(numpad) - 1) and column < (NUM_COLUMN_COUNT - 1):
        grid.update({"columnspan":(NUM_COLUMN_COUNT - 1)})
        params.update({"width":BTN_WIDTH*3})

    generateBtn(numArea, label=numpad[x], grid=grid, params=params)

    if count == (NUM_COLUMN_COUNT - 1):
        row += 1
        column = 0
        count = 0
    else:
        count += 1
        column += 1

generateBtn(operatorArea, label='c', grid={ "column":0, "row":0, "columnspan":2}, params={"width":BTN_WIDTH*3})

row = 1; column = count = 0
for x in range(0, len(operators)):
    params = {}
    grid = {"column":column, "row":row}

    if x == (len(operators) - 1) and len(operators) % 2 == 1:
        if column < 1:
            grid.update({"columnspan":2})
            params.update({"width":BTN_WIDTH*3})

    generateBtn(operatorArea, label=operators[x], grid=grid, params=params)

    if count == 1:
        row += 1
        column = 0
        count = 0
    else:
        count += 1
        column += 1

generateBtn(dispalyArea, label='clear', grid={ "column":3, "row":0, "columnspan":1})

box.config(menu=menubar())
box.mainloop()
```
The [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)) line `#!/usr/bin/python3` at the begginig of the code defines where the interpreter is located. In this case, the python3 interpreter is located in `/usr/bin/python3`.

```python
#!/usr/bin/python3
...
...
```

A shebang line could also be a bash, ruby, perl or any other scripting languages' interpreter, for example: `#!/bin/bash`. Without the shebang line, the operating system does not know it's a python script, even if you set the execution flag on the script and run it like `./script.py`. To make the script run by default in python3, either invoke it as `python3 script.py` or set the shebang line.

You can use `#!/usr/bin/env python3` for portability across different systems in case they have the language interpreter installed in different locations.

### Python Tkinter GUI Toolkit

`sudo apt install python3-tk`

[Tkinter](https://en.wikipedia.org/wiki/Tkinter) is Python's de facto standard GUI. We are going to import the module into our Python program to create interactive graphic user interface for our calculator.

```python
import tkinter
import tkinter.messagebox
```
The `import` keyword is used to import a module in Python. We have imported `tkinter` and `tkinter.messagebox` into our program. If you are running Python3 on Ubuntu and tkinter is not installed, run the following command in your terminal: `sudo apt-get install python3-tk`

```python
box = tkinter.Tk()
...
box.mainloop()
```
`box = tkinter.Tk()` initialize the TK module. This allows us to use TK functions within our program.

### Configure Xcalc
```python
WIN_WIDTH = 360
WIN_HEIGHT = 420

NUM_COLUMN_COUNT = 3
OPERATOR_COLUMN_COUNT = 3

BTN_WIDTH = 4
BTN_HEIGTH = 3
BTN_FONT = ("Arial", 14)

DISPLAY_HEIGHT = 3
DISPLAY_WIDTH = 20
DISPLAY_FONT = ("Arial", 18)
DISPLAY_SM_FONT = ("Arial", 14)

DISPLAY_CHAR_LIMIT = 20
```
The use of variables within Xcalc functions means that you can customize the calculator properties without edition the code functions.

```python
WIN_WIDTH = 360
```
Set calculator window width.

```python
WIN_HEIGHT = 420
```

Set calculator window height.
```python
NUM_COLUMN_COUNT = 3
```
Configure number of buttons per row on the numbers column.

Set calculator window height.
```python
OPERATOR_COLUMN_COUNT = 3
```
Configure number of buttons per row on the operator column.
