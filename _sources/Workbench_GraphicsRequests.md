# Verifying Plots

There are various options for varifying the correct appearance of plots, and the corresponding tests are collected within the "obj.mlt.mltutorGraphicsRequest" object. We will describe the available tests and their signature in this chapter.


## Specifying the correct figure and axis

Since multiple figures can be plotted within one Example, we have to direct tests towards the correct one. We do this by referencing the "figure_n" field of the mltutorGraphicsRequest object, and then referencing "axes_1" on that field. It is demonstrated in the following line of code, where we direct our tests toward Figure 1:
```
this.mlt.mltutorGraphicsRequest.figure_1.axes_1
```
"figure_n" is referencing towards different [figures](https://www.mathworks.com/help/matlab/ref/figure.html), while axes_1 corresponds to the [Matlab-axes object](https://www.mathworks.com/help/matlab/ref/axes.html). Typically, you will not have to call any axes other than axes_1.


## Properties of the axes_1 object

The axes_1 object itself provides different subfields that we can access. These include:
- {ref}`line_n`
- {ref}`title_1`
- [xlabel](xlabel)
- [ylabel](xlabel)
- {ref}`request`

Whenever we want to include one of these properties in our tests, we have to assign an cell array to the "request"-subfield of the corresponding property (except when calling "request" directly on the "axes_1"-object).

(line_n)=
### axes_1.line_n
This field is useful when we want to verify that the correct data was plotted (xdata and ydata) as well as the correct linespec was used. If we want to include certain tests, we assign a cell-array to its "request"-subfield, containing character-arrays labeling the property we want to check. The following code will perform assertions regarding x- and y-data, as well as the color and marker:

```
this.mlt.mltutorGraphicsRequest.figure_1.axes_1.line_1.request = {'xdata','ydata','color', 'marker'};
```
For an exhaustive list of properties that can be checked see [Line Properties](https://de.mathworks.com/help/matlab/ref/matlab.graphics.chart.primitive.line-properties.html)

(title_1)=
### axes_1.title_1
This field is useful for verifying the correct title. In order to check the title of a plot, assign a cell-array to its "request"-subfield, containing a second, nested cell-array. The inner cell array should contain 2 elements: the literal character array 'string', followed by a character array with a [Regular Expression](https://www.princeton.edu/~mlovett/reference/Regular-Expressions.pdf), which the desired title has to match. The following code will check that a title contains one of the words "Hyperbel", "HYPERBEL" or "hyperbel", and only whitespace before or after:

```
this.mlt.mltutorGraphicsRequest.figure_2.axes_1.title_1.request = {{'string','^\s*(Hyperbel|HYPERBEL|hyperbel)\s*$'}}
```
There are no limits to the complexity of the Regular Expression, naturally.


(xlabel)=
### axes_1.xlabel and ylabel
Similar to [](title_1), the "request" fields of these "xlabel" and "ylabel" will verify the correct labels on x- or y-axis. For example, the following code will ensure that the x-label is set exactly to the string "x":
```
this.mlt.mltutorGraphicsRequest.figure_1.axes_1.xlabel_1.request = {{'string','^x$'}};
```

(request)=
### axes_1.request
By referencing the "request"-field directly on the "axes_1" object, we access additional axes-properties, for example axis-limits. The "axes_1.request" field allows us to specify multiple tests at once.  For example, the following code asserts correct y-limits, and also shortcuts the title-evaluation (meaning we don't have to reference the "title" field like described in [](title_1)):
```
this.mlt.mltutorGraphicsRequest.figure_1.axes_1.request = {'ylim','title'};
```
The following code will check if the [axis-mode](http://www.mathworks.com/help/matlab/ref/axis.html) was set appropriately:
```
this.mlt.mltutorGraphicsRequest.figure_1.axes_1.request = {'xlim','ylim','PlotBoxAspectRatio','PlotBoxAspectRatioMode'}
```
For an exhaustive list of properties that can be checked, see [Axes Properties](https://de.mathworks.com/help/matlab/ref/matlab.graphics.axis.axes-properties.html)




