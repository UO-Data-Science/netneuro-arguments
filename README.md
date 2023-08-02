# Using Arguments

As a reminder from our first [lecture](https://github.com/UO-Data-Science/NetNeuro2023/raw/main/docs/slides/Good_Data_Practices.pptx). It is good practice to avoid hard coding things that only you can use into your scripts. For example paths to files like 
```python
df=pd.read_csv("C:/Jakes/random/folder/data.csv")
```
or in R
```R
df<-read_csv("C:/Jakes/random/folder/data.csv")
```
Why? Because anyone else who wants to use your code will have to open up a file and re-write this code for it to work for them. This can be error prone, and makes it difficult understand why you got a result, because you can't trust that the file currently on your computer is the one you ran because it changes regularly in your workflow. As a side note having people change code as part of a workflow also makes version control messy because every person contributing will have a different value for that line. So what are some solutions.

## File organization
* organize code into packages and use relative paths (for more on paths see [here](https://datacarpentry.org/shell-economics/02-the-filesystem/index.html))
in this workflow everyone sets up there package like this, and puts there data in the same place `mydata.csv`. This is normally good enough if you agree to also run the script with your package directory as your working directory. (Note this isn't good enough if you want to run in other directories)
```
├── my_package
│ ├── my_analysis.R
│ ├── my_analysis.py
│ ├── data
│ │ ├── mydata.csv

```

```python
df=pd.read_csv("./data/mydata.csv")
```
or in R

```R
df<-read_csv("./data/mydata.csv")
```

 ### In some cases when everyone can use the same disks you can use a path everyone has access to
```python
df=pd.read_csv("/path/everyone/can_use.csv")
```
this is easy, but only works for people on your team that can access that data (for example you're all working Talapas) and it won't work if you want to share your code elsewhere.

## Use Command Line Arguments

Another way is to set values using command line arguments. It's important to note that this generally means you'll be running your script on the command line for example in a terminal you would run:

```R Rscript 01-my_script.R --inpufile="./my_data.csv" ``` 

or in python

```python python my_script.py --inpufile="./my_data.csv" ``` 

There are several ways to use these variables in your code, but it won't work without some changes. You'll need a package to parse the arguments you pass to your script. For example in R

```R

# This is a common R command line parser
library("optparse")

# It starts with defining the options you expect to get on the command line
# Arguments can have short tag with a dash and a letter (-f)
# Or a longer tag with two dashes and a description --input_file
#
# Command line arguments are always text so if you want a different type
# for example an integer set type='integer'

option_list = list(
  make_option(c("-f", "--input_file"), action="store", default=NA, type='character',
              help="just a variable named a"),
  make_option(c("-o", "--output_file"), action="store", default=NA, type='character',
              help="just a variable named a")
)


opt = parse_args(OptionParser(option_list=option_list))

# You can now access the values using the long name you gave them above
print(opt$input_file)
print(opt$output_file)

```

to run the above type in a terminal (assuming the code above is in a file called r_opt_test.R)
```bash
Rscript  r_opt_test.R -f 'test.csv' -o 'output.csv'

# you should see
[1] "test.csv"
[1] "output.csv"
```

### Using Python
The python code to access arguments is very similar to the R code above and same principals apply.



```python

# This is a common python command line parser
from optparse import OptionParser

# As before it starts with defining the options you expect to get on the command line
# Arguments can have short tag with a dash and a letter (-f)
# Or a longer tag with two dashes and a description --input_file
# This package also lets use set a destination or the name of the variable you'll use
# when accessing this option in your code
#
# Command line arguments are always text so if you want a different type
# for example an integer set type='integer'
# 

parser = OptionParser()
parser.add_option("-f", "--input_file", dest="filename",
                  help="write report to FILE", metavar="FILE")
parser.add_option("-o", "--output_file",
                  action="store", dest="out_filename", default='output.csv',
                  help="output_file")

(opt, args) = parser.parse_args()

print(opt.filename)
print(opt.out_filename)

```

to run the above type in a terminal (assuming the code above is in a file called r_opt_test.R)
```bash
python  py_opt_test.py -f 'test.csv' -o 'output.csv'

# you should see
[1] "test.csv"
[1] "output.csv"
```

### Tips
* Using arguments with and IDE like RStudio or Spyder can be a little tricky. When you run the script in your IDE it needs to know what arguments to run with, which isn't always an option you can set. When a script is run without arguments it defaults to the value set in the `default= value` part of the option creation. You can set this function argument to point to your own files while developing your code, and either clear them or keep them when you share it. Just remember if you're using version control not everyone can set there own default without running into conflicts. 

* Save a log with values of your arguments along with your script's results, so you can re-create what you did in the past.

## Use a Config file

Another option is to read in all your options from a file. There are a bunch of ways to do this, but the idea is simple instead of using command line arguments, save a file that contains everything you want to configure in your code. Your code will read the file and use it to set all the options. Here are a few guidelines if you want to go down this route.

1) Pick a file format for your config file common ones are CSV, YAML, or JSON.
2) Write a function that reads the file and converts the arguments into variables
   * This function should have defaults or error checking to make sure the variables are set correctly
   * It should return an object with all the values in it, since you may use this in several places in your code make it easy to reuse
3) Save a copy of the config file along with your results, so you can re-create what you ran in the past
4) Consider passing the path of your config file as a command line argument, so you can run with many different config's easily. 

Here are a few things to get you started if you want to learn more about reading and writing the above file types.
* Configr package (many file types) [\[Link\]](https://cran.r-project.org/web/packages/configr/readme/README.html)
* Config package in R (YAML) [\[Link\]](https://cran.r-project.org/web/packages/config/vignettes/introduction.html)
* Using YAML for python configuration [\[Link\]](https://betterdatascience.com/python-yaml-configuration-files/)
* Using JSON for python configuration [\[Link\]](https://betterdatascience.com/python-json-configuration-file/)

## Give it a try with one of your scripts and ask slack if you run into trouble


