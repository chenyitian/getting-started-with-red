## 4. Some Rote GUI, File, and Data List Examples in Rebol

This section demonstrates a variety of code patterns which are commonly used to create apps in Red. They make use of user interfaces, functions, variables, string concatenation, lists, loops, conditional evaluations, and other code structures which you've been introduced to already. The examples may seen randomly selected or trivial - just paste or type each of them into a script file, by rote, and then compile/run the code. You'll use variations of each snippet in the 20+ example apps which make up the rest of the tutorial. The first examples focus on performing basic interactions with a user interface, then progress towards handling more interesting data manipulation operations. Pay particular attention to the use of lists and loops, as the examples advance.

### 4.1 Basic GUI Examples

Create a window with a button:

```
Red [needs: 'view]
view [
    button "Click Me"
]

```

When the button is clicked, do something:

```
Red [needs: 'view]
view [
    button "Click Me" [print "I've been clicked!"]
]

```

Here's a window with a button and a text entry field labeled 'f. When the button is clicked, print the text currently in the 'f field. Try typing something in the field, then click the button:

```
Red [needs: 'view]
view [
    f: field "Type here, then click the button"
    button "Click Me" [print f/text]
]

```

When the button is clicked, write the contents of the field to the file mytext.txt, then print that the file has been saved:

```
Red [needs: 'view]
view [
    f: field "Type here, then click the button"
    button "Click Me" [
        write %mytext.txt f/text
        print "Saved"
    ]
]

```

Add another button to read the file contents back into the field. Now you can close the program, run it again, and retrieve the saved text:

```
Red [needs: 'view]
view [
    f: field 
    button "Save" [
        write %mytext.txt f/text
        print "Saved"
    ]
    button "Load" [
        f/text: form read %mytext.txt
    ]
]

```

Here's the exact same program as above, except with a text area widget, instead of a one-line field. This forms a simple text editor:

```
Red [needs: 'view]
view [
    a: area
    button "Save" [
        save %mytext.txt a/text
        print "Saved"
    ]
    button "Load" [
        a/text: load %mytext.txt
    ]
]

```

Here the text is *appended* to mylist.txt. Instead of overwriting the file contents each time, each new save operation loads the previous file, then adds an additional new line. The 'append function joins together the loaded text with the new text (along with some newline characters), so that a multi-line log file is created:

```
Red [needs: 'view]
view [
    below
    f: field "Enter some lines here..."
    button "Save" [
        save %mylist.txt append append load %mylist.txt f/text "^M^/"
        print "Saved"
    ]
    a: area "All log entries will appear here when loaded..."
    button "Load" [
        a/text: form load %mylist.txt
    ]
]

```

### 4.2 Some GUI Examples Using Lists and Loops

Now here are some examples which display lists in a GUI layout, using the 'text-list widget:

```
Red [needs: 'view]
view [
    text-list data ["John" "Dave" "Jane" "Bob" "Sue"]
]

```

You can display a previously created list:

```
Red [needs: 'view]
mylist: copy ["John" "Dave" "Jane" "Bob" "Sue"]
view [
    text-list data mylist
]

```

A 'do block can be used to evaluate code and make changes to the GUI, before a layout is displayed. Here, a blank text-list widget is created, then a block of values is added to it, before being displayed:

```
Red [needs: 'view]
mylist: copy ["John" "Dave" "Jane" "Bob" "Sue"]
view [
    t: text-list data []
    do [append t/data mylist]
]

```

You can also add items to a widget, using the action block of another widget:

```
Red [needs: 'view]
mylist: copy ["John" "Dave" "Jane" "Bob" "Sue"]
view [
    t: text-list data []
    button "Add Items to List" [append t/data mylist]
]

```

This example uses the 'extract function to create a *new list* containing just the names from the original list, which is then displayed in the text-list widget:

```
mylist: copy ["John" 2804 "Dave" 9439 "Jane" 2386 "Bob" 9823 "Sue" 4217]
names: extract mylist 2
view [
    t: text-list data names
]

```

This example does the same thing as above, using a 'foreach loop technique. First, a blank 'names list is created using "copy []", then items are added to this list using 'foreach and 'append. We'll use this code pattern going forward, to help familiarize the use of 'foreach, since it is very common in all types of Red apps:

```
Red [needs: 'view]
mylist: ["John" 2804 "Dave" 9439 "Jane" 2386 "Bob" 9823 "Sue" 4217]
names: copy []
foreach [n c] mylist [append names n]
view [
    t: text-list data names
]

```

The "index" of an item in a list is its numerical position in the list. The index of the clicked item in a text-list widget is referred to with "/selected":

```
Red [needs: 'view]
mylist: ["John" 2804 "Dave" 9439 "Jane" 2386 "Bob" 9823 "Sue" 4217]
names: extract mylist 2
view [
    t: text-list data names [probe t/selected]
]

```

You can use a selected index, to pick out actual values from the text-list data block:

```
Red [needs: 'view]
mylist: ["John" 2804 "Dave" 9439 "Jane" 2386 "Bob" 9823 "Sue" 4217]
names: copy []
foreach [n c] mylist [append names n]
view [
    t: text-list data names [probe pick t/data t/selected]
]

```

The word 'face can be used when a widget refers to itself. Here's the same example as above, using 'face instead of 't:

```
Red [needs: 'view]
mylist: ["John" 2804 "Dave" 9439 "Jane" 2386 "Bob" 9823 "Sue" 4217]
names: copy []
foreach [n c] mylist [append names n]
view [
    t: text-list data names [
        probe form index? find mylist (pick face/data face/selected)
    ]
]

```

You can select the next item in a list by adding 1 to the current item's index. In the 'mylist example below, each number value is always found at the next index location, immediately following every name (i.e., "Jane" is at position 5 in the example list, her number 2386, is at position 6). In code, that's thought of as 1 + (Jane's index position):

```
Red [needs: 'view]
mylist: ["John" 2804 "Dave" 9439 "Jane" 2386 "Bob" 9823 "Sue" 4217]
names: copy []
foreach [n c] mylist [append names n]
view [
    t: text-list data names [
        print form pick mylist (
            1 + index? find mylist (pick t/data t/selected)
        )
    ]
]

```

### 4.3 Building a Basic CRUD (create, read, update, delete) Contacts App

Here's the same idea as above, but with more data fields for each name (3 columns of table data, instead of 2). Name, Address, and Phone values can be found at consecutive Name, Name + 1, and Name + 2 positions:

```
Red [needs: 'view]
mycontacts: [
    "John Smith" "123 Tomline Lane Forest Hills, NJ" "555-1234"
    "Paul Thompson" "234 Georgetown Pl. Peanut Grove, AL" "555-2345"
    "Jim Persee" "345 Pickles Pike Orange Grove, FL" "555-3456"
    "George Jones" "456 Topforge Court Mountain Creek, CO" ""
    "Tim Paulson" "" "555-5678"
]
names: copy []
foreach [name address phone] mycontacts [append names name]
view [
    t: text-list data names [
        print [
            "Name: "
            pick t/data t/selected
            " Address: "
            pick mycontacts (
                1 + index? find mycontacts (pick t/data t/selected)
            )
            " Phone: "
            pick mycontacts (
                2 + index? find mycontacts (pick t/data t/selected)
            )
        ]
    ]
]

```

Instead of printing some rejoined text, display each data item in a text entry field:

```
Red [needs: 'view]
mycontacts: copy [
    "John Smith" "123 Tomline Lane Forest Hills, NJ" "555-1234"
    "Paul Thompson" "234 Georgetown Pl. Peanut Grove, AL" "555-2345"
    "Jim Persee" "345 Pickles Pike Orange Grove, FL" "555-3456"
    "George Jones" "456 Topforge Court Mountain Creek, CO" ""
    "Tim Paulson" "" "555-5678"
]
names: copy []
foreach [name address phone] mycontacts [append names name]
view [
    below
    t: text-list data names [
        n/text: pick t/data t/selected
        a/text: pick mycontacts (
            1 + index? find mycontacts (pick t/data t/selected)
        )
        p/text: pick mycontacts (
            2 + index? find mycontacts (pick t/data t/selected)
        )
    ]
    text "Name:"
    n: field
    text "Address:"
    a: field
    text "Phone:"
    p: field
]

```

Here's a version of the code above, with a nicer layout using the 'panel widget:

```
Red [needs: 'view]
mycontacts: copy [
    "John Smith" "123 Tomline Lane Forest Hills, NJ" "555-1234"
    "Paul Thompson" "234 Georgetown Pl. Peanut Grove, AL" "555-2345"
    "Jim Persee" "345 Pickles Pike Orange Grove, FL" "555-3456"
    "George Jones" "456 Topforge Court Mountain Creek, CO" ""
    "Tim Paulson" "" "555-5678"
]
names: copy []
foreach [name address phone] mycontacts [append names name]
view [
    t: text-list data names [
        n/text: pick t/data t/selected
        a/text: pick mycontacts (
            1 + index? find mycontacts pick t/data t/selected
        )
        p/text: pick mycontacts (
            2 + index? find mycontacts pick t/data t/selected
        )
    ]
    panel [
        below
        text "Name:"
        n: field 200
        text "Address:"
        a: field 200
        text "Phone:"
        p: field 200
    ]
]

```

Add items to a block (data displayed in a text-list) by entering text into field widgets:

```
Red [needs: 'view]
contacts: copy []  names: copy [""]
view [
    t: text-list data names [
        n/text: pick t/data t/selected
        a/text: pick contacts (
            1 + index? find contacts pick t/data t/selected
        )
        p/text: pick contacts (
            2 + index? find contacts pick t/data t/selected
        )
    ]
    panel [
        below
        text "Name:"
        n: field 200
        text "Address:"
        a: field 200
        text "Phone:"
        p: field 200
        button "Add" [
            append contacts reduce [copy n/text copy a/text copy p/text]
            clear names
            foreach [name address phone] contacts [append names name]
        ]
    ]
]

```

This example uses a function to encapsulate duplicated code (which doesn't shorten the whole program much here, but if the duplicated code was, for example, 20 lines long and used in 5 different places, the whole program would be about 100 lines shorter):

```
Red [needs: 'view]
extract-names: func [] [
    clear names
    foreach [name address phone] contacts [append names name]
    copy names
]
contacts: copy []  names: copy [""]
view [
    t: text-list data names [
        n/text: pick t/data t/selected
        a/text: pick contacts (
            1 + index? find contacts pick t/data t/selected
        )
        p/text: pick contacts (
            2 + index? find contacts pick t/data t/selected
        )
    ]
    panel [
        below
        text "Name:"
        n: field 200
        text "Address:"
        a: field 200
        text "Phone:"
        p: field 200
        button "Add" [
            append contacts reduce [copy n/text copy a/text copy p/text]
            extract-names
        ]
    ]
]

```

Load a list from file using 'load %filename, and assign it a variable label. Save a list to file using 'save %filename [list]. Create an empty file using save %filename [], only if it doesn't already exist (if the file is not found in the reading of the current directly listing):

```
Red [needs: 'view]
extract-names: func [] [
    clear names
    foreach [name address phone] contacts [append names name]
    if names = [] [names: [""]]
    copy names
]
if not find read %. %contacts [save %contacts []]
contacts: load %contacts   
names: [""]
names: extract-names
view [
    title "Contacts"
    t: text-list data names [
        n/text: copy pick t/data t/selected
        a/text: copy pick contacts (
            1 + index? find contacts pick t/data t/selected
        )
        p/text: copy pick contacts (
            2 + index? find contacts pick t/data t/selected
        )
    ]
    panel [
        below
        text "Name:"
        n: field 200
        text "Address:"
        a: field 200
        text "Phone:"
        p: field 200
        button "Add" [
            append contacts reduce [copy n/text copy a/text copy p/text]
            extract-names
        ]
        button "Save" [save %contacts contacts]
    ]
]

```

Notice that a 'title widget with the text "Contacts" was added to the program above, which is displayed in the title bar of the GUI window.

You could create a wide variety of basic "CRUD" (create, read, update, delete) apps using variations of the code above. So far, this program actually only creates and reads, but we'll add more to it shortly.