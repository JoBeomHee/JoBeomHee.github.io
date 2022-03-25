---
title: DevExpress 21v Chart Control Annotation 표시 방법 2
author: BeomBeomJoJo
date: 2022-03-25 12:33:00 +0800
categories: [DevExpress, WPF]
tags: [DevExpress, WPF]
math: true
mermaid: true
---

## **DevExpress Chart 기능 POC - Annotation 점선 표시 기능**
* DevExpress Chart Control 에서 막대바 중간에 점선 표시 가능한지 테스트 진행합니다.

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
                                                    <Line      X1="0" X2="360"
                                                               StrokeDashArray="1"                     
                                                               Stroke="Black"
                                                               StrokeThickness="2"
                                                               Width="{Binding RelativeSource={RelativeSource FindAncestor, AncestorType=Canvas, AncestorLevel=1}, Path=ActualWidth}" 
                                                               Canvas.Top="{Binding SeriesPoint.Tag.LinePosition}">
                                                    </Line>
                                                    <Label Content="SpecOver"
                                                             FontSize="7"
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
                    Content="Test" />
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

            public double TestPosition { get => LinePosition - 10; }
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

<br/>

## **DevExpress Chart Spec Over Annotation 표시**
* DevExpress Chart에서 Spec Over Annotation 표시 가능한 것 확인하였습니다. 

![1](https://user-images.githubusercontent.com/22911504/160104473-206c2ee9-34a1-4c7c-b3d1-7bb6ea907f5e.png)

<br/>
