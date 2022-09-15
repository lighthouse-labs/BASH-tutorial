# BASH-tutorial

In this tutorial, we will get familiar with basic data operations that can be done through `BASH` programming directly in the terminal.

We can leverage the power of BASH mainly with huge data. Imagine that you have files that don't fit into the memory of your PC. You want to get some basic statistics or information about the dataset without loading it anywhere or copying it from your computer. This is where you can rely on `BASH` and the terminal to help you.

### Download the Data

> #### Instruction
> Download the data from [**here**](https://drive.google.com/file/d/1u1h79Ljz-3_gide11AKIFDHvijuPKNO9/view?usp=sharing). It isn't a huge dataset but it will help us see the power of `BASH`.

Now, we should find the file `people_bash_tutorial.data` in our computer.

> #### Note
> All commands in this activity can be run directly in Terminal in Linux and Mac. If you are using Windows set up **Bash on Windows** using [this guide](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/).


<!-- -->

> #### Instruction
> Go to the directory where your data are stored using the `cd` command.

### Count

`wc`: Returns three outputs, the number of rows (lines), words and characters in a document, which is important for huge documents.

`wc -l`: Returns the number of rows in a document.

`wc -w`: Returns the number of words in a document.

> #### Instruction
> Find out how many rows does our data have:
>
> ```
> wc -l people_bash_tutorial.data
> ```

`ls`: Shows all files in a directory.

`ls -l`: Shows all files in a directory with their respective sizes and access rights.

If we combine the `wc -l` and `ls -l` commands using `|`, we can also count the number of files in the file system. `|` means that the output of a command is used as the input of another command.

> #### Instruction
> How many files are in your current directory?
>
> ```
> ls -l | wc -l
> ```


### Explore

Let's go back to our people\_bash\_tutorial.data and check the first couple of rows:

```
head -n 2 people_bash_tutorial.data
```

With this approach, we can easily find out what delimiter is used in the files or the exact structure of the file. We can also see that there is no header (column names) in the file. We can add it directly using the command line.

`uniq`: Allows us to find the number of duplicate rows in the file.

`uniq -c`: Adds the repetition count to each line.

`uniq -d`: Outputs duplicate lines.

`uniq -u`: Outputs unique lines.

However, `uniq` is not a very smart command. Repeated lines will not be detected if they are not adjacent. Which means that we first need to sort the file. For this, we can use the command `sort`.

> #### Instruction
> How many duplicate lines are in the file people_bash_tutorial.data?
>
>```
> sort people_bash_tutorial.data | uniq -d | wc -l
>```


### Merge

`cat`: Prints a file's content to the standard output (your terminal window). It can also be used to concatenate a series of files:

```
cat file_1.csv file_2.csv
```
The command above will add **file_2** behind **file_1** and print it in the terminal window.

`>`: Points the output from the terminal window into the file we specify:

```
cat file_1.csv file_2.csv > target_file.csv
```

> #### Instruction
> Add a header (column names) into our table `people_bash_tutorial.data` and store it as `people_bash_tutorial.csv`

1. Let's first create the header:<br> 
```
echo "age,workclass,fnlwgt,education,education-num,>marital-status,occupation,relationship,race,sex,>capital-gain,capital-loss,native-country,class" > header.csv
```
2. Now, we can combine it with the `people_bash_tutorial.data`:<br> 
```
cat header.csv people_bash_tutorial.data
``` 
3. Then, we can point the output to the file and not to the terminal window:<br> 
```
cat header.csv people_bash_tutorial.data > people_bash_tutorial.csv
``` 
4. Finally, let's check the results:<br> 
```
head -n 2 people_bash_tutorial.csv
```


### Edit

`grep`: Searches for a specific value within a file.

Our dataset uses `?` for missing values. We can search for those and count them using command below.

```
grep ", ?," people_bash_tutorial.csv | wc -l
```

They should be replaced by a better-suited value. We can use command called `sed` for that.

`sed`: Allows us to modify a table, remove some of the rows or replace specific values:

```
sed "s/<string to replace>/<string to replace it with>/g" <source_file> > <target_file>
```


> #### Instruction
> Use the command below to replace the `?` used for missing values with a blank space:
>
>```
> sed "s/, ?,/,,/g" people_bash_tutorial.csv >  people_bash_tutorial_no_missing.csv
>```

<!-- -->

> #### Warning
> Don't forget to also use delimiters to avoid replacing legit question marks that could be present elsewhere in the dataset.

Furthermore, `sed` is a very potent command and can also be used instead of `head`, or to delete some rows from a dataset. The following command will remove the first line from a file (replace `<file_name>` with the name of your file):

```
sed -i '' 1d <file_name>
```

If you are using linux or windows, you might need to put `1d` into quotes: `"1d"`.


Now, we can move to `cut` command which can be used to do operations on specific columns. It has two main parameters `-d` and `-f`.

`cut -d`: Specifies the delimiter in a CSV file.

`cut -f`: Specifies the number of the column, which we want to work on.

> #### Instruction
> Follow the steps below to count the number of unique values in the workclass column. The workclass column is the second column of the dataset.
> 
> First, use the command below to check whether we have the right column:
> 
>```
> cut -d "," -f 2 people_bash_tutorial.csv | head -3
>```
> 
> Now, we can count the unique values:
> 
>```
> cut -d "," -f 2 people_bash_tutorial.csv | sort | uniq -c| wc -l
>```


### Subset

Suppose you're dealing with over 30 million rows. If the data are sitting in the cloud you don't want to bring the whole file to your machine and sample it there. You can do this easily with the combination of `head` and `tail` commands:

```
head -n 120 people_bash_tutorial.csv | tail -n 20 > people_bash_tutorial_sample.csv
```

The commands look at 120 rows and then take the last 20 of them. We will end up with rows 101 - 120. We can then add column names back to the file:

```
cat header.csv people_bash_tutorial_sample.csv > people_bash_tutorial_sample_with_header.csv
```
