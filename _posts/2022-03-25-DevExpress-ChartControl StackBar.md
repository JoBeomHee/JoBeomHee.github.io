---
title: DevExpress 21v Stack Bar Chart 컨트롤
author: BeomBeomJoJo
date: 2022-03-25 12:33:00 +0800
categories: [DevExpress, WPF]
tags: [DevExpress, WPF]
math: true
mermaid: true
---

## **DevExpress Chart 기능 POC - Stack Bar Chart**
* DevExpress Chart 중 Bart 차트에서 Stack Bar 차트 POC 진행합니다.

<br/>

## **예제 코드**

```xml
<dx:ThemedWindow
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:dx="http://schemas.devexpress.com/winfx/2008/xaml/core"
    xmlns:dxe="http://schemas.devexpress.com/winfx/2008/xaml/editors" 
    xmlns:dxc="http://schemas.devexpress.com/winfx/2008/xaml/charts" 
    x:Class="DXApplication1.MainWindow"
    Title="MainWindow" Height="450" Width="450">
    <Grid>
        <dxc:ChartControl Name="chartControl">
            <dxc:ChartControl.Titles>
                <dxc:Title Content="Sales by Regions" 
                           HorizontalAlignment="Center"/>
            </dxc:ChartControl.Titles>
            <dxc:ChartControl.Legends>
                <dxc:Legend HorizontalPosition="RightOutside"/>
            </dxc:ChartControl.Legends>
            
            <dxc:ChartControl.Diagram>
                <dxc:XYDiagram2D>
                    <dxc:XYDiagram2D.Series>
                        
                        <dxc:BarStackedSeries2D DisplayName="First Series" Brush="Red">
                            <dxc:BarStackedSeries2D.Model>
                                <dxc:FlatBar2DModel/>
                            </dxc:BarStackedSeries2D.Model>
                            <dxc:BarStackedSeries2D.Points>
                                <dxc:SeriesPoint Argument="A" Value="2"/>
                                <dxc:SeriesPoint Argument="B" Value="2"/>
                                <dxc:SeriesPoint Argument="C" Value="1"/>
                                <dxc:SeriesPoint Argument="D" Value="6"/>
                            </dxc:BarStackedSeries2D.Points>
                        </dxc:BarStackedSeries2D>
                        
                        <dxc:BarStackedSeries2D DisplayName="Second Series" Brush="Green">
                            <dxc:BarStackedSeries2D.Model>
                                <dxc:FlatBar2DModel/>
                            </dxc:BarStackedSeries2D.Model>
                            <dxc:BarStackedSeries2D.Points>
                                <dxc:SeriesPoint Argument="A" Value="4"/>
                                <dxc:SeriesPoint Argument="B" Value="3"/>
                                <dxc:SeriesPoint Argument="C" Value="2"/>
                                <dxc:SeriesPoint Argument="D" Value="1"/>
                            </dxc:BarStackedSeries2D.Points>
                        </dxc:BarStackedSeries2D>

                        <dxc:BarStackedSeries2D DisplayName="Third Series" Brush="Yellow">
                            <dxc:BarStackedSeries2D.Model>
                                <dxc:FlatBar2DModel/>
                            </dxc:BarStackedSeries2D.Model>
                            <dxc:BarStackedSeries2D.Points>
                                <dxc:SeriesPoint Argument="A" Value="6"/>
                                <dxc:SeriesPoint Argument="B" Value="2"/>
                                <dxc:SeriesPoint Argument="C" Value="1"/>
                                <dxc:SeriesPoint Argument="D" Value="3"/>
                            </dxc:BarStackedSeries2D.Points>
                        </dxc:BarStackedSeries2D>

                        <dxc:BarStackedSeries2D DisplayName="Fourth Series" Brush="Blue">
                            <dxc:BarStackedSeries2D.Model>
                                <dxc:FlatBar2DModel/>
                            </dxc:BarStackedSeries2D.Model>
                            <dxc:BarStackedSeries2D.Points>
                                <dxc:SeriesPoint Argument="A" Value="2"/>
                                <dxc:SeriesPoint Argument="B" Value="3"/>
                                <dxc:SeriesPoint Argument="C" Value="4"/>
                                <dxc:SeriesPoint Argument="D" Value="5"/>
                            </dxc:BarStackedSeries2D.Points>
                        </dxc:BarStackedSeries2D>

                    </dxc:XYDiagram2D.Series>
                </dxc:XYDiagram2D>
            </dxc:ChartControl.Diagram>
        </dxc:ChartControl>
    </Grid>
</dx:ThemedWindow>
```

<br/>

## **실행 결과**
* 실행 결과, Stack Bar Chart 컨트롤 정상 실행 됩니다.

![1](https://user-images.githubusercontent.com/22911504/160102624-4fbe4417-f22b-498d-95bf-71fa423bcc03.png)