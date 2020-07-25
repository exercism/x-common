If you want to build something using a Raspberry Pi, you'll probably use _resistors_. For this exercise, you need to know two things about them:

* Each resistor has a resistance value.
* Resistors are small - so small in fact that if you printed the resistance value on them, it would be hard to read.

To get around this problem, manufacturers print color-coded bands onto the resistors to denote their resistance values. Each band has a numeric and a positional value. For example, if they printed a brown band (value 1) followed by a green band (value 5), it would translate to the numeric value.

In this exercise you are going to create a helpful program so that you don't have to remember the numeric values of the bands. The program will take color names as input and output a numeric value of at most two digits.

The band colors are encoded as follows:

- Black: 0
- Brown: 1
- Red: 2
- Orange: 3
- Yellow: 4
- Green: 5
- Blue: 6
- Violet: 7
- Grey: 8
- White: 9

From the example above:

- Brown and green should return 15.
- If your language track enables more than two colors as input, such as brown, green and violet, this should return 15, too, ignoring additional colors beyond the first two.
