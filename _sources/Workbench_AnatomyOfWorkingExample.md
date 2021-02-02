# Anatomy of an Example.m-file


In order to be able to create our own Examples, it is important to understand the underlying files necessary for a working Example. Along with the solution-code provided as a matlab-script and the problem description in Markdown, the Example.m file is the most important one for creating a working Tutor Example. We will discuss the structure of Example.m files in this chapter. It is assumed that you followed the chapter [](example_implementation), and already have a working starting point for the implementation of further tests and additional data. If you don't have an Example including an Example.m-file assigned to an Excercise, or the Validation of an Example fails, it is best to start fresh, before editting the Example.m-file. Altering the Example.m file is very sensitive to breaks, and debugging may be difficult. It is best practice to start with a working Example (with successful Validation), and running the validation process whenever adding new tests, or refactoring the code in any other sense. This way, it should be easy to revert breaking changes and quickly find faulty code-edits.


## Purpose
The Example.m file is responsible for setting up required tests correctly, provides information about useful links and used functions, and defines which files are used within the Example.

## Naming

In order for the Example to work correctly, a naming structure has to be followed: Each Example.m filename must start with "Example", followed by additional characters of your choice. Inside the file, a class with the **same name** has to be defined. 
A valid Example.m file-name would be: "ExampleMathConstants.m", with a class named "ExampleMathConstants".
The class will contain certain properties and methods, which again have to follow a naming structure. If this structure is neglected, the Tutor Environment may ignore your Example setup, or not Validate your Example at all. If you follow the following code-examples, everything should work out fine.

## File Content
The Example.m file contains a [Matlab-class](https://de.mathworks.com/help/matlab/object-oriented-programming.html), which defines tests and additional data. Meta-information like additional files and links are provided as class-properties, while tests are defined as methods, the so called matlabTutorTests, which again have to follow a naming-structure.

### Class-definition
As usual, the class defined within the Example.m file should be consistent with the file-name. If we name our class "ExampleMathConstants", it should be placed in a file called "ExampleMathConstants.m". In order for the tests to be working, we have to inherit from the class "MatlabTutorExample". A correct class-definition would look like the following:
```
classdef ExampleMathConstants < MatlabTutorExample
    % Mathematische Konstanten, Format
    %
```

### Properties
If you are unsure about what properties are, or how they work in Matlab, you may want to take a look at these resources:<br>
[Properties](https://de.mathworks.com/help/matlab/properties-storing-data-and-state.html) <br>
[Ways to Use Properties](https://de.mathworks.com/help/matlab/matlab_oop/how-to-use-properties.html) <br>
[Property Definition](https://de.mathworks.com/help/matlab/matlab_oop/specifying-properties.html)

Right after our class-definition, we define a properties-block. It will include additional information for students, as well as meta-information about the Example-file structure. If you followed our Tutorial on [implementing new Tutor Examples](example_implementation), the properties-block of your Example.m file will look like the following:  

(propertiesField)=
```
classdef ExampleMathConstants < MatlabTutorExample
    % Mathematische Konstanten, Format
    %
    properties
        usedFunctions = {'exp','sin','pi','format','arithmeticoperators','doc','i'}
        requiredSolutions = {'mathc.m'}
        requiredData = {}
        extraData = {'kerrtab_i.dat'}
        extraDataReadOnly = {}
        extraPrograms = {}
        extraProgramsReadOnly = {}
        testPrograms = {}
        programTags = {}
        links = { ...
            {'appsoftkapitel://2','Skriptum Einfuehrungskapitel'}, ...
            {'matlabroot://','Matlab Hilfe'}, ...
            {'mltutorhint://Strichpunkt','Hints zu Strichpunkt'}, ...
            {'mltutorhint://Komplexe_Zahlen','Hints zu komplexen Zahlen'}, ...
            {'matlabdocu://elementary-math','Elementare Mathematik'}, ...
            {'matlabpublish://simplestart1','Einfache Einfuerung in Matlab'} ...
        }        
    end
```


Let's break the individual fields down:
(usedFunctions)=
#### usedFunctions
When working on Examples, students can be provided with additional resources. One of these are the Used Functions. If Matlab-functions or statements are declared within this field (different entries provided as char-arrays), MatlabTutor will include the according references in the Used Functions Tab, visible to students working on the example. The references provided by MatlabTutor will automatically be linked towards the correct Matlab Help page. 
For Example, the above declaration of used functions will provide the following links within the Used Functions Tab:


![UsedFunctionsTab](tutor_screenshots/UsedFunctionsTab.png)

Providing Used Functions is optional, if no help should be provided (for example, in an exam), set the usedFunctions field to an empty cell array.

(requiredSolutions)=
#### requiredSolutions
This field refers to the matlab files that students will write their code in. The specified files will be checked against your reference solutions when Validating the Example. It is possible to include multiple files within this field. For example, the following code will include 3 scripts:
```
requiredSolutions = {'sgauss.m','secansh.m','pskript.m'}
```
Files specified in this field will be visible to the students when opening the Example, and are expected to be worked on by the students. For the students, the presented files will be empty initially. Upon Validation, the student solutions will be compared to your reference solutions, depending on the tests specified. 
Provide matlab file-names character arrays.

#### requiredData = {}
#### extraData
Similar to [required Solutions](requiredSolutions), the files listed within this field will be visible to the students upon opening the Example. The difference is that these files will not be empty to the students, but appear unchanged from how they are present within the Example files. This field is useful if you want to include a dataseries that the student should utilize for solving a task. Files specified within this field are not limitted to matlab-files, you can also include csv, txt, and so on. Students are not supposed to change the files specified as extra Data. If no extra Data is necessary, set this field to an empty cell-array.

#### extraDataReadOnly = {}
#### extraPrograms = {}
siehe 3.1 'if_tests'
#### extraProgramsReadOnly = {}
#### testPrograms = {}
siehe 2.6 'plot_res.m'
#### programTags = {}

#### links
Similar to [Used Functions](usedFunctions), the students can be provided with additional links under the Links tab. For example, the above mentioned [code](propertiesField) will include the following links:

![LinksTab](tutor_screenshots/LinksTab.png)

When adding links to this field, it is necessary to use Link-Abreviations, as can be found under the Administration-perspective. If regular html-links are found within this field, Validation may break with the error-code "Bad Subscript".


### Methods
If you are unsure about how methods work in Matlab, you may want to take a look at these resources:<br>
[Methods](https://de.mathworks.com/help/matlab/methods-defining-operations.html)<br>
[Methods in Class Design](https://de.mathworks.com/help/matlab/matlab_oop/how-to-use-methods.html)<br>
[Define Class Methods and Functions](https://de.mathworks.com/help/matlab/matlab_oop/specifying-methods-and-functions.html)

The methods-section of the Example-class will contain all the tests that we define.
The general structure is the following:

```
methods

    function matlabTutorTest_test1(this)
        % Testdescription, will be visible in MatlabTutor when inspecting test-results
        % @name Test all variables

        % initialize test
        this.init();


        % these variables will be checked in the test
        this.mlt.mltutorVariableCheck = {};
        % matlab_tutor variables checked in the test
        this.mlt.mltutorInternalVariableCheck = {};
        % these variables will only be displayed in the test (no check)
        this.mlt.mltutorVariableDisplay = {};
        % matlab_tutor variables 
        this.mlt.mltutorInternalVariableDisplay = {};
        
        %% GraphicsRequests and settings before tests
        
        
        
        %% running the script and tests
        try
            mathc
            [count_test,count,c_stat] = mltutorCountCommand('mathc.m',{'exp'},[2],[2]);
        catch mltutor_error
            this.mlt.mltutorError = mltutor_error;
        end
        
        
        %% shutting down the test
        this.deinit();
    end
end
```
The methods-block can contain one or more matlabTutorTest-functions. You are not limitted to a single test-method, and you can also include multiple different tests within a single method (e.g. GraphicsRequests, CommandCounts, etc). In order for the tests to work properly, a naming structure has to be followed: Each method within the method-block needs a name starting with "matlabTutorTest", followed by characters of your choice. Deviating from this naming structure may prevent your tests from working. A good practice is to name consecutive tests "matlabTutorTest_test1", "matlabTutorTest_test2" and so on. In most cases, it should be sufficient to work with a single test-method, though.

#### this.init()
The first line of code within our Test-method should be calling the init()-method on our object (remember that we inherited from "MatlabTutorExample").

#### this.mlt.mltutorVariableCheck
After that, we can start to specify variables that we want to be validated, when an Example is tested. This includes the following 4 fields:
```
this.mlt.mltutorVariableCheck = {};
this.mlt.mltutorInternalVariableCheck = {};
this.mlt.mltutorVariableDisplay = {};
this.mlt.mltutorInternalVariableDisplay = {};
```
Further Information about how they work can be found within the [](AvailableTests)-section

#### Settings before tests

#### GraphicsRequests

#### Try-catch block

#### Deinitialization of the test
