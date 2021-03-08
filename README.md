# Toy-Store
A program used for working with CSV files of products for a toy store. Once run,
it acts as a command-line terminal, allowing the user to change the store currency,
add some discounts, add a new currency, list all the products in the store and much more.

Oprea-Groza Gabriel

My program writes all the output to the RESULT.OUT file, and in 
exceptional cases, messages corresponding to STDOUT are displayed.
The tests were made by me, to confirm the correctness of the application.
Design patterns used: Builder, Singleton, Strategy


The testX_out folder contains the result.out file and the other
.csv or .bin files generated.

For this assignment I implemented all the bonuses and required design patterns.
The additional pattern I used is Strategy Pattern.
My idea was to create a common interface to be implemented by
all possible orders. Thus, I created a class for each command that implements
the interface, which has 2 methods:
canProcess which decides whether a command can process the received input. With other
words, it is useful to select the correct command. The execute method does
the actual processing, based on the command's argument list, arguments
which I added in a vector called args. Basically, args [0] represents
the first command argument, the second args[1], etc.

In the Terminal class, I have a static field that is the Writer, which is used by
all commands to write the required information. The execute command from the terminal
will instantiate all commands, then iterate through them and decide which one is the 
right one, based on the canProcess method. After the right command is found,
its execute method is called.

Regarding the exceptions, I added a few new ones, which seemed to me
relevant, namely CommandNotFoundException and ProductNotFoundException.
I added them because I thought that in such an application, it is possible
for the user to give a potentially valid command, but which has not been
implemented still(in this case I throw CommandNotFoundException). The exception
ProductNotFoundException is thrown when an input ID is
wrong in the showproduct or calculatetotal commands.

The other classes such as Store, Discount, Currency, Manufacturer, Product
are the fundamental ones in this application. To instantiate a product I used
the ProductBuilder class, which uses the Product class setter to "build" an instance.

In the Store class I also made many useful methods to search for a product,
a discount or a currency in the application. A Currency can be searched both
by its name, as well as by the symbol (the search by symbol is done when we import
a new .csv file, and we want to know what parity the current currency has,
of which we know only the symbol.

I chose the implementation with ArrayList in favor of the one with arrays. 
The fields of a Store are represented by an ArrayList of
discounts, currencies, manufacturers, products, one current currency
and a name. For the Store I used the singleton design pattern, not only
to have a single instance of the store, but also to facilitate easier access
to the store by any entity in the application.

To solve the assignment I used the additional library apache commons,
of which I used the Pair to build a currency-price pair,
commons csv to be able to easily parse the information from the input file,
and math3.util to be able to round the calculations performed by
applying discounts to a fixed number of decimals.

In the readCSV method I iterate through each line of the .csv file and take all the
necessary information. If a product has no price, I ignore that field. If
the manufacturer has appeared before in the file, I increase his number of
products. Regarding the currency, I use pairs, search by symbol and convert
from string to double to get the value and the corresponding parity. Finally,
with all the information obtained, I instantiate a new product with the corresponding
builder, and I add the product in an ArrayList which I return. This ArrayList will be
used by the command readcsv, which will process and add each product from the array
to the store.

The writeCSV method takes all the necessary information about each product and
writes them in the file. In order to be able to write the Store in a binary file,
I have chosen for all objects inside a Store to implement the interface
Serializable.

Regarding the tests:

test0.in is like the original 0 test, slightly modified. The out.csv file is
identical to the cleaned one, except for the price format (I decided to write
each price by 2 decimal places).
test1.in is a simple test to confirm the correctness of listing commands
test2.in checks both the use of discounts and binary file writing
test3.in is specially built for those exceptional situations (products
does not exist, negative prices are obtained, etc.)
test4.in makes a lot of conversions between cryptocurrencies
test5.in combines the principles of the other tests, with many discounts
and conversions, and generates 4 .csv files
