(AvailableTests)=
# Available Tests

MatlabTutor provides various tests that allow us to test different aspects of student solutions. For example, we can check how often a certain Matlab command was used and specify its minimum and maximum number of occurence, check if certain variables in the solution have the correct value, verify how plots look, and so on. Arguably, checking variables and plots are the most frequent types of tests you will use. This chapter will give an overview of the workings and signatures of available tests. Each and every check has to be defined within the "Example...".m file, that is part of every working Tutor Example. If you are unsure about how and where to add certain tests within this file, check the according section (Anatomy of Example.m, add link here)


## mltutorCountCommand
```
[count_test,count,stat] = mltutorCountCommand(filename,commands,max_count,min_count);
```

Checks if the number of command-calls is within min_count and max_count (including min and max limits). Is usually put inside the try-catch block of a test, along with the script/function call.

filename: specifies the matlab-file that this test is to be run on. Has to be a character-vector, ending with ".m"

commands: a cell array containing the commands in form of character-vectors. This can likely include any command known to Matlab, for example 'tan', 'sqrt', 'plot', 'for', 'try' etc.

max_count: Row-vector with the same number of elements as items in commands. First element in the vector corresponds to first command in commands-array, and so on. The test will verify that commands(i) within the specified matlab-file are is present at most max_count(i). If no upper limit of occurence should be specified, include nan in the corresponding position.

min_count: Identical to max_count, but specifying the minimum number of occurence. If no lower limit of occurence should be specified, inclue nan in the corresponding position.
```{note}
This test does not need to be referenced as a method of the Example-Class. In short, write mltutorCountCommand(), not obj.mltutorCountCommand()
```

## mltutorReadFile
```
f_lines = mltutorReadFile(filename);
```
Reads the specified file and returns its content as a cell-array containing character-arrays. Each character array is representing a line of the file. This is helpful if the presence of certain code-constructs should be tested, for example in combination with [Regular Expressions](https://www.princeton.edu/~mlovett/reference/Regular-Expressions.pdf). 
Is usually put inside the try-catch block of a test, along with the script/function call.

The following code will read through the matlab-file "mathc.m" and proceed to match the declaration of the number 1.5\*10⁶ in scientific notation in every line of the file:

```
f_lines = mltutorReadFile('mathc.m');
f_find_1 = regexp(f_lines,'1.5[eEdD]\+*0*6');
```
The following code will return true only if the scientific notation was found somewhere within the file:
```
f_lines = mltutorReadFile('mathc.m');
f_find_2 = any(~cellfun(@isempty,regexp(f_lines,'1.5[eEdD]\+*0*6')));
```
```{note}
This test does not need to be referenced as a method of the Example-Class. In short, write mltutorCountCommand(), not obj.mltutorCountCommand()
```










