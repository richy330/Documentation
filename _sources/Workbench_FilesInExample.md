<!-- #region -->
# Files within Examples

A Tutor Example consists of several files, from reference-solutions, to problem-descriptions, to special metadata files. In this section, we will give a quick overview over what purpose individual files that you may find serve.

## convert.json
Created when running the admin-function `mltutorCreateConvertFile` (see [](convert_file_creation)).
Allows to make thorough changes to the existing Example before creating a duplicate. Only used when creating new Examples.

## example.json
Set up automatically, contains metainformation and additional information found within Example*.m. Should usually not be altered by the user.

## Example*.m

This file provides additional information to students, including used functions and links, and contains all the validation tests that students have to pass in order to solve examples. The structure of this file is covered in great depth in [](Example-file)


## meta.json
Contains additional metadata and filenames. If certain filenames in the example are altered, it may be necessary to include the changes to this file, otherwise the Example may break.

```
{
	"title": "Geometrische Eigenschaften mehrerer Partikel 2",
	"author": "Richard Amering",
	"tags": [],
	"main": "ExampleGesamtvolumen_part2.m",
	"type": "matlab",
	"descriptionMain": "GesamtvolumenMehrererPartikel_part2.mmd",
	"descriptionType": "markdown",
	"descriptionOutput": "html",
	"descriptionDirectory": "",
    "dependencies" : ["vt2.2021.GesamtvolumenMehrererPartikel_part1"]
}
```

Important fields are `title, author, main` and `descriptionMain`, with the last two referencing the Example*.m file and the problem-description markdown file.

### Reusing earlier Examples
From a didactic standpoint, it may be very interesting to build examples that rely on earlier student solutions. That way, greater programms can be built from many smaller student solutions, and students learn to write reusable programs. If you want to include other Examples as dependencies for the current Example, include the path to the other Example in the `dependencies`-array, like shown above. Here, part 2 of the programm relies on the solutions found within part 1. By including the path to other Examples, all the files defined there can also be used in the current example. This includes Matlab scripts and function. This technique may be especially helpful if Examples start to get too big for a single evaluation. 






<!-- #endregion -->
