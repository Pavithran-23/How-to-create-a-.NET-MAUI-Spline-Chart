# How-to-create-a-.NET-MAUI-Spline-Chart
The SplineSeries resembles line series, but instead of connecting the data points with line segments, the data points are connected by smooth bezier curves.

![.NET MAUI Spline Chart](https://user-images.githubusercontent.com/92786382/192692779-494315cf-efd0-4830-ab83-7c5bce64ad6f.png)

## Register the handler.
Syncfusion.Maui.Core nuget is a dependent package for all Syncfusion controls of .NET MAUI. In the MauiProgram.cs file, register the handler for Syncfusion core. For more details refer this link.

## Initialize Chart
Import the SfCartesianChart namespace as shown below.


**[XAML]**

```
xmlns:chart="clr-namespace:Syncfusion.Maui.Charts;assembly=Syncfusion.Maui.Charts"
```

**[C#]**

```
using Syncfusion.Maui.Charts;
```
Initialize an empty chart with XAxes and YAxes as shown in the following code sample:

**[XAML]**

```
<chart:SfCartesianChart>

    <chart:SfCartesianChart.XAxes >
        <chart:CategoryAxis/>
    </chart:SfCartesianChart.XAxes>

    <chart:SfCartesianChart.YAxes>
        <chart:NumericalAxis/>
    </chart:SfCartesianChart.YAxes>

</chart:SfCartesianChart>

```
**[C#]**

```
SfCartesianChart chart = new SfCartesianChart();

//Initializing Primary Axis
CategoryAxis primaryAxis = new CategoryAxis();

chart.XAxes.Add(primaryAxis);

//Initializing Secondary Axis
NumericalAxis secondaryAxis = new NumericalAxis();

chart.YAxes.Add(secondaryAxis);

this.Content = chart;

```

## Initialize view model
Now, define a simple data model that represents a data point for .NET MAUI Spline Chart.

```
public class Model
{
    public string Day { get; set; }

    public double Degree { get; set; }

    public Model(string Day , double degree)
    {
        Day = Day;
        Degree = degree;
    }
}
```

Create a view model class and initialize a list of objects as shown below,

```
public class ViewModel
{
    public List<Model> Data { get; set; }

    public ViewModel()
    {
        Data = new ObservableCollection<Model>()
        {
          new Model ("Sun",80),            
          new Model ("Mon",60 ),            
          new Model ("Tue",80),            
          new Model ("Wed",20),            
          new Model ("Thu",70),            
          new Model ("Fri",70),            
          new Model ("Sat",70)
        };
    }
}
```

Set the ViewModel instance as the BindingContext of chart; this is done to bind properties of ViewModel to SfCartesianChart.

> Note: Add the namespace of ViewModel class in your XAML page if you prefer to set BindingContext in XAML.

**[XAML]**

```
xmlns:viewModel ="clr-namespace:Syncfusion_Dot_NET_MAUI_Spline_Charts"
. . .
<chart:SfCartesianChart>

    <chart:SfCartesianChart.BindingContext>
        <viewModel:ViewModel/>
    </chart:SfCartesianChart.BindingContext>

</chart:SfCartesianChart>
```
**[C#]**

```
SfCartesianChart chart = new SfCartesianChart();
chart.BindingContext = new ViewModel();
```

## How to populate data in .NET MAUI Spline Charts
As we are going to visualize the comparison of weather reports in the data model, add SplineSeries to SfCartesianChart.Series property, and then bind the Data property of the above ViewModel to the SplineSeries.ItemsSource property as shown below.

> Note:You need to set XBindingPath and YBindingPath properties, so that series would fetch values from the respective properties in the data model to plot the series.

**[XAML]**

```
  <chart:SfCartesianChart>
    <chart:SfCartesianChart.BindingContext>
        <viewModel:ViewModel/>
    </chart:SfCartesianChart.BindingContext>
. . .
    <chart:SfCartesianChart.Series>
        <chart:SplineSeries ItemsSource="{Binding Data}" 
                            XBindingPath="Day" 
                            YBindingPath="Degree" ShowDataLabels="True"/>
    </chart:SfCartesianChart.Series>

</chart:SfCartesianChart> 
```
**[C#]**

```
SfCartesianChart chart = new SfCartesianChart();
chart.BindingContext = new ViewModel();
. . .
var binding = new Binding() { Path = "Data" };
var splineSeries = new SplineSeries()
{
XBindingPath = "Day",
YBindingPath = "Degree", 
ShowDataLabels = true
};

splineSeries.SetBinding(ChartSeries.ItemsSourceProperty, binding);
chart.Series.Add(splineSeries);
```
