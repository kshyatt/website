
# The Args Named Argument System

### The Problem

A unfortunate fact about C++ functions is that arguments must be passed in a fixed order.
This is a nuisance when you are happy with the default value of, say, the third argument,
but have to provide it anyway to reach the fourth and fifth arguments.

To make matters worse, the values passed to a function can be opaque and hard to interpret.
<br/>In the following code

    truncateMPS(psi,500,1E-5,false);

it is fairly clear that psi is some matrix product state to be truncated, but what do
the other parameters mean?

In the above example, if the truncateMPS function accepted an Args object instead, we
could call it like this

    truncateMPS(psi,{"Maxm",500,"Cutoff",1E-9,"ShowSizes",false});

This much easier to read and lets us specify only those parameters we care about,
leaving the rest to have default values. For example, if we are happy with the default
value for "Maxm" and "ShowSizes", we could just call truncateMPS as

    truncateMPS(psi,{"Cutoff",1E-9});

We can also pass the named arguments in any order we like

    truncateMPS(psi,{"ShowSizes",false,"Maxm",500,"Cutoff",1E-9});

### Setting a Function to Accept Args

To allow a function to take an Args object, first make sure the Args class is available

    #include "args.h"

<!--v1-->
Next define the last argument of your function to be

    void func(..., Args const& args = Args::global());

where the "..." means all the usual arguments the function "func" accepts
(the return type is void here for simplicity but could be any type).
For example, the truncateMPS function above could be declared as

    MPS truncateMPS(MPS const& psi, Args const& args = Args::global());

Making the default value `Args::global()` does the following: if no named arguments
are passed then "args" will refer to the global Args object. By default the
global Args object is empty;
however, one can add arguments to Args::global() to set
global defaults&mdash;for more details see the section on argument lookup order below.

Sometimes you may want to further modify the args object within your function.
For such cases it is better to accept args by value

    void func(..., Args args = Args::global());

### Accessing Named Arguments Within a Function

Using the fictitious truncateMPS function as an example, recall that it 
accepts three named arguments:
* "Maxm" &mdash; an integer
* "Cutoff" &mdash; a real number
* "ShowSizes" &mdash; a boolean

(Named arguments can also be string valued.)

To access these arguments in the body of the function, do the following:

    MPS
    truncateMPS(MPS const& psi,
                Args const& args) //no Args::global() because we already
                                  //specified it in the declaration
        {
        auto maxm = args.getInt("Maxm",5000);
        auto cutoff = args.getReal("Cutoff",1E-12);
        auto show_sizes = args.getBool("ShowSizes",false);

        ...

        }


The second argument to each of the above "get..." methods is the default value
that will be used if that argument is not present in `args`. The 
order in which these functions are called is not important.

Occasionally a named argument should be mandatory. To make it so, just
leave out the default value when calling getInt, getReal, getBool, or getString:

        void
        func(...
             Args const& args)
        {
        //Mandatory named argument
        auto result_name = args.getString("ResultName");
        ...
        }


### Lookup Order and Global Args

When calling one of the "get..." methods (such as getInt or getString) on an Args instance,
the value returned will be as follows:

1. The value if defined in the instance itself

2. If not defined in the instance, the value defined in the global Args object

3. If not defined in the global Args, the default value provided to "get..."

Here is an example:

    Args::global().add("DoPrint",true);

    auto args = Args("Cutoff",1E-10);

    auto do_print = args.getBool("DoPrint",false);
        // ^ do_print == true, found in Args::global()

    auto cutoff = args.getReal("Cutoff",1E-5);
        // ^ cutoff == 1E-10, found in args

    auto maxm = args.getInt("Maxm",5000);
       // ^ maxm == 5000, not found in args or Args::global() so default used
    

### Creating Args Objects

There are a few different ways to construct args objects. The simplest is the Args constructor,
which accepts any number of name-value pairs:

    auto args = Args("Name","some_string",
                     "Size",100,
                     "Threshold",1E-12,
                     "DoThing",false);

The above constructor is what is called when calling a function using the syntax

    someFunc(...,{"Name","a_name","Size",40});

Args objects can also be constructed from strings of the form `"Name1=value1,Name2=value2,..."`.
So for example

    auto args = Args("Name=some_string,Size=200,Threshold=1E-10");

Finally, arguments can be added to an Args object that is already defined using the add method.
Adding an argument that is already defined overwrites the previously defined value.

    auto args = Args("Name","name1");

    args.add("Threshold",1E-8);

    if(args.defined("Threshold")) println("args contains Threshold");




<br/>
<br/>
<i style="color:#aaa;font-size:10pt">Contributed by Miles Stoudenmire</i>

<br/>
[[Back to Quickstart Guides|guides]]

[[Back to Main|main]]