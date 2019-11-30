---
layout: post
title: Using Syncfusions Datagrid to make a crud interface
excerpt: "In this tutorial we will be looking at Syncfusions data table component for blazor."
categories: [Programming]
tags: [Syncfusion, Blazor, ASP.net, Data grid, CRUD]
modified: 2019-11-29
comments: true
---

## Description

In this tutorial we will be looking at Syncfusions data table component for blazor.

## Goals

* Use the Syncfusion DataGrid component.
* Set up two way binding with an API.
* Be able to create/update/edit/delete content all from the control.
* Have a toolbar with create/edit/delete
* Have a context menu with edit/delete

## Perquisites

* Computer Win/Mac/Linux
* VS Code
* .Net Core 3.1 SDK
* Blazor Server side app
* Syncfusion Nuget Installed (ToDo : Create tutorial on basic setup of Syncfusion)

## Basic Data Grid - View only
This Data Grid is used to display tabular data. It can pull the data from data sources, like IEnumerable, OData web services, or  `DataManager`  and bind the object fields to columns. It also displays a column header to identify the field.

In the example below, the Grid is populated with its minimum default settings

    @page "/Grid/DefaultFunctionalities"
    @using Syncfusion.EJ2.Blazor.Grids
    <div class="col-lg-12 control-section">
        <div class="content-wrapper">
            <div class="row">
                <EjsGrid DataSource="@GridData" AllowPaging="true" >
                    <GridColumns>
                        <GridColumn Field=@nameof(OrdersDetails.OrderID) HeaderText="Order ID" TextAlign="TextAlign.Right"  Width="120"></GridColumn>
                        <GridColumn Field=@nameof(OrdersDetails.CustomerID) HeaderText="Customer Name" Width="150"></GridColumn>
                        <GridColumn Field=@nameof(OrdersDetails.OrderDate) HeaderText="Order Date" Format="yMd" Type="ColumnType.Date" TextAlign="TextAlign.Right"  Width="130"></GridColumn>
                        <GridColumn Field=@nameof(OrdersDetails.Freight) HeaderText="Freight" Format="C2" TextAlign="TextAlign.Right"  Width="120"></GridColumn>                   
                        <GridColumn Field=@nameof(OrdersDetails.ShipCountry) HeaderText="Ship Country" Width="150"></GridColumn>
                    </GridColumns>
                </EjsGrid>
            </div>
        </div>
    </div>
    @code{
        public List<OrdersDetails> GridData { get; set; }
        protected override void OnInitialized()
        {
            GridData = OrdersDetails.GetAllRecords();
        }
    }

## Data Grid - Two-Way Binding

### Custom Binding
I am going to use the custom binding solution as this fits best with my current project.

My Classes for my current project are set up as follows :-
| class | description|
|---|---|
| Service | Talks with the API & the Service class <br> Handles data transfer from API to webapp |
| State| Talks with the Service Class & Razor View <br> This handles data binding for the View, message boxes, text boxes etc. |
| Razor View| UI that binds to the State |

With this new control i will be able to refactor out a lot of messiness in my existing code :-
| class | description|
|---|---|
| ApiService | Uses: Api <br> Exposes: CRUD |
| Adapter | Uses: Service <br> Exposes: CRUD |
| Razor View| Uses: Adapter <br> Binds: Data Grid to Adapter |

The custom data binding can be performed in the Grid component by providing the custom adaptor class and overriding the  `Read or ReadAsync`  method of the DataAdaptor abstract class. The CRUD operations for the custom bounded data in the Grid component can be implemented by overriding the following CRUD methods of the DataAdaptor abstract class,  

-   `Insert/InsertAsync`  - Performs Insert operation.
-   `Remove/RemoveAsync`  - Performs Remove operation.
-   `Update/UpdateAsync`  - Performs Update operation.
-   `BatchUpdate/BatchUpdateAsync`  - Performs BatchUpdate operation.

### The code

#### Model

```csharp
    public class Address : IEntity
    {
        public string Id { get; set; }

        [Required(ErrorMessage = "Name is required")]
        public string Name { get; set; }

        [Required(ErrorMessage = "Street is required")]
        public string Street { get; set; }

        [Required(ErrorMessage = "Town is required")]
        public string Town { get; set; }

        [Required(ErrorMessage = "Postcode is required")]
        public string Postcode { get; set; }

        public string AddressString =>
            this.Name
            + ", " + this.Street
            + ", " + this.Town
            + ", " + this.Postcode;
    }
```

#### ApiService Interface

```csharp
using System.Collections.Generic;
using System.Threading.Tasks;
using curly.Models;

namespace curly.Services
{
    public interface IApiServiceBase<TEntity>
    {
        IEnumerable<TEntity> AllEntities { get; }
        global::System.Boolean GetErrors { get; }

        Task<TEntity> CreateAsync(TEntity TEntity);
        Task<TEntity> ReadAsync(global::System.String id);
        Task<IEnumerable<TEntity>> ReadAllAsync();
        Task<TEntity> UpdateAsync(TEntity TEntity);
        Task DeleteAsync(global::System.String id);
    }
}
```

#### ApiService Base Class

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json;
using curly.Models;

namespace curly.Services
{
    public abstract class ApiServiceBase<TEntity>
        where TEntity : IEntity,
        new()
    {
        private readonly IHttpClientFactory _clientFactory;
        private readonly string _apiAddress;
        private readonly string _relativePath;
        public IEnumerable<TEntity> AllEntities { get; private set; }
        public bool GetErrors { get; private set; }
        
        public ApiServiceBase(IHttpClientFactory clientFactory, string apiAddress, string relativePath)
        {
            _clientFactory = clientFactory;
            _apiAddress = apiAddress;
            _relativePath = relativePath;
        }
        
        public async Task<TEntity> CreateAsync(TEntity TEntity)
        {
            var client = _clientFactory.CreateClient();

            string output = JsonConvert.SerializeObject(TEntity);

            var result = await _clientFactory.CreateClient().PostAsync($"{_apiAddress}{_relativePath}", new StringContent(output, Encoding.UTF8, "application/json"));

            var deserializedProduct = JsonConvert.DeserializeObject<TEntity>(result.Content.ReadAsStringAsync().Result);

            return JsonConvert.DeserializeObject<TEntity>(result.Content.ReadAsStringAsync().Result);
        }
        
        public async Task<TEntity> ReadAsync(string id)
        {
            var client = _clientFactory.CreateClient();

            var result = await _clientFactory.CreateClient().GetAsync($"{_apiAddress}{_relativePath}/{id}");

            if (result.IsSuccessStatusCode)
            {
                return JsonConvert.DeserializeObject<TEntity>(result.Content.ReadAsStringAsync().Result);
            }
            else
            {
                GetErrors = true;
                return new TEntity();
            }
        }
        
        public async Task<IEnumerable<TEntity>> ReadAllAsync()
        {
            var request = new HttpRequestMessage(HttpMethod.Get, $"{_apiAddress}{_relativePath}");

            var client = _clientFactory.CreateClient();

            var response = await client.SendAsync(request);

            if (response.IsSuccessStatusCode)
            {
                using (var contentStream = await response.Content.ReadAsStreamAsync())
                {
                    AllEntities = await System.Text.Json.JsonSerializer.DeserializeAsync<IEnumerable<TEntity>>(contentStream);
                }

                return AllEntities.ToArray();
            }
            else
            {
                GetErrors = true;
                AllEntities = Array.Empty<TEntity>();
                return AllEntities.ToArray();
            }
        }

        public async Task<TEntity> UpdateAsync(TEntity TEntity)
        {
            var client = _clientFactory.CreateClient();

            string output = JsonConvert.SerializeObject(TEntity);

            var result = await _clientFactory.CreateClient().PutAsync($"{_apiAddress}{_relativePath}/{TEntity.Id}", new StringContent(output, Encoding.UTF8, "application/json"));

            var deserializedProduct = JsonConvert.DeserializeObject<TEntity>(result.Content.ReadAsStringAsync().Result);

            return JsonConvert.DeserializeObject<TEntity>(result.Content.ReadAsStringAsync().Result);
        }

        public async Task DeleteAsync(string id)
        {
            var client = _clientFactory.CreateClient();
            var result = await _clientFactory.CreateClient().DeleteAsync($"{_apiAddress}{_relativePath}/{id}");
        }
    }
}
```

#### Address Adapter

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Syncfusion.EJ2.Blazor;
using curly.Models;
using curly.Services;

namespace curly.Adapter
{
    public class AddressAdapter : DataAdaptor
    {
        public event Action OnChange;
        private void NotifyStateChanged() => OnChange?.Invoke();
        private AddressApiService _addressService;
        public AddressAdapter(AddressApiService addressService)
        {
            _addressService = addressService;
            LoadEntities();
        }

        public async void LoadEntities()
        {
            Entities = await _addressService.ReadAllAsync();
            NotifyStateChanged();
        }

        public IEnumerable<Address> Entities;

        public override Object Read(DataManagerRequest dm, string key = null)
        {
            IEnumerable<Address> DataSource  = Entities;

            if (dm.Search != null && dm.Search.Count > 0)
            {
                // Searching
                DataSource = DataOperations.PerformSearching(DataSource, dm.Search);
            }
            if (dm.Sorted != null && dm.Sorted.Count > 0)
            {
                // Sorting
                DataSource = DataOperations.PerformSorting(DataSource, dm.Sorted);
            }
            if (dm.Where != null && dm.Where.Count > 0)
            {
                // Filtering
                DataSource = DataOperations.PerformFiltering(DataSource, dm.Where, dm.Where[0].Operator);
            }
            int count = DataSource.Cast<Address>().Count();
            if (dm.Skip != 0)
            {
                //Paging
                DataSource = DataOperations.PerformSkip(DataSource, dm.Skip);
            }
            if (dm.Take != 0)
            {
                DataSource = DataOperations.PerformTake(DataSource, dm.Take);
            }
            return dm.RequiresCounts ? new DataResult() { Result = DataSource, Count = count } : (object)DataSource;
        }
        public override async Task<Object> InsertAsync(DataManager dataManager, object value, string key)
        {
            await _addressService.CreateAsync(value as Address);
            LoadEntities();
            return value;
        }
        public override async Task<object> RemoveAsync(DataManager dataManager, object value, string keyField, string key)
        {
            string data = (string)value;
            await _addressService.DeleteAsync(data);
            LoadEntities();
            return value;
        }
        public override async Task<object> UpdateAsync(DataManager dataManager, object value, string keyField, string key)
        {
            var val = await _addressService.UpdateAsync(value as Address);
            LoadEntities();
            return value;
        }
    }
}
```

#### The View

```html
@using Syncfusion.EJ2.Blazor
@using Syncfusion.EJ2.Blazor.Grids
@using Syncfusion.EJ2.Blazor.Data
@using Newtonsoft.Json
@using curly.Adapter
@using curly.Models
@inject AddressAdapter _addressAdapter
@implements IDisposable

<div class="col-lg-12 control-section">
    <div class="content-wrapper">
        <div class="row">
            <EjsGrid TValue="Address" AllowPaging="true" AllowFiltering="true" 
                Toolbar="@(new string[] {"Add","Edit","Delete","Update","Cancel"})" 
                ContextMenuItems="@(new List<object>() { "AutoFit", "AutoFitAll", "SortAscending", "SortDescending","Copy", "Edit", "Delete", "Save", "Cancel","PdfExport", "ExcelExport", "CsvExport", "FirstPage", "PrevPage","LastPage", "NextPage"})">
                <EjsDataManager AdaptorInstance="@typeof(AddressAdapter)" Adaptor="Adaptors.CustomAdaptor"></EjsDataManager>
                <GridPageSettings PageSize="25"></GridPageSettings>
                <GridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true" Mode="EditMode.Normal"></GridEditSettings>
                <GridColumns>
                    <GridColumn Field=@nameof(Address.Id) HeaderText="ID" IsPrimaryKey="true" TextAlign="@TextAlign.Center" HeaderTextAlign="@TextAlign.Center" Width="140"></GridColumn>
                    <GridColumn Field=@nameof(Address.Name) HeaderText="Name" Width="150"></GridColumn>
                    <GridColumn Field=@nameof(Address.Street) HeaderText="Street"></GridColumn>
                    <GridColumn Field=@nameof(Address.Town) HeaderText="Town"></GridColumn>
                    <GridColumn Field=@nameof(Address.Postcode) HeaderText="Postcode"></GridColumn>
                </GridColumns>
            </EjsGrid>
        </div>
    </div>
</div>

@code {
  protected override void OnInitialized()
  {
      _addressAdapter.OnChange += StateHasChanged;
  }
  
  public void Dispose()
  {
      _addressAdapter.OnChange -= StateHasChanged;
  }

}
```

![Smithsonian Image]({{ site.url }}/img/2019-11-30-using-syncfusion-blazor-datagrid.png)
{: .pull-right}

## References

*  [Syncfusion.com](https://blazor.syncfusion.com/demos/Grid/)

  

## What's Next

*  [Jekyll](https://jekyllrb.com/)