## 思维导航

* [项目介绍](https://github.com)
* [Blazor是什么？](https://github.com)
* [项目源代码](https://github.com)
* [组件库引入](https://github.com)
* [更多组件库样式展示](https://github.com)
* [项目源码地址](https://github.com)
* [优秀项目和框架精选](https://github.com)
* [DotNetGuide技术社区](https://github.com)

## 项目介绍


MudBlazor是一个基于Material Design风格开源、免费（MIT License）、功能强大的Blazor组件框架，注重易用性和清晰的结构。它非常适合想要快速构建Web应用程序的 .NET 开发人员，无需费力地处理 CSS 和 JavaScript。由于MudBlazor完全使用C\#编写，因此你可以自由地调整、修复或扩展该框架。文档中有大量示例代码，能够帮助开发者快速理解和学习MudBlazor框架。


## Blazor是什么？


Blazor是一个使用 .NET框架和C\#编程语言Razor语法构建Web应用程序的UI框架，它可以用于构建单页应用（SPA）和 Web服务，它使用编译的C\#来操纵HTML DOM来替代JavaScript。Blazor 的目标是让开发人员使用C\#编程语言来编写 Web 应用程序，使得C\#程序员可以在一个熟悉的编程语言中完成整个应用程序的开发。这样既可以提高开发效率，也可以减少学习成本。


* [全面的ASP.NET Core Blazor简介和快速入门](https://github.com):[楚门加速器p](https://tianchuang88.com)


## 项目源代码


![](https://img2024.cnblogs.com/blog/1336199/202411/1336199-20241108225052535-194838200.png)


## 组件库引入


### 安装NuGet包



```
dotnet add package MudBlazor
```

### 将以下内容添加到 `_Imports.razor`中



```
@using MudBlazor
```

### 将以下内容添加到`App.razor`中



### 将以下内容添加到`index.html`中



![](https://img2024.cnblogs.com/blog/1336199/202411/1336199-20241108225109853-481513278.png)


### 将以下内容添加到 Program.cs 的相关部分



```
using MudBlazor.Services;builder.Services.AddMudServices();
```

### Bar Chart


![](https://img2024.cnblogs.com/blog/1336199/202411/1336199-20241108225218839-764589246.png)



```
    Selected portion of the chart: @Index@code {    private int Index = -1; //default value cannot be 0 -> first selectedindex is 0.    public List Series = new List()    {        new ChartSeries() { Name = "United States", Data = new double[] { 40, 20, 25, 27, 46, 60, 48, 80, 15 } },        new ChartSeries() { Name = "Germany", Data = new double[] { 19, 24, 35, 13, 28, 15, 13, 16, 31 } },        new ChartSeries() { Name = "Sweden", Data = new double[] { 8, 6, 11, 13, 4, 16, 10, 16, 18 } },    };    public string[] XAxisLabels = { "Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep" };}
```

### Basic Pie


![](https://img2024.cnblogs.com/blog/1336199/202411/1336199-20241108225235243-2114979412.png)



```
        Add    Randomize    Remove  Selected portion of the chart: @Index@code {    private int Index = -1; //default value cannot be 0 -> first selectedindex is 0.    int dataSize = 4;    double[] data = { 77, 25, 20, 5 };    string[] labels = { "Uranium", "Plutonium", "Thorium", "Caesium", "Technetium", "Promethium",                        "Polonium", "Astatine", "Radon", "Francium", "Radium", "Actinium", "Protactinium",                        "Neptunium", "Americium", "Curium", "Berkelium", "Californium", "Einsteinium", "Mudblaznium" };    Random random = new Random();    void RandomizeData()    {        var new_data = new double[dataSize];        for (int i = 0; i < new_data.Length; i++)            new_data[i] = random.NextDouble() * 100;        data = new_data;        StateHasChanged();    }    void AddDataSize()    {        if (dataSize < 20)        {            dataSize = dataSize + 1;            RandomizeData();        }    }    void RemoveDataSize()    {        if (dataSize > 0)        {            dataSize = dataSize - 1;            RandomizeData();        }    }}
```

### Time Series Chart


![](https://img2024.cnblogs.com/blog/1336199/202411/1336199-20241108225254328-1775754941.png)



```
@using MudBlazor.Components.Chart.Models


    <MudTimeSeriesChart ChartSeries="@_series" @bind-SelectedIndex="Index" Width="100%" Height="350px" ChartOptions="@_options" CanHideSeries TimeLabelSpacing="TimeSpan.FromMinutes(5)" />
    <MudGrid>
        <MudItem xs="6">
            <MudText Typo="Typo.body1" Class="py-3">Selected: @(Index < 0 ? "None" : _series[Index].Name)MudText>
        MudItem>
        <MudItem xs="6">
            <MudSlider @bind-Value="_options.LineStrokeWidth" Min="1" Max="10" Color="Color.Info">Line Width: @_options.LineStrokeWidth.ToString()MudSlider>
        MudItem>
    MudGrid>

@code
{
    private int Index = -1; //default value cannot be 0 -> first selectedindex is 0.
    private ChartOptions _options = new ChartOptions
        {
            YAxisLines = false,
            YAxisTicks = 500,
            MaxNumYAxisTicks = 10,
            YAxisRequireZeroPoint = true,
            XAxisLines = false,
            LineStrokeWidth = 1,
        };

    private TimeSeriesChartSeries _chart1 = new();
    private TimeSeriesChartSeries _chart2 = new();
    private TimeSeriesChartSeries _chart3 = new();

    private List<TimeSeriesChartSeries> _series = new();

    private readonly Random _random = new Random();

    protected override void OnInitialized()
    {
        base.OnInitialized();

        var now = DateTime.Now;

        _chart1 = new TimeSeriesChartSeries
            {
                Index = 0,
                Name = "Series 1",
                Data = Enumerable.Range(-360, 360).Select(x => new TimeSeriesChartSeries.TimeValue(now.AddSeconds(x * 10), _random.Next(6000, 15000))).ToList(),
                IsVisible = true,
                Type = TimeSeriesDiplayType.Line
            };

        _chart2 = new TimeSeriesChartSeries
            {
                Index = 1,
                Name = "Series 2",
                Data = Enumerable.Range(-360, 360).Select(x => new TimeSeriesChartSeries.TimeValue(now.AddSeconds(x * 10), _random.Next(0, 7000))).ToList(),
                IsVisible = true,
                Type = TimeSeriesDiplayType.Area
            };

        _chart3 = new TimeSeriesChartSeries
            {
                Index = 2,
                Name = "Series 3",
                Data = Enumerable.Range(-90, 60).Select(x => new TimeSeriesChartSeries.TimeValue(now.AddSeconds(x * 30), _random.Next(4000, 10000))).ToList(),
                IsVisible = true,
                Type = TimeSeriesDiplayType.Line
            };

        _series.Add(_chart1);
        _series.Add(_chart2);
        _series.Add(_chart3);

        StateHasChanged();
    }
}

```

## 更多组件库样式展示


![](https://img2024.cnblogs.com/blog/1336199/202411/1336199-20241108225311278-93547181.png)


![](https://img2024.cnblogs.com/blog/1336199/202411/1336199-20241108225315373-2063037986.png)


![](https://img2024.cnblogs.com/blog/1336199/202411/1336199-20241108225321606-47032519.png)


![](https://img2024.cnblogs.com/blog/1336199/202411/1336199-20241108225327115-1743100743.png)


![](https://img2024.cnblogs.com/blog/1336199/202411/1336199-20241108225334029-948278536.png)


![](https://img2024.cnblogs.com/blog/1336199/202411/1336199-20241108225339887-987680246.png)


![](https://img2024.cnblogs.com/blog/1336199/202411/1336199-20241108225347708-466134297.png)


![](https://img2024.cnblogs.com/blog/1336199/202411/1336199-20241108225353183-1000873505.png)


![](https://img2024.cnblogs.com/blog/1336199/202411/1336199-20241108225358672-1300065145.png)


![](https://img2024.cnblogs.com/blog/1336199/202411/1336199-20241108225404145-1304409029.png)


## 项目源码地址


更多项目实用功能和特性欢迎前往项目开源地址查看👀，别忘了给项目一个Star支持💖。


* 开源地址：[https://github.com/MudBlazor/MudBlazor](https://github.com)
* 在线文档：[https://mudblazor.com/docs/overview](https://github.com)


## 优秀项目和框架精选


该项目已收录到C\#/.NET/.NET Core优秀项目和框架精选中，关注优秀项目和框架精选能让你及时了解C\#、.NET和.NET Core领域的最新动态和最佳实践，提高开发工作效率和质量。坑已挖，欢迎大家踊跃提交PR推荐或自荐（让优秀的项目和框架不被埋没🤞）。


* GitHub开源地址：[https://github.com/YSGStudyHards/DotNetGuide/blob/main/docs/DotNet/DotNetProjectPicks.md](https://github.com)
* Gitee开源地址：[https://gitee.com/ysgdaydayup/DotNetGuide/blob/main/docs/DotNet/DotNetProjectPicks.md](https://github.com)


## DotNetGuide技术社区


* DotNetGuide技术社区是一个面向.NET开发者的开源技术社区，旨在为开发者们提供全面的C\#/.NET/.NET Core相关学习资料、技术分享和咨询、项目框架推荐、求职和招聘资讯、以及解决问题的平台。
* 在DotNetGuide技术社区中，开发者们可以分享自己的技术文章、项目经验、学习心得、遇到的疑难技术问题以及解决方案，并且还有机会结识志同道合的开发者。
* 我们致力于构建一个积极向上、和谐友善的.NET技术交流平台。无论您是初学者还是有丰富经验的开发者，我们都希望能为您提供更多的价值和成长机会。



> [**欢迎加入DotNetGuide技术社区微信交流群👪**](https://github.com)


