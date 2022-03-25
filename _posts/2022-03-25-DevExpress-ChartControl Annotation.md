---
title: DevExpress 21v Chart Control Annotation 표시 방법
author: BeomBeomJoJo
date: 2022-03-25 12:33:00 +0800
categories: [DevExpress, WPF]
tags: [DevExpress, WPF]
math: true
mermaid: true
---


## **DevExpress Chart 기능 POC - Annotation 표시 방법 1**
* DevExpress Chart 중 Bar 차트에서 Line Annotation 표시 방법 POC 진행합니다.

<br/>

## **예제 코드**

```xml
<dx:ThemedWindow x:Class="DXApplication1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:dx="http://schemas.devexpress.com/winfx/2008/xaml/core"
        xmlns:dxe="http://schemas.devexpress.com/winfx/2008/xaml/editors" 
        xmlns:dxc="http://schemas.devexpress.com/winfx/2008/xaml/charts" 
                 xmlns:local="clr-namespace:DXApplication1"
        Title="MainWindow" Height="350" Width="525">
    <dx:ThemedWindow.DataContext>
        <local:ChartViewModel/>
    </dx:ThemedWindow.DataContext>
    <Grid>
        <dxc:ChartControl Name="chartControl1">
            <dxc:ChartControl.Diagram>
                <dxc:XYDiagram2D Name="chartDiagram" SeriesItemsSource="{Binding Data}" >
                    <dxc:XYDiagram2D.SeriesItemTemplate>
                        <DataTemplate>
                            <dxc:BarSideBySideSeries2D DisplayName="{Binding Name}" 
                                               DataSource="{Binding Values}"
                                               ArgumentDataMember="ClassNumber"
                                               ValueDataMember="Value" 
                                               BarWidth="0.6">
                                <dxc:BarSideBySideSeries2D.Model>
                                    <dxc:CustomBar2DModel>
                                        <dxc:CustomBar2DModel.PointTemplate>
                                            <ControlTemplate>
                                                <Canvas Background="SkyBlue">
                                                    <Rectangle Height="1" 
                                                               Width="{Binding RelativeSource={RelativeSource FindAncestor, AncestorType=Canvas, AncestorLevel=1}, Path=ActualWidth}" 
                                                               Canvas.Top="{Binding SeriesPoint.Tag.LinePosition}">
                                                        <Rectangle.Fill>
                                                            <SolidColorBrush Color="Black"></SolidColorBrush>
                                                        </Rectangle.Fill>
                                                        
                                                    </Rectangle>
                                                    <TextBox Text="SpecOver"
                                                             FontSize="6"
                                                             Canvas.Top="{Binding SeriesPoint.Tag.TestPosition}"/>
                                                </Canvas>
                                            </ControlTemplate>
                                        </dxc:CustomBar2DModel.PointTemplate>
                                    </dxc:CustomBar2DModel>
                                </dxc:BarSideBySideSeries2D.Model>
                            </dxc:BarSideBySideSeries2D>
                        </DataTemplate>
                    </dxc:XYDiagram2D.SeriesItemTemplate>
                    <dxc:XYDiagram2D.AxisX>
                        <dxc:AxisX2D>
                            <dxc:AxisX2D.DateTimeScaleOptions>
                                <dxc:ManualDateTimeScaleOptions MeasureUnit="Year" GridAlignment="Year" 
                                                                AutoGrid="False" GridSpacing="1"/>
                            </dxc:AxisX2D.DateTimeScaleOptions>
                        </dxc:AxisX2D>
                    </dxc:XYDiagram2D.AxisX>
                </dxc:XYDiagram2D>
            </dxc:ChartControl.Diagram>
            <dxc:ChartControl.Legends>
                <dxc:Legend HorizontalPosition="Right"/>
            </dxc:ChartControl.Legends>
            <dxc:ChartControl.Titles>
                <dxc:Title
                    Dock="Top"
                    HorizontalAlignment="Center"
                    FontSize="12"
                    Content="Chart Title" />
                <dxc:Title
                    Dock="Top"
                    HorizontalAlignment="Center"
                    FontSize="12"
                    Content="(Poisson)" />
            </dxc:ChartControl.Titles>
        </dxc:ChartControl>
    </Grid>

</dx:ThemedWindow>
```

<br/>

```csharp
using DevExpress.Xpf.Core;
using System.Collections.ObjectModel;

namespace DXApplication1
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : ThemedWindow
    {
        public MainWindow()
        {
            InitializeComponent();
        }   
    }

    public class ChartViewModel
    {
        public ObservableCollection<DataSeries> Data { get; private set; }

        public ChartViewModel()
        {
            Data = new ObservableCollection<DataSeries> {
                new DataSeries{
                    Name = "Test",
                    Values = new ObservableCollection<DataPoint> {
                        new DataPoint ("1-Test", 0.11, 100),
                        new DataPoint ("2-Test", 0.2, 50),
                        new DataPoint ("3-Test", 0.3, 10),
                        new DataPoint ("4-Test", 1.43, 20),
                        new DataPoint ("6-Test", 1.55, 15),
                        new DataPoint ("9-Test", 1.75, 50),
                        new DataPoint ("65-Test", 2.45, 24),
                        new DataPoint ("74-Test", 0.22, 33),
                        new DataPoint ("100-Test", 0.98, 44)
                    }
                }
            };
        }

        public class DataSeries
        {
            public string Name { get; set; }
            public ObservableCollection<DataPoint> Values { get; set; }
        }

        public class DataPoint
        {
            public double LinePosition {get; set;}

            public double TestPosition { get => LinePosition + 5; }
            public string ClassNumber { get; set; }
            public double Value { get; set; }
            public DataPoint(string classNumber, double value, double position)
            {
                ClassNumber = classNumber;
                Value = value;
                LinePosition = position;
            }
        }
    }
}
```

## **실행 결과**
* 실행 결과, DevExpress Chart에서 Line Annotation 사용 가능합니다.

![1](https://user-images.githubusercontent.com/22911504/160103892-16a5e6e7-3e89-4807-a395-8b055eeb981a.png)

<br/>
