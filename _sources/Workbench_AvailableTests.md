(AvailableTests)=
# Available Tests

MatlabTutor provides various tests that allow us to test different aspects of student solutions. For example, we can check how often a certain Matlab command was used and specify its minimum and maximum number of occurence, check if certain variables in the solution have the correct value, verify how plots look, and so on. Arguably, checking variables and plots are the most frequent types of tests you will use. This chapter will give an overview of the workings and signatures of available tests. Each and every check has to be defined within a test-method in the ```Example....m``` file, that is part of every working Tutor Example. If you are unsure about how and where to add certain tests within this file, check the according section [](Example-file).

## Variable Checks




### this.mlt.mltutorVariableCheck
Arguably one of the most important fields in any test-method. `mltutorVariableCheck` can be assigned with a struct containing any variable that should be tested against your reference solution. This includes any variable used in the students solution script, and variables defined within the encapsulating test-method. Several of those variables can be listed in the cell array. Certain tests will yield a return value, that has to be included within this field to work properly.
```
% these variables will be checked in the test
this.mlt.mltutorVariableCheck = {student_variable_1, student_variable_2, mltutorCountCommandReturnValues, ...};
```

#### Caveats
While many different types of variables are supported by `mltutorVariableCheck`, there are certain types that should not be included, since they are not 'comparable'. This includes:

- anonymous functions
- custom classes
- errors

Tests of anonymous functions are described in [](anonymous_function_test).

### this.mlt.mltutorInternalVariableCheck
```
% matlab_tutor variables checked in the test
this.mlt.mltutorInternalVariableCheck = {};
```
Variables included within this field are not specific to the scripts given to the students, but instead include MatlabTutor-internal variables.


Some test are automatically done if you add a specific predefined variable to the the cell array. Several of those variables can be listed in the cell array.

#### semicolon (;)

This test automatically tests the proper use of semicolons in the required solutions:

`this.mlt.mltutorInternalVariableCheck = {'mltutorSemicolon‘};`

This test does not require any further settings.

#### Test to prohibit use of the command `clear`

`this.mlt.mltutorInternalVariableCheck = {'mltutorClearCount‘};`

This test does not require any further settings.

#### Help text

To check the help text of a solution use 

`this.mlt.mltutorInternalVariableCheck = {'mltutorHelpRegexpCorrect’};`

This has to be accompanied with a proper specification of the variable `this.mlt.mltutorHelpRegexp`

```
this.mlt.mltutorHelpRegexp = {'^[ \t]*[Mm]atlab’, 'Program’};
```

This would mean that the first two lines of the help text are checked:

- Line 1: Could start with zero or more blanks or tabs followed by either Matlab or matlab 
- Line 2: Must contain the word Program

One can use all the syntax from regular expressions in Matlab.

An extensive example can be found in `Einfaches Plotprogramm` 

#### Graphics

To check graphics requires the specification

```
this.mlt.mltutorInternalVariableCheck = {'mltutorGraphicsResults'};
```

which needs to be accompanied, e.g., by 

```
this.mlt.mltutorGraphicsRequest.figure_1.axes_1.line_1.request = ...
{'xdata','ydata','color','linestyle','marker'};
this.mlt.mltutorGraphicsRequest.figure_1.axes_1.line_2.request = ...
{'xdata','ydata','color','linestyle','marker'};
```




### this.mlt.mltutorVariableDisplay
If you want to provide further information to students when validating their scripts, it may be helpful to include variables within this field. Included variables will not be validated, but displayed in the test-results.
```
% these variables will only be displayed in the test (no check)
this.mlt.mltutorVariableDisplay = {};
```

### this.mlt.mltutorInternalVariableDisplay

Similar to `mltutorVariableDisplay`, but used to display internal variables. Usually left out.
```
% matlab_tutor variables 
this.mlt.mltutorInternalVariableDisplay = {};
```


## Additional Tests
The tests described in the following sections have certain return variables. It is not sufficient to only write a certain test, instead, the return variables have to be included in the `this.mlt.mltutorVariableCheck = {};` field in the beginning of the encapsulating test - method. For example, if you use [](CountCommand), you have to include its return values, like in the following:
```
this.mlt.mltutorVariableCheck = {count_test, count, stat};
%
%
%
[count_test, count, stat] = mltutorCountCommand(filename, commands, max_count, min_count);
```

The same pattern holds for other tests as well.

(CountCommand)=
### mltutorCountCommand
```
[count_test, count, stat] = mltutorCountCommand(filename, commands, max_count, min_count);
```

Checks if the number of command-calls is within min_count and max_count (including min and max limits). Is usually put inside the try-catch block of a test, along with the script/function call. Include the return values `count_test, count, stat` within `mltutorVariableCheck`!

**filename**: specifies the matlab-file that this test is to be run on. Has to be a character-vector, ending with ".m"

**commands**: a cell array containing the commands in form of character-vectors. This can likely include any command known to Matlab, for example 'tan', 'sqrt', 'plot', 'for', 'try' etc.

**max_count**: Row-vector with the same number of elements as items in commands. First element in the vector corresponds to first command in commands-array, and so on. The test will verify that commands(i) within the specified matlab-file are is present at most max_count(i). If no upper limit of occurence should be specified, include nan in the corresponding position.

**min_count**: Identical to max_count, but specifying the minimum number of occurence. If no lower limit of occurence should be specified, inclue nan in the corresponding position.

Example:
```
[count_test, count, stat] = mltutorCountCommand(f_name, ...
    {'if', 'elseif', 'switch', 'for', 'while', 'mean', 'sum', 'cumsum'}, ...
    [ 2,    2,        0,        2,     0,       0,      0,     0      ], ...
    [ 2,    2,        nan,      2,     nan,     nan,    nan,   nan    ]);
```
with the meaning that `if`, `elseif`, and `for` have to be used exactly twice (the maximum value
is the same as the minimum value), and the others are not allowed to be used at all.

When `mltutorCountCommand` is used, there is also a test message created which can be viewed in the the test results editor.


```{note}
This test does not need to be referenced as a method of the Example-Class. In short, write mltutorCountCommand(), not obj.mltutorCountCommand()
```

### mltutorReadFile
```
f_lines = mltutorReadFile(filename);
```
Reads the specified file and returns its content as a cell-array containing character-arrays. Each character array is representing a line of the file. This is helpful if the presence of certain code-constructs should be tested, for example in combination with [Regular Expressions](https://www.princeton.edu/~mlovett/reference/Regular-Expressions.pdf). 
Is usually put inside the try-catch block of a test, along with the script/function call.

The following code will read through the matlab-file "mathc.m" and proceed to match the declaration of the number 1.5\*10⁶ in scientific notation in every line of the file:

```
f_lines = mltutorReadFile('mathc.m');
f_find_1 = regexp(f_lines, '1.5[eEdD]\+*0*6');
```
The following code will return true only if the scientific notation was found somewhere within the file:
```
f_lines = mltutorReadFile('mathc.m');
f_find_2 = any(~cellfun(@isempty, regexp(f_lines, '1.5[eEdD]\+*0*6')));
```
```{note}
This test does not need to be referenced as a method of the Example-Class. In short, write mltutorCountCommand(), not obj.mltutorCountCommand()
```


### Use of the function `input`

If your Example should include the `input` function, you have to specify the expected inputs with the following code:
```
this.mlt.mltutorInputAnswers = {'0.1','0.2'};
```
In this example there would be two answers ('0.1' and '0.2') for two distinct calls of `input`.


You can then test if the correct questions (=input-messages) were asked for by specifying, e.g.,

```
this.mlt.mltutorInputRegexp = { ...
{'[B|b]itte\s+geben\s+[S|s]ie\s+x\s+ein:'}, ...
{'[B|b]itte\s+geben\s+[S|s]ie\s+y\s+ein:'} ...
};
```

Don't forget to specify `this.mlt.mltutorInternalVariableCheck = {'mltutorInputRegexpAllCorrect'};` in this case!
This example is from `Vergleich von Formeln` from `Programmieren in der Physik`.

### Checking output from `disp`

These tests require `this.mlt.mltutorInternalVariableCheck = {'mltutorDispRegexpCorrect'};` to be set. 

In the following example there are four outputs with disp required from the student

```
this.mlt.mltutorDispRegexp = {...
['[Aa]ussen:.*',num2str(ua(N),14)],...
['[Ii]nnen:.*',num2str(ui(N),14)],...
['[Mm]ittel:.*',num2str(m(N),14)],...
['[Pp]i:.*',num2str(pi,14)]
};
```

Again, regexp is used to specify the required output, e.g., the first line has to start with
`Aussen:` or `aussen:`, then followed by some characters (zero or more times), and then followed
by the Nth value in the variable `ua` with given accuracy.




(anonymous_function_test)=
### Testing of an anonymous function

If the student, is required to write an anonymous function, like `nc = @(a,x) a * sin(x).^2` the function handle `func` can not be used in the variable list for automatic tests.

You have to execute it within the test in the `try-catch`-environment, as demonstrated in the following code:

```
try
    a_mlt = 2;
    x_mlt = linspace(-pi, pi, 100);
    func_test = func(a_mlt, x_mlt);
catch test_try_catch
    func_test = nan;
    this.mlt.mltutorTestMessage.('message_1') = 'func could not be executed!';
end
```

Now you can include `func_test` in the list of variables to be tested. Here also a test message is generated, if the function could not be executed at all.






## Accuracy of variable tests

If you would like to specify the relative acceptable error of variable tests you can specify

```
this.mlt.mltutorDoubleAccuracy = 1.E-6;
this.mlt.mltutorSingleAccuracy = 1.E-4;
```

The default values are

```
mltutorDoubleAccuracy = 1.E-12
mltutorSingleAccuracy = 1.E-6
```

This value is then used for all variables within this test.










