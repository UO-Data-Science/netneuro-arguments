# Using Arguments

As a reminder from our first [lecture](https://github.com/UO-Data-Science/NetNeuro2023/raw/main/docs/slides/Good_Data_Practices.pptx). It is good practice to avoid hard coding things that only use can use into your scripts. For example paths to files like 
```python
df=pd.read_csv("C:/Jakes/random/folder/data.csv")
```
or in R
```
df<-read_csv("C:/Jakes/random/folder/data.csv")
```
Why? Because anyone else who want to use your code will have to open up a file and re-write this code to work for them. This can be error prone, particularly, when you're trying to understand when why you got a result because you can't trust that the file that currently in your script is the one the code was run on because it changes regularly in your worflow. As a side note having people change code as part of a workflow also makes version control messy because every person contributing will have a different value for that line. So what are some solutions.

## File organization
* orgainize code into packages and use relative paths (for more on paths see [here](https://datacarpentry.org/shell-economics/02-the-filesystem/index.html))
in this workflow everyone sets up there package like this, and puts there data in the same place 'mydata.csv'. This is normally good enough if you agree to also run the script with your package directory as your working directory. (Note this isn't good enough if you want to run in other directories)
```
├── my_package
├── my_analysis.R
├── my_analysis.py

│   ├── data
│   │   ├── mydata.csv

```

```python
df=pd.read_csv("./data/mydata.csv")
```
or in R
```

df<-read_csv("./data/mydata.csv")
```

 * In some cases when everyone can use the same disks you can use a path everyone has access to
```python
df=pd.read_csv("/path/everyone/can_use.csv")
```
this is easy, but only works for people on your team that can access that data and won't work if you want to share your code elsewhere

## Use Command Line Arguments

## Use a Config file

## Use Enviromental Variables
