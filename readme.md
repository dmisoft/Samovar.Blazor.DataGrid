# SamovarGrid. DataGrid for Blazor.
## Functions
- Sorting
- Default sorting field
- Paging
- Editing
- Deleting
- Cell template
- Row single selection
- Keyboard navigation
- Virtual scrolling 
```html
    <SamovarGrid Pageable="false" </SamovarGrid>
```

v2.1.0
    Resizing table columns with the mouse
    Column width mode: relative, absolute

Column width mode:
    - Relative (default) with '*' annotation
```html
    <GridColumn Title="Date" Field="@nameof(WeatherForecast.Date)" /><!--default '1*'-->
    <GridColumn Width="2*" Title="TemperatureC" Field="@nameof(WeatherForecast.TemperatureC)" />
    <GridColumn Width="3*" Title="TemperatureF" Field="@nameof(WeatherForecast.TemperatureF)" />
```
    - Absolute 
```html
    <GridColumn Width="100px" Title="TemperatureC" Field="@nameof(WeatherForecast.TemperatureC)" />
    <GridColumn Width="150px" Title="TemperatureF" Field="@nameof(WeatherForecast.TemperatureF)" />
```

Styling:
```html
    <GridColumn TableTagClass="table table-dark table-sm table-striped table-hover" ... />
    <GridColumn TheadTagClass="thead-light" ... />
    <GridColumn SelectedRowClass="bg-warning" ... />
```

## Prerequisites 
- Bootstrap 4
- Open Iconic

## Javascript and CSS files
```html
Bind Samovar Javascript and CSS files in _Host.cshtml
...
<link href="_content/SamovarGrid/samovar.grid.css" rel="stylesheet" />
...
<script src="_content/SamovarGrid/samovar.grid.js"></script>
...

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>PacketServerTest</title>
    <base href="~/" />
    <link rel="stylesheet" href="css/bootstrap/bootstrap.min.css" />
    <link href="css/site.css" rel="stylesheet" />
    <link href="_content/SamovarGrid/samovar.grid.css" rel="stylesheet" />

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"></script>
</head>
<body>
    <app>
        @(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))
    </app>

    <script src="_framework/blazor.server.js"></script>
    <script src="_content/SamovarGrid/samovar.grid.js"></script>
</body>
</html>
```


## TODO for Samples: Modify standard VS template code for data service

```csharp
public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate, int dataCnt)
        {
            var rng = new Random();
            int pos = 0;
            return Task.FromResult(Enumerable.Range(1, dataCnt).Select(index => new WeatherForecast
            {
                Position = ++pos,
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                SummaryDbl = rng.Next(-20, 55) + 0.125,
                Summary = Summaries[rng.Next(Summaries.Length)],
                YesNo = true,
                SByteValue = (sbyte)rng.Next(-128, 127),
                ByteValue = (byte)rng.Next(0, 255),
                CharValue = 'c'
            }).ToArray());
        }
```

## Sample 1. Pageable Grid

```html
@page "/"
@using Samovar
@using System.Linq
@using SamovarGridProServerTest.Data
@inject WeatherForecastService ForecastService

<h1>Table basic (pageable)</h1>

<SamovarGrid Data="@forecasts1"
             OrderFieldByDefault="@nameof(WeatherForecast.TemperatureC)"
             OrderDesc="true"
             Pageable="true"
             PageSize="20"
             PagerSize="10"
             Height="400px">
    <GridColumns>
        <GridColumn Title="Date" Field="@nameof(WeatherForecast.Date)" />
        <GridColumn Title="TemperatureC" Field="@nameof(WeatherForecast.TemperatureC)" />
        <GridColumn Title="TemperatureF" Field="@nameof(WeatherForecast.TemperatureF)" />
        <GridColumn Title="Summary" Field="@nameof(WeatherForecast.Summary)" />
    </GridColumns>
</SamovarGrid>

<h1>Table basic (pageable headless)</h1>

<SamovarGrid Data="@forecasts2"
             OrderDesc="true"
             Pageable="true"
             PageSize="20"
             PagerSize="10"
             Height="400px"
             Headless="true">
    <GridColumns>
        <GridColumn Title="Date" Field="@nameof(WeatherForecast.Date)" />
        <GridColumn Title="TemperatureC" Field="@nameof(WeatherForecast.TemperatureC)" />
        <GridColumn Title="TemperatureF" Field="@nameof(WeatherForecast.TemperatureF)" />
        <GridColumn Title="Summary" Field="@nameof(WeatherForecast.Summary)" />
    </GridColumns>
</SamovarGrid>
```
```csharp
@code{
    List<WeatherForecast> forecasts1 = new List<WeatherForecast>();
    List<WeatherForecast> forecasts2 = new List<WeatherForecast>();

    protected override async Task OnInitializedAsync()
    {
        forecasts1 = (await ForecastService.GetForecastAsync(DateTime.Now, 1000)).ToList();
        forecasts2 = (await ForecastService.GetForecastAsync(DateTime.Now, 100)).ToList();
    }
}
```


## Sample 2. Virtual scrolling

```html
@page "/TableBasicVirtualScroll"
@using Samovar
@using System.Linq
@using SamovarGridProServerTest.Data
@inject WeatherForecastService ForecastService

<h1>Table basic (virtual scroll)</h1>

<SamovarGrid Data="@forecasts1"
             Pageable="false"
             Height="400px">
    <GridColumns>
        <GridColumn Title="Date" Field="@nameof(WeatherForecast.Date)" />
        <GridColumn Title="TemperatureC" Field="@nameof(WeatherForecast.TemperatureC)" />
        <GridColumn Title="TemperatureF" Field="@nameof(WeatherForecast.TemperatureF)" />
        <GridColumn Title="Summary" Field="@nameof(WeatherForecast.Summary)" />
    </GridColumns>
</SamovarGrid>

<h1 class="mt-4">Table basic (virtual scroll and headless)</h1>

<SamovarGrid Data="@forecasts2"
             Pageable="false"
             Height="400px"
             Headless="true">
    <GridColumns>
        <GridColumn Title="Date" Field="@nameof(WeatherForecast.Date)" />
        <GridColumn Title="TemperatureC" Field="@nameof(WeatherForecast.TemperatureC)" />
        <GridColumn Title="TemperatureF" Field="@nameof(WeatherForecast.TemperatureF)" />
        <GridColumn Title="Summary" Field="@nameof(WeatherForecast.Summary)" />
    </GridColumns>
</SamovarGrid>
```
```csharp
@code{
        List<WeatherForecast> forecasts1 = new List<WeatherForecast>();
    List<WeatherForecast> forecasts2 = new List<WeatherForecast>();

    protected override async Task OnInitializedAsync()
    {
        forecasts1 = (await ForecastService.GetForecastAsync(DateTime.Now, 10000)).ToList();
        forecasts2 = (await ForecastService.GetForecastAsync(DateTime.Now, 15000)).ToList();
    }
}
```
## Sample 3. Column width

```html
@page "/ColumnWidth"
@using Samovar
@using System.Linq
@using SamovarGridProServerTest.Data
@inject WeatherForecastService ForecastService

<h1>Column width. Relative and absolute together</h1>

<SamovarGrid Data="@forecasts1"
             OrderFieldByDefault="@nameof(WeatherForecast.TemperatureC)"
             OrderDesc="true"
             Pageable="true"
             PageSize="20"
             PagerSize="10"
             Height="400px">
    <GridColumns>
        <GridColumn Title="Date" Field="@nameof(WeatherForecast.Date)" Width="200px" />
        <GridColumn Title="TemperatureC" Field="@nameof(WeatherForecast.TemperatureC)" Width="3*" />
        <GridColumn Title="TemperatureF" Field="@nameof(WeatherForecast.TemperatureF)" Width="2*" />
        <GridColumn Title="Summary" Field="@nameof(WeatherForecast.Summary)" Width="100px" />
    </GridColumns>
</SamovarGrid>

<h1 class="mt-4">Column width. Absolute col width</h1>

<SamovarGrid Data="@forecasts2"
             OrderFieldByDefault="@nameof(WeatherForecast.TemperatureC)"
             OrderDesc="true"
             Pageable="true"
             PageSize="20"
             PagerSize="10"
             Height="400px">
    <GridColumns>
        <GridColumn Title="Date" Field="@nameof(WeatherForecast.Date)" Width="200px" />
        <GridColumn Title="TemperatureC" Field="@nameof(WeatherForecast.TemperatureC)" Width="100px" />
        <GridColumn Title="TemperatureF" Field="@nameof(WeatherForecast.TemperatureF)" Width="150px" />
        <GridColumn Title="Summary" Field="@nameof(WeatherForecast.Summary)" Width="100px" />
    </GridColumns>
</SamovarGrid>
```
```csharp
@code{
    List<WeatherForecast> forecasts1 = new List<WeatherForecast>();
    List<WeatherForecast> forecasts2 = new List<WeatherForecast>();

    protected override async Task OnInitializedAsync()
    {
        forecasts1 = (await ForecastService.GetForecastAsync(DateTime.Now, 1000)).ToList();
        forecasts2 = (await ForecastService.GetForecastAsync(DateTime.Now, 1000)).ToList();
    }
}
```

## Sample 4. Commands and event handlers

```html
@page "/GridCommands"
@using Samovar
@using Samovar.Common
@using System.Linq
@using SamovarGridProServerTest.Data
@inject WeatherForecastService ForecastService

<h1>Commands and event handlers</h1>
<button class="btn btn-primary" @onclick="AddNewRow">add new row</button>
<button class="btn btn-primary" @onclick="CreateNewDataSet">create new data set</button>
<button class="btn btn-primary" @onclick="ClearDataSet">clear data set</button>
<SamovarGrid Data="@forecasts"
             OrderFieldByDefault="@nameof(WeatherForecast.TemperatureC)"
             OrderDesc="true"
             Pageable="true"
             PageSize="20"
             PagerSize="10"
             Height="@gridHeight"
             Width="@gridWidth"
             OnRowSelected="RowSelectedHandler"
             OnRowDeleteBegin="RowDeleteBeginHandler"
             OnRowDeleteCommit="RowDeleteCommitHandler"
             OnRowDeleteCancel="RowDeleteCancelHandler"
             OnRowEditBegin="RowEditBeginHandler"
             OnRowEditCommit="RowEditCommitHandler"
             OnRowEditCancel="RowEditCancelHandler"
             TableTagClass="table table-striped table-hover"
             Headless="false">
    <GridColumns>
        <GridColumn Title="Date" Field="@nameof(WeatherForecast.Date)">
            <CellShowTemplate>
                @{
                    WeatherForecast data = context as WeatherForecast;
                    <div>@data.Date.ToShortDateString()</div>
                }
            </CellShowTemplate>
        </GridColumn>
        <GridColumn Title="TemperatureC" Field="@nameof(WeatherForecast.TemperatureC)" />
        <GridColumn Title="TemperatureF" Field="@nameof(WeatherForecast.TemperatureF)" />
        <GridColumn Title="Summary" Field="@nameof(WeatherForecast.Summary)">
            <CellEditTemplate>
                @{
                    WeatherForecast data = context as WeatherForecast;
                    <input type="text" @bind="@data.Summary" />
                }
            </CellEditTemplate>
            <CellShowTemplate>
                @{
                    WeatherForecast data = context as WeatherForecast;
                    <div style="word-break:break-all;">@data.Summary</div>
                }
            </CellShowTemplate>
        </GridColumn>
        <GridCommandColumn>
            <GridCommand CommandName="edit" CommandType=GridRowCommandType.Edit />
            <GridCommand CommandName="delete" CommandType=GridRowCommandType.Delete />
        </GridCommandColumn>
    </GridColumns>
</SamovarGrid>

<h4>Grid items</h4>
<div>@forecasts.Count().ToString()</div>

<h4>Action</h4>
<div>@selectedAction</div>

<h4>Action row item</h4>
<div>@actionItem</div>
```
```csharp
@code{
    string gridHeight = "600px";
    string gridWidth = "100%";

    string selectedAction { set; get; }
    string actionItem { set; get; }

    List<WeatherForecast> forecasts = new List<WeatherForecast>();

    protected override async Task OnInitializedAsync()
    {
        forecasts = (await ForecastService.GetForecastAsync(DateTime.Now)).ToList();
    }

    void RowSelectedHandler(GridRowEventArgs args)
    {
        actionItem = ((WeatherForecast)args.RowData).Summary;
        selectedAction = "RowSelected";
    }

    void RowDeleteBeginHandler(GridRowEventArgs args)
    {
        actionItem = ((WeatherForecast)args.RowData).Summary;
        selectedAction = "RowDeleteBegin";
    }

    void RowDeleteCancelHandler(GridRowEventArgs args)
    {
        actionItem = ((WeatherForecast)args.RowData).Summary;
        selectedAction = "RowDeleteCancel";
    }

    void RowDeleteCommitHandler(GridRowEventArgs args)
    {
        actionItem = ((WeatherForecast)args.RowData).Summary;
        this.forecasts.Remove((WeatherForecast)args.RowData);
        selectedAction = "RowDeleteCommit";
    }

    void RowEditCommitHandler(GridRowEventArgs args)
    {
        WeatherForecast item = (WeatherForecast)args.RowData;
    }

    void RowEditBeginHandler(GridRowEventArgs args)
    {
        actionItem = ((WeatherForecast)args.RowData).Summary;
        selectedAction = "RowEditBegin";
    }

    void RowEditCancelHandler(GridRowEventArgs args)
    {
        actionItem = ((WeatherForecast)args.RowData).Summary;
        selectedAction = "RowEditCancel";
    }

    void AddNewRow()
    {
        forecasts.Add(new WeatherForecast
        {
            Date = new DateTime(2019, 1, 1),
            Summary = "Summary",
            Summary2 = "Summary2",
            TemperatureC = 33
        });
    }

    async Task CreateNewDataSet()
    {
        forecasts = (await ForecastService.GetForecastAsync(DateTime.Now)).ToList();
    }

    void ClearDataSet()
    {
        forecasts.Clear();
    }   
}
```

## Sample 5. Grid cell templating 

```html
@page "/GridCellTemplating"
@using Samovar
@using Samovar.Common
@using System.Linq
@using SamovarGridProServerTest.Data
@inject WeatherForecastService ForecastService

<h1>Cell templating</h1>

<SamovarGrid Data="@forecasts"
             OrderFieldByDefault="@nameof(WeatherForecast.TemperatureC)"
             OrderDesc="true"
             Pageable="false"
             PageSize="20"
             PagerSize="10"
             Height="@gridHeight"
             OnRowSelected="RowSelectedHandler"
             OnRowDeleteBegin="RowDeleteBeginHandler"
             OnRowDeleteCommit="RowDeleteCommitHandler"
             OnRowDeleteCancel="RowDeleteCancelHandler"
             OnRowEditBegin="RowEditBeginHandler"
             OnRowEditCommit="RowEditCommitHandler"
             OnRowEditCancel="RowEditCancelHandler"
             Headless="false"
             SelectedRowClass="bg-warning">
    <GridColumns>
        <GridColumn Title="Date" Width="100px">
            <CellShowTemplate>
                @{
                    WeatherForecast data = context as WeatherForecast;
                    <div>@data.Date.ToShortDateString()</div>
                }
            </CellShowTemplate>
        </GridColumn>
        <GridColumn Title="Date2" Field="@nameof(WeatherForecast.Date)" Width="200px" />
        <GridColumn Title="TemperatureC" Field="@nameof(WeatherForecast.TemperatureC)" Width="200px" />
        <GridColumn Title="TemperatureF" Field="@nameof(WeatherForecast.TemperatureF)" Width="150px" />
        <GridColumn Title="Summary" Field="@nameof(WeatherForecast.Summary)" Width="150px">
            <CellEditTemplate>
                @{
                    WeatherForecast data = context as WeatherForecast;
                    <input style="width:100%;" type="text" @bind="@data.Summary" @oninput="onInput" />

                    void onInput(ChangeEventArgs args)
                    {
                    }
                }
            </CellEditTemplate>
            <CellShowTemplate>
                @{
                    WeatherForecast data = context as WeatherForecast;
                    <div style="word-break:break-all;">@data.Summary</div>
                }
            </CellShowTemplate>
        </GridColumn>
        <GridCommandColumn Width="120px">
            <GridCommand CommandName="edit" CommandType=GridRowCommandType.Edit />
            <GridCommand CommandName="delete" CommandType=GridRowCommandType.Delete />
        </GridCommandColumn>
    </GridColumns>
</SamovarGrid>

<div class="form-group">
    <label class="mr-2">Grid height</label>
    <input @bind-value="@gridHeight" @bind-value:event="onchange" />
</div>

<h4>Datasize</h4>
<div>@forecasts.Count()</div>

<h4>Action</h4>
<div>@selectedAction</div>

<h4>Action row item</h4>
<div>@actionItem</div>
```
```csharp
@code{
    string gridHeight = "500px";

    string selectedAction { set; get; }
    string actionItem { set; get; }

    List<WeatherForecast> forecasts = new List<WeatherForecast>();

    protected override async Task OnInitializedAsync()
    {
        forecasts = (await ForecastService.GetForecastAsync(DateTime.Now, 1000)).ToList();
    }

    void RowSelectedHandler(GridRowEventArgs args)
    {
        actionItem = ((WeatherForecast)args.RowData).Summary;
        selectedAction = "RowSelected";
    }

    void RowDeleteBeginHandler(GridRowEventArgs args)
    {
        actionItem = ((WeatherForecast)args.RowData).Summary;
        selectedAction = "RowDeleteBegin";
    }

    void RowDeleteCancelHandler(GridRowEventArgs args)
    {
        actionItem = ((WeatherForecast)args.RowData).Summary;
        selectedAction = "RowDeleteCancel";
    }

    void RowDeleteCommitHandler(GridRowEventArgs args)
    {
        actionItem = ((WeatherForecast)args.RowData).Summary;
        this.forecasts.Remove((WeatherForecast)args.RowData);
        selectedAction = "RowDeleteCommit";
    }

    void RowEditCommitHandler(GridRowEventArgs args)
    {
        WeatherForecast item = (WeatherForecast)args.RowData;
    }

    void RowEditBeginHandler(GridRowEventArgs args)
    {
        actionItem = ((WeatherForecast)args.RowData).Summary;
        selectedAction = "RowEditBegin";
    }

    void RowEditCancelHandler(GridRowEventArgs args)
    {
        actionItem = ((WeatherForecast)args.RowData).Summary;
        selectedAction = "RowEditCancel";
    }    
}
```

## Sample 6. Grid width adn height

```html
@page "/GridWidthHeight"
@using Samovar
@using Samovar.Common
@using System.Linq
@using SamovarGridProServerTest.Data
@inject WeatherForecastService ForecastService

<h1>Grid width and height</h1>

<SamovarGrid Data="@forecasts1"
             OrderFieldByDefault="@nameof(WeatherForecast.TemperatureC)"
             OrderDesc="true"
             Pageable="true"
             PageSize="20"
             PagerSize="10"
             Height="calc(100vh - 200px)"
             Width="100%">
    <GridColumns>
        <GridColumn Title="Date" Field="@nameof(WeatherForecast.Date)" />
        <GridColumn Title="TemperatureC" Field="@nameof(WeatherForecast.TemperatureC)" />
        <GridColumn Title="TemperatureF" Field="@nameof(WeatherForecast.TemperatureF)" />
        <GridColumn Title="Summary" Field="@nameof(WeatherForecast.Summary)" />
        <GridCommandColumn>
            <GridCommand CommandName="edit" CommandType=GridRowCommandType.Edit />
            <GridCommand CommandName="delete" CommandType=GridRowCommandType.Delete />
        </GridCommandColumn>
    </GridColumns>
</SamovarGrid>
```
```csharp
@code{
    List<WeatherForecast> forecasts1 = new List<WeatherForecast>();

    protected override async Task OnInitializedAsync()
    {
        forecasts1 = (await ForecastService.GetForecastAsync(DateTime.Now, 1000)).ToList();
    }    
}
```