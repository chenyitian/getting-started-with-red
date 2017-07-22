## 5. A Few More Complete App Examples

### 5.1 Tip Calculator

This app calculates the total amount, including tip, to pay for a restaurant meal:

1. A window layout is created. It contains a title widget, 3 text field widgets, labeled 'f,'t, and 'x, and a button widget. The 'below word is used to position each consecutive widget below one another.
2. The 'f and 't fields contain some default money and tip rate values ($9 and .2 (20 percent)).
3. When the button is clicked by the user, the 'x field's text is set to the value computed by multiplying the 'f field's float value, times the 't field's float value + 1 (i.e., the total to pay for a $9 bill is ($9 times 1.2)):

```
Red [needs: 'view]
view [
    title "Tip Calculator"
    below
    f: field "9"
    t: field ".2" 
    button "Calculate" [
        append clear x/text (to float! f/text) * (1 + (to float! t/text))
    ]
    x: field "tip"
]

```

### 5.2 Tile Game

The code below creates a playable tile game. Click any tile piece to move it into the empty space. Rearrange the tiles in ascending order:

1. A GUI window layout is created with the title "Tile Game" and a silver backdrop color.
2. The 'style word, in Red's GUI dialect, is used to create a new 'button object labeled 't. Whenever a 't widget is clicked by the user, three actions occur: the label 'x is set to the box's current 'offset (coordinate position), the box's coordinate position is set to the 'e widget's position, and the 'e widget's position is changed to the 'x position. These three actions together effectively *swap* the positions of the clicked button and the 'e widget (as you'll see below, the widget labeled 'e is just a plain silver base widget (box), which appears as a blank space in the window layout).
3. The 'return word, in Red's GUI dialect, is used to start a new "line" of widgets (similar to how a carriage return is used to start a new line in a text document).
4. A bunch of 't box widgets are added to lines in the window layout, along with the final base widget labeled 'e. Any time you click a numbered button, its position is swapped with the empty silver box.

```
Red [needs: 'view] 
view [ 
     title "Tile Game"
     backdrop silver
     style t: button 100x100 [
         x: face/offset
         face/offset: e/offset 
         e/offset: x
     ] 
     t "8"  t "7"  t "6"  return 
     t "5"  t "4"  t "3"  return 
     t "2"  t" 1"  e: base silver
]

```

### 5.3 Generic Calculator

Below is a calculator app. Most of the code should make sense.

1. A GUI window is created, with the title "Calculator"
2. The window layout includes a field labeled 'f. It is 230x50 pixels in size, and the font size of text displayed in it is 25.
3. The 'style word is used to create a new button object called 'b, which is 50x50 pixels in size, and which appends the current button's face text to the 'f field widget's text, whenever one of the 'b buttons is clicked.
4. A bunch of 'b buttons are added to the window layout, each displaying a different number or mathematical operator. The 'return word is used to start new lines of buttons.
5. The "=" button attempts to set the 'f field's text to whatever results in "do"ing (evaluating) the current mathematical expression displayed in the 'f field.

```
Red [needs: 'view]
view [
     title "Calculator"
     f: field 230x50 font-size 25 ""  return 
     style b: button 50x50 [append f/text face/text]
     b "1"  b "2"  b "3"  b " + "  return 
     b "4"  b "5"  b "6"  b " - "  return 
     b "7"  b "8"  b "9"  b " * "  return 
     b "0"  b "."  b " / "  b "=" [attempt [
             calculation: form do f/text 
             append clear f/text calculation
     ]] 
]

```

NOTE: if you want to work with floats (fractions) in this calculator app, be sure to include a decimal point in at least one of the entered numbers (i.e., 1.0 / 2, instead of 1 / 2)

Try building this same app in any other programming language, and you'll see that there is absolutely nothing simpler than Red's dialect approach to building windowed programs.

### 5.4 Coin Flip

Clicking the button in this app randomly flips between a heads and tails coin image:

1. Two images are loaded from specified URLs on the Internet. The label 'h is assigned to the loaded heads image. The label 't is assigned to the loaded tails image.
2. A window layout is created with the title "Coin Flip" and the 'below layout directive. It contains an image widget labeled 'i which initially displays the 'h image (the coin head image), a text field entry widget labeled 'f, and a button displaying the text "Flip".
3. When the button is clicked, it sets the 'f field text to the first item in a randomly ordered list of the 2 text strings "Heads" and "Tails".
4. Next, an 'either conditional evaluation is performed. If the text in the 'f field is "Heads" (that random state was determined in the previous step), then the image value of the 'i widget is set to the 'h image (the heads image). Otherwise (i.e., if the text in the 'f field is "Tails"), then the image in the 'i widget is set to 't (the tails image).
5. The display (labeled 'g) is updated using the 'show function.

```
Red [needs: 'view]
h: load http://re-bol.com/heads.jpg
t: load http://re-bol.com/tails.jpg
view [
    title: "Coin Flip"
    below
    i: image h
    f: field
    button "Flip" [
        f/text: first random ["Heads" "Tails"]
        either f/text = "Heads" [i/image: h] [i/image: t] 
    ]
]

```

NOTE: In this example, the random number generator is not seeded. This tutorial will be updated when that functionality ('random/seed now' in Rebol) is implemented more simply in Red.

### 5.5 Additional Example Apps to Study

Here are a few more short app examples. See if you can follow the basic flow of code in each program. Pay attention to which pieces of code are variable labels, GUI layout widgets, conditional evaluations, etc.

Here's an app to quiz users on basic addition math facts. Try changing it to quiz subtraction, multiplication, and division on selected ranges of random numbers (i.e., subtraction on numbers 1 - 100 or multiplication on numbers 1 - 12):

```
Red [needs: 'view]
x: func [] [append append form random 10 " + " form random 20]
view [
    title "Math Test"
    f1: field 
    f2: field "Answer_here..."
    button "Check Answer" [
        print either f2/text = form do f1/text ["Yes!"]["No"]
        f1/text: x
        f2/text: ""
    ]
    do [f1/text: x]
]

```

Try changing this one to generate insults:

```
Red [needs: 'view]
xx: form x: ["brilliant" "rare" "unique" "talented" "exceptional"]
yy: form y: ["genius" "champion" "winner" "success" "achiever"]
view [
    title "Compliment Generator"
    below
    area xx
    area yy
    button "Compliment" [
        print [
            "You're a"
            first random x
            first random y "!"
        ]
    ] 
]

```

More examples are coming soon!

(Be sure to see [http://re-bol.com/rebol_quick_start.html](http://re-bol.com/rebol_quick_start.html) and [http://re-bol.com/short_rebol_examples.r](http://re-bol.com/short_rebol_examples.r) to see where this tutorial is heading, as Red matures to become as full featured as Rebol 2).