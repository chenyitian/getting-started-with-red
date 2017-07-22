## 3. Red Fundamentals

### 3.1 Functions

Functions are words which perform actions upon data "arguments" (or "parameters"). The 'print function prints some text to the Red console. In the following code, 'print is the function, "Hello World" is the argument. Try pasting this example into the Red console:

```
print "Hello World"

```

The 'halt function doesn't require any argument. It just stops the program and displays the console:

```
halt

```

The 'request-file function pops up a dialog which allows the user to select a file from the local file system:

```
request-file

```

In Red, some functions have optional "refinements", which allow the function to accept a variety of potentially useful parameters. Here are some refinements of the 'request-file function. Try them in the console:

```
request-file/file %test.red
request-file/file/save %test.red

```

#### 3.1.1 Return Values

The 'ask function *returns*, or outputs, the text entered by the user:

```
ask "Enter some text:  "

```

When working in the console, the return values of functions are automatically printed. When working in a script file, you can *print* the return value of a function to the console:

```
Red []        ; header, because this will be run from a script file
print ask "Enter some text:  "
halt          ; this stops the interpreter from closing after printing

```

The example below uses the 'print function to display the output of the 'request-dir function, right back to the user:

```
print request-dir

```

Note that before the 'print function above can complete its operation, a value first needs to be returned from the 'request-dir function. Because of that requirement, when this code runs, the print function is held up temporarily, and the first thing the user sees is the 'request-dir popup.

Using the output value of one function as the input argument of another function is a routine way of composing code in Red.

### 3.2 Data Persistence - Red Requires No Database System

To save text data to a hard drive, thumb drive, or other storage medium, use the 'write function. The 'write function takes two arguments: a file location, and the data to be written to that file. Local file names are preceded by the percent symbol (%):

```
write %mydata "This is some text that I want to save for later."

```

To retrieve the text later, use the 'read function. You can use the 'print function to print formatted data read from a file, or the 'probe function to view the raw data without any formatting:

```
print read %mydata

probe read %mydata

```

You can read from servers at a network/Internet URL, in the same way you read from local drives on your computer:

```
read http://site.com/mydata.txt

```

The /binary refinement allows you to read data byte for byte:

```
read/binary http://redprogramming.com/Home.html

```

Combining 'write with 'read performs a download:

```
write %r.html read/binary http://redprogramming.com/Home.html

```

To store complex structured data types, in any format which is natively understood by Red, use the 'save function:

```
save %mycontacts ["John" "Bill" "Jane" "Ron" "Sue"]

```

To load saved structured data, use the "load" function:

```
probe load %mycontacts

```

You'll see that Red provides all the required abilities to search, sort, compare, combine, add, remove, change, load, save, and otherwise manipulate persistent data. Together, all these features provide the same capabilities as database systems such as MySQL, SQLite, etc., with a shorter and more consistent learning curve, and more fine grained control of data processing algorithms, all in a tiny, 100% portable tool set. The functions learned, and the set of skills used to manage persistent data structures in Red are also used to tackle many other sorts of problem domains in the language (access to network socket connections, email accounts, graphic displays, console and IO interactions, etc.). This single set of skills is much more broadly applicable in Red, than for example SQL is in other languages (SQL is a domain specific language which can only be applied to processing information in database operations). This consistently applied concept is a basic feature which helps Red maintain simplicity. The issue of managing concurrent access to data by multiple client users is [easily solved](http://re-bol.com/rebol-multi-client-databases.html) in Red.

### 3.3 Variables

Variables are word labels (some series of characters) assigned to represent data in a program. In Red, word labels are created with the colon symbol. Try pasting these examples into the Red console:

```
name: "John"
probe name

```

Variables can be **changed** to hold different values (the data they refer to is *variable*). The code above prints "John". The code below prints "Bill":

```
name: "Bill"
probe name

```

Note that Red is *case insensitive*. The interpreter treats lower case and capital letters in identifiers the same:

```
name: "George"
print name
print Name
print NAME
print NaMe

```

You can assign a variable word label to data returned by requestor functions. This code prints the name of the file which the user selects:

```
name: request-file
probe name

```

You can assign a label to data read from a file:

```
data: read %mydata
probe data

```

You can assign a label to any *block of code*, and evaluate (run) that code with the 'do function:

```
mycode: [print "Hello" halt]
do mycode

```

### 3.4 Conditional Evaluations

Most programs make use of "if/then" evaluations: if (this) is true [do this]:

```
account: -10
if account < 0 [print "Your account is overdrawn."]

```

"Either" is like if/else evaluations in other languages: if (this) is true [do this] [otherwise do this]. Notice that each separate action block is indented, so that you can clearly see two 2 differentiated actions. Notice also that this interactive example is wrapped in a "do []" block:

```
do [
    pass: ask "Enter Password:  "
    either pass = "secret" [
        print "Welcome back."
    ] [
        print "Incorrect password."
    ]
]

```

"Case" can be used to choose between a variety of actions, depending on the situation:

```
name: "john"
case [
    find name "a" [print {Your name contains the letter "a"}]
    find name "e" [print {Your name contains the letter "e"}]
    find name "i" [print {Your name contains the letter "i"}]
    find name "o" [print {Your name contains the letter "o"}]
    find name "u" [print {Your name contains the letter "u"}]
    true [print {Your name doesn't contain any vowels!}]
]

```

There are many other conditional evaluation structures in Red, but these are enough to handle most situations.

### 3.5 Using Text Strings and Concatenation

{Curly braces} can be used instead of quotes, to enclose strings of text which contain quote characters, or multi-line strings:

```
print {She said "Hi"}

print {
    line 1
    line 2
    line 3
}

```

The 'trim function provides a number of refinements which are helpful in removing white space from strings:

```
print trim {
    line 1
    line 2
    line 3
}

print trim/all {
    line 1
    line 2
    line 3
}

```

The "^/" character is used to represent a carriage return:

```
print "line 1^/line 2^/line 3"

```

The "prin" function is used to print text without a carriage return:

```
prin "no newline " prin "add a newline manually" prin newline  ; "^/"

```

"Concatenation" is the process of joining together multiple pieces of data. Red uses the 'append function to concatenate text:

```
print append "Chosen file name: " request-file

```

Most programs make use of concatenated values which are represented by variable labels, typically obtained from user input, resulting from conditional operations, resulting from various potential program states, or other data items which change dynamically during the operation of the program. Notice that concatenated items can be placed on multiple lines and indented for readability:

```
do [
    name: ask "Your Name:  "
    file: request-file
    print append append name ", your chosen file is: " file
]

```

When printing multiple values, there's no need to concatenate. Just put each separate value inside a block, and they will be joined automatically when printed:

```
name: "John"  birthday: "1-Jan-1999"  phone: "123-234-3456"
prin ["name: " name  "  birthday: " birthday "  phone: " phone]

```

You can concatenate strings of code, and run that code using the 'do function (this is a simple form of "metaprogramming"):

```
do [
    code: ask {Enter some code:  }   ; for example, {print "hello"}
    do append {print "Here's your running code..."} code
]

```

### 3.6 Lists

Most computer applications deal with some sort of data list(s): lists of coordinates in a graphics program, lists of items (and perhaps their prices, sizes, shipping dates, etc.) in an inventory program, lists of names (and their account numbers, passwords, transaction details, etc.) in a financial program, lists of files in a utility program, emails in an email program, etc. Red's main list structure is called the "series". There are a number of functions built into Red which allow you to add, remove, change, search, sort, compare, combine, load, save, and otherwise manipulate series data. Manipulating characters within strings of text (lists of characters), graphics on screen in a game (list of images and screen locations in a draw block), pieces of data being transferred between network ports, etc. all require knowledge of only that one simple set of functions.

The basic code structure used to store series of data items in Red is called the "block". Just put square brackets around a list of items, and assign it a variable label:

```
names: ["John" "Dave" "Jane" "Bob" "Sue"]
codes: [2804 9439 2386 9823 4217]
files: [%employees %vendors %contractors %events]

```

You can pick, find, and sort items from the list using simple functions and constructs:

```
print pick names 3          ; these two lines do
print names/3               ; exactly the same thing
print find names "Dave"
print sort names
print sort codes

```

Notice in the sort examples above that each type of data is automatically sorted appropriately (names alphabetically, numbers ordinally, etc.).

Save and load blocks of data using the 'save and 'load functions:

```
names: ["John" "Dave" "Jane" "Bob" "Sue"]
save %mynames names
loaded-names: load %mynames
probe loaded-names

```

Mastering series manipulation is a core skill required for Red proficiency. These functions will become thoroughly familiar: pick, find, at, index?, length?, append, remove, insert, extract, copy, replace, select, sort, reverse, head, next, back, last, tail, skip, change, poke, clear, join, intersect, difference, exclude, union, unique, empty?, write, read, save, load.

### 3.7 Loops

Loops are used to do something to or with each consecutive item in a list. 'Foreach is the most commonly used type of loop in Rebol:

```
names: ["John" "Dave" "Jane" "Bob" "Sue"]
foreach name names [print name]

```

The variable labels in loops can be set to any arbitrary word. For example, this example does the exact same thing as the code above:

```
n: ["John" "Dave" "Jane" "Bob" "Sue"]
foreach i n [print i]

```

Very often, you'll check each item in a list for a condition, and do something appropriate based on the evaluation of each item:

```
names: ["John" "Dave" "Jane" "Bob" "Sue"]
foreach name names [
    if find name "j" [print name]
]

numbers: [323 2498 94321 31 82]
foreach number numbers [
    if number > 300 [print form number]
]

```

You can structure data in a list to form rows and columns. The following foreach loop labels every 3 consecutive pieces of data in the 'mycontacts list as a name, address, and phone value. Notice that "" is used as an empty place holder:

```
mycontacts: [
    "John Smith" "123 Tomline Lane Forest Hills, NJ" "555-1234"
    "Paul Thompson" "234 Georgetown Pl. Peanut Grove, AL" "555-2345"
    "Jim Persee" "345 Pickles Pike Orange Grove, FL" "555-3456"
    "George Jones" "456 Topforge Court Mountain Creek, CO" ""
    "Tim Paulson" "" "555-5678"
]
foreach [name address phone] mycontacts [
    print name
]

```

You can use the 'extract function, instead of a 'foreach loop, as a shortcut to get columns of data:

```
mycontacts: [
    "John Smith" "123 Tomline Lane Forest Hills, NJ" "555-1234"
    "Paul Thompson" "234 Georgetown Pl. Peanut Grove, AL" "555-2345"
    "Jim Persee" "345 Pickles Pike Orange Grove, FL" "555-3456"
    "George Jones" "456 Topforge Court Mountain Creek, CO" ""
    "Tim Paulson" "" "555-5678"
]
probe extract mycontacts 3          ; every 3 items 
                                    ; (the 1st column of above data)
probe extract/index mycontacts 3 3  ; every 3 items, starting with 3rd
                                    ; (the 3rd column of above data)

```

You can use 'repeat loops much like 'foreach, to count through items in a list. This example sets a counter variable 'i to count from 1 to the length of the names block, incrementing the variable by 1 each time through the loop:

```
names: ["John" "Dave" "Jane" "Bob" "Sue"]
repeat i (length? names) [
    print append append form i ": " pick names i
]

names: ["John" "Dave" "Jane" "Bob" "Sue"]
repeat i (length? names) [
    print append append pick names i " next: " pick names (i + 1)
]

```

A 'forever loop just repeats forever, until a 'break function is encountered:

```
count: 99
forever [
    print append form count " bottles of beer on the wall"
    count: count - 1
    if count = 0 [break]
]

```

### 3.8 User Interfaces

#### 3.8.1 Console Apps

Utility scripts often print a quick (non-graphic) display of data to the Red console, or perhaps require a brief answer to a question asked at the console text prompt. You can use the 'ask and 'print functions to do simple console i/o. Save this code to a file and run it (type "do filename.r" in the console):

```
Red []
user: ask "Username (iwu83):  "
pass: ask "Password (zqo72):  "
either all [user = "iwu83" pass = "zqo72"] [
    print "Welcome back"
] [
    print "Incorrect Username/Password"
]
ask ""

```

#### 3.8.2 GUI Apps

For most "apps", a GUI (graphic user interface) screen is used to input and output data and to control program flow. Buttons, text fields, drop-down selectors, and other "widgets" allow the user to operate the program. Red provides a simple built-in code dialect for creating interactive GUIs. The word "view" create a window layout. A block holds widget descriptions. The words 'below and 'across are used to set the default sequential position of widgets. Save each of the examples in this section to a .red file, and compile them to see the screens created:

```
Red [title: "test" needs: 'view]
view [
    below
    button
    field
    text "Red is really pretty easy to program."
    text-list
    check
]

```

You can adjust the visual characteristics of any widget in a screen layout by following each widget with appropriate modifiers:

```
Red [title: "test" needs: 'view]
view [
    below
    button red "Click Me"              
    field 400 "Enter some text here"  
    text font-size 16 "Red is really pretty easy to program." purple
    text-list 400x300 data ["line 1" "line 2" "another line"]
    check yellow
]

```

You can have widgets perform functions, or any block of code, when clicked or otherwise activated. Just put the functions inside another set of brackets after the widget. This is how you get your GUIs to 'do something':

```
Red [title: "test" needs: 'view]
view [
    button "Click me" [print "You clicked the button."]
]

```

You can refer to data contained in widgets using the path ("/") syntax. Assign a variable label to any widgets which you want to reference:

```
Red [title: "test" needs: 'view]
view [
    below
    text "Some action examples.  Try using each widget:"
    button red "Click Me" [
        print "You clicked the red button."
    ]
    f: field 400 "Type some text here, then press [Enter]" [
        print f/text
    ]
    t: text-list 400x300 data ["Select this" "Then this" "Now this"][
        print pick t/data t/selected
    ]
    check yellow [print "You clicked the yellow check box."]
    button "Quit" [unview]    
]

Red [title: "test" needs: 'view]
view [
    below
    a: area                               ; an area widget labeled 'a
    f: field                              ; a field widget labeled 'f
    across
    button "Show" [print a/text]
    button "Save" [write %somedatafile.txt a/text]
    button "Load" [f/text: read %somedatafile.txt]
]

Red [title: "test" needs: 'view]
view [
    below
    t: text-list data [1 2 3]                ; a text-list labeled 't
    button "Show Selected" [print pick t/data t/selected]
]

```

You can build layouts dynamically using loops, and then display the resulting constructed GUI code:

```
Red [title: "test" needs: 'view]
gui: copy []
foreach color [red green blue] [
    append gui reduce ['text color]
]
view layout gui

```

You'll need to learn a lot more about creating GUI screens if you want to create user apps with Red, but you can get a lot done with just the basics demonstrated above.

Take a look through some [additional GUI examples](http://redprogramming.com/Short%20Red%20Code%20Examples.html) which demonstrate interesting widgets and how to use them.

The examples in the second half of this text will explain and demonstrate by rote many common GUI techniques.

#### 3.8.3 Server and CGI Web Apps

Red has network and data/code transfer features built in, which allow you to easily create client-server applications. Client apps can be written in Red, HTML/CSS/Javascript, or any other language which can connect using standard network protocols (HTTP://, TCP, UDP, etc.). Red also provides native "CGI" capabilities, so that server scripts can run on a typical Apache host or any other web server stack, and accept input from HTML web forms, Ajax requests, etc., then print HTML or formatted data responses to appear in the user's web browser.

A complete section at the end of this text is dedicated to creating CGI applications (stand-alone network applications are covered in Quick Start, Part 2). This allows you to create useful web based data management apps, with Red, which can be accessed on mobile devices, or any platform which has a browser.

#### 3.8.4 Faceless Apps

Certain types of utility scripts may not require any interaction with a user. For example, apps that run in the background to perform scheduled file maintenance, to upload routine data backups, to check and update the current system time, etc., don't necessarily need to show you that they're running. Red scripts can run in memory without any front end display.

### 3.9 User Created Functions

You can create your own functions in Red using the "func" code structure. New functions are assigned a word label with a colon symbol. The label(s) of data arguments which the function will process are listed in a block, then the calculations or other actions which the function performs are included in a following block.

Here's a function called 'triple, which multiplies the number 3 times any given number argument (called 'x here):

```
triple: func [x] [
    print 3 * x
]

```

Now you can apply the 'triple function to any argument value, as if that function was a native function word built into the Red language:

```
triple 4
triple 5
triple 6
print "I just tripled the numbers 4 (12), 5 (15), and 6 (18)"

```

Think of the 'x argument (parameter) in the function definition above just like some program's command line option - it represents some variable data that you send to the program to do some work with. In the 'triple function, the argument represents whatever number is multiplied by 3, and 'x is the variable placeholder used to represent that parameter value.

The 'does structure is a shortcut for 'func, when no data parameters are needed:

```
cls: does [loop 100 [print newline]]
cls

```

#### 3.9.1 Return Values

The last value in a Red function definition is treated as its "return" value. The 'check function below takes a series of string values as its parameter (labeled 'list here), and checks to see if it contains any bad words (in this example, bad words are specified by the characters "--"). This function starts out by setting the variable 'answer to "safe", then uses a 'foreach loop and an 'if condition to see if "--" is found in any of the strings in the series. If at any point the bad characters are found, the 'answer variable is set to "unsafe". At the end of the function the 'answer variable is returned. The function is then run on both the names1 and names2 lists, and the user is shown the returned results:

```
check: func [list] [
    answer: "safe"
    foreach l list [
        if find l "--" [answer: "unsafe"]
    ]
    answer
]
names1: ["Joe" "Dan" "Sh--" "Bill"]
names2: ["Paul" "Tom" "Mike" "John"]
print append "names1 is " check names1
print append "names2 is " check names2

```

#### 3.9.2 Libraries

You can save collections of useful functions ("libraries") to a file, and import them with 'do. This saves you from having to retype or paste the code of commonly used function definitions into each new program. Save just the 'check function code above to a file named "myfunctions.red" - be sure to include the rebol[] header when saving code to a file:

```
Red []
check: func [list] [
    answer: "safe"
    foreach l list [
        if find l "--" [answer: "unsafe"]
    ]
    answer
]

```

The program from the previous section then looks like this:

```
Red []
do %myfunctions.red
names1: ["Joe" "Dan" "Sh--" "Bill"]
names2: ["Paul" "Tom" "Mike" "John"]
print append "names1 is " check names1
print append "names2 is " check names2

```

Imported files can be found on the local file system (i.e., on your hard drive, a thumb drive, etc.), or at a network/Internet URL. For example, if you save the "myfunctions.r" file on your web site server, you could import it like this:

```
do http://site.com/myfunctions.r

```

You can actually include *any* code you want in imported files (not just function definitions). You could put the entire program above into a file on your web site and run it by "do"ing the URL of the file. Importing a file with 'do is exactly the same as copying and pasting the contents of the file into your code, and then executing it.

### 3.10 Handling Errors

The 'error? and 'try functions can be used together to detect runtime errors in Red code. Use an 'if or other conditional evaluation to handle any error events:

```
if error? try [0 / 0] [alert "Dividing by zero is an error"]

```

You can examine error properties by assigning a label to the result of the 'try function:

```
if error? err: try [0 / 0] [probe err]

```

The 'attempt function can also be used to handle errors:

```
attempt [1 / 1]
attempt [0 / 0]

```
