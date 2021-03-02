(GraphicsRequests)=
# GraphicsRequests

There are various options for varifying the correct appearance of plots, and the corresponding tests are collected within the "obj.mlt.mltutorGraphicsRequest" object. We will describe the available tests and their signature in this chapter.


## Specifying the correct figure and axis

Since multiple figures can be plotted within one Example, we have to direct tests towards the correct one. We do this by referencing the "figure_n" field of the mltutorGraphicsRequest object, and then referencing "axes_1" on that field. It is demonstrated in the following line of code, where we direct our tests toward Figure 1:
```
this.mlt.mltutorGraphicsRequest.figure_1.axes_1
```
"figure_n" is referencing towards different [figures](https://www.mathworks.com/help/matlab/ref/figure.html), while axes_1 corresponds to the [Matlab-axes object](https://www.mathworks.com/help/matlab/ref/axes.html). Typically, you will not have to call any axes other than axes_1, except if you include [subplots](https://de.mathworks.com/help/matlab/ref/subplot.html) in your figures.


## Properties of the axes_1 object

The axes_1 object itself provides different subfields that we can access, depending on the type of plot. Inthe case of a regular [plot](https://www.mathworks.com/help/matlab/ref/plot.html), these include:
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
Properties we can check here include: 'xlim','xlabel','ylabel','title','yscale'...


For an exhaustive list of properties that can be checked, see [Axes Properties](https://de.mathworks.com/help/matlab/ref/matlab.graphics.axis.axes-properties.html)


## Expanding the notion to other types of plots

At the beginning, it may be unclear which fields we can specify on which types of plots. But there is a simple strategy to find the available subfields, which we will demonstrate here. First, create a new figure, that you want to include in your Graphics Requests. We will choose to include 2 subplots, with 2 [errorbar](https://de.mathworks.com/help/matlab/ref/errorbar.html) plots each:

```
f = figure
subplot(2,1,1)
errorbar(1:10, 1:10, rand(1,10), 'rx')
hold on
errorbar(1:10, 2:11, rand(1,10), 'bx')
hold off
xlabel("x-position")
ylabel("u")
legend({"u_x", "u_y"})
title("Data aggregated by common x-position")


subplot(2,1,2)
errorbar(1:10, 10:-1:1, rand(1,10), 'rx')
hold on
errorbar(1:10, 11:-1:2, rand(1,10), 'bx')
hold off
xlabel("x-position")
ylabel("v")
legend({"v_x", "v_y"})
```

Notice that we saved a handle-reference to the figure in the variable 'f'. This allows us to inspect the 'Children' or elements of the figure, like axes, legends, etc.
These 'Children' are the subfields that we used earlier when specifying our mltutorGraphicsRequest.
Let's inspect the Children with the following line of code:

```
f.Children

  4x1 graphics array:

  Legend    (v_x, v_y)
  Axes
  Legend    (u_x, u_y)
  Axes      (Data aggregated by common x-position)
```

We find a so called [graphics array](https://de.mathworks.com/help/matlab/creating_plots/concatenating-handles-into-arrays.html), with 4 elements in it. 2 of them are legends, the other 2 axes. Like any other array in matlab, we can reference into the array with indexes. For example, the following line of code will reference the first axes-object (which is the first subplot in our figure):

```
f.Children(2)
  
  ans = 

  Axes with properties:
             XLim: [1 10]
             YLim: [0 12]
           XScale: 'linear'
           YScale: 'linear'
    GridLineStyle: '-'
         Position: [0.1300 0.1100 0.7750 0.3412]
            Units: 'normalized'
```

This is how we can find the available subfields. Matlabtutor on the other hand does not use indexes in the way we do. If we want our mltutorGraphicsRequest to target the same subplot, we would write:

```
this.mlt.mltutorGraphicsRequest.figure_1.axes_1
```

All of the properties in the code output above can be tested with the request-field, for example by writing:

```
this.mlt.mltutorGraphicsRequest.figure_1.axes_1.request = {...
'XLim', 'YScale'...
}
```


If we write 'axes_2' instead, we would find the second subplot, or the same element as we would get by by calling 'f.Children(4)'

Now we have found the four subfields we can call on the figure-object within our mltutorGraphicsRequest (axes_1, axes_2, legend_1, legend_2). But we have still not managed to find the correct field for testing if the correct dataset was plotted. Let's continue to find the next subfields, on the axes_1-object, or one layer deeper. Again, we use the Children-property, this time on one of the axes:

```
f.Children(2).Children

  ans = 

  2x1 ErrorBar array:

  ErrorBar    (v_y)
  ErrorBar    (v_x)
```

We found 2 errorbar objects belonging to the axes. These are the 2 dataseries that we plotted by using [hold](https://de.mathworks.com/help/matlab/ref/hold.html). If we had used a regular plot instead, these would be line-objects instead. Continue this process further, and inspect the second of these errorbars:

```
s.Children(2).Children(2)

  ans = 

  ErrorBar (v_x) with properties:

             Color: [1 0 0]
         LineStyle: 'none'
         LineWidth: 0.5000
            Marker: 'x'
             XData: [1 2 3 4 5 6 7 8 9 10]
             YData: [10 9 8 7 6 5 4 3 2 1]
    XNegativeDelta: [1x0 double]
    XPositiveDelta: [1x0 double]
    YNegativeDelta: [1x10 double]
    YPositiveDelta: [1x10 double]
```
All of these properties are again testable by the request-field, meaning we can stop here. If we called 'Children' once more on the ErrorBar object, we would only find an empty array, meaning that matlab did not assign any further elements to the plot under ErrorBar. 

If we would like to the test the appearance of the errorbar-plot, we simply use the following syntax, as we did before:

```
this.mlt.mltutorGraphicsRequest.figure_1.axes_1.errorbar_2.request = {...
'XData', 'YData', 'XNegativeDelta', 'XPositiveDelta',...
'YNegativeDelta', 'YPositiveDelta', 'Marker'...
}
```

This strategy will allow you to find the correct set of subfields for testing plots, even if they are not mentioned within this documentation.