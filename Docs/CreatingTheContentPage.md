
# 1. Create a content page

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage
    x:Class="MauiWeatherApp.Pages.WeatherInfoPage"
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:viewModels="clr-namespace:MauiWeatherApp.Models.ViewModels"
    Title="Weather Information"
    x:DataType="viewModels:WeatherInfoPageViewModel"
    BackgroundColor="#606cf2">
    <VerticalStackLayout>
        <Frame Margin="20">
            <Grid ColumnSpacing="5">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Entry
                    Grid.Column="0"
                    Placeholder="Lat"
                    Text="{Binding Latitude}" />
                <Entry
                    Grid.Column="1"
                    Placeholder="Lon"
                    Text="{Binding Longitude}" />
                <Button
                    Grid.Column="2"
                    Command="{Binding FetchWeatherInformationCommand}"
                    SemanticProperties.Hint="Fetch weather info"
                    Text="Fetch" />
            </Grid>
        </Frame>
        <Image
            Aspect="AspectFill"
            HeightRequest="100"
            Source="{Binding WeatherIcon}"
            WidthRequest="100" />
        <Label
            FontSize="70"
            HorizontalOptions="Center"
            Text="{Binding Temperature}"
            TextColor="White" />
        <Label
            FontAttributes="Bold"
            FontSize="25"
            HorizontalOptions="Center"
            Text="{Binding WeatherDescription}"
            TextColor="White" />
        <Label
            FontAttributes="Bold"
            FontSize="25"
            HorizontalOptions="Center"
            Text="{Binding Location}"
            TextColor="White" />

        <Grid Margin="20" ColumnSpacing="5">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
            <Frame Grid.Column="0">
                <VerticalStackLayout>
                    <Label
                        FontAttributes="Bold"
                        FontSize="30"
                        HorizontalOptions="Center"
                        Text="{Binding Humidity}"
                        TextColor="#606cf2" />
                    <Label
                        FontAttributes="Bold"
                        FontSize="17"
                        HorizontalOptions="Center"
                        Text="Humidity" />
                </VerticalStackLayout>
            </Frame>
            <Frame Grid.Column="1">
                <VerticalStackLayout>
                    <Label
                        FontAttributes="Bold"
                        FontSize="30"
                        HorizontalOptions="Center"
                        Text="{Binding CloudCoverLevel}"
                        TextColor="#606cf2" />
                    <Label
                        FontAttributes="Bold"
                        FontSize="17"
                        HorizontalOptions="Center"
                        Text="Cloud" />
                </VerticalStackLayout>
            </Frame>
            <Frame Grid.Column="2">
                <VerticalStackLayout>
                    <Label
                        FontAttributes="Bold"
                        FontSize="30"
                        HorizontalOptions="Center"
                        Text="{Binding IsDay}"
                        TextColor="#606cf2" />
                    <Label
                        FontAttributes="Bold"
                        FontSize="17"
                        HorizontalOptions="Center"
                        Text="Is Day" />
                </VerticalStackLayout>
            </Frame>
        </Grid>
    </VerticalStackLayout>

</ContentPage>

```

## 2. Create the View Model

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using CommunityToolkit.Mvvm.ComponentModel;
using CommunityToolkit.Mvvm.Input;
using MauiWeatherApp.Services;

namespace MauiWeatherApp.Models.ViewModels
{
    public partial class WeatherInfoPageViewModel : ObservableObject
    {
        private readonly WeatherApiService _weatherApiService;

        public WeatherInfoPageViewModel()
        {
            _weatherApiService = new WeatherApiService();
        }

        [ObservableProperty]
        private string latitude;

        [ObservableProperty]
        private string longitude;

        [ObservableProperty]
        private string weatherIcon;

        [ObservableProperty]
        private string temperature;

        [ObservableProperty]
        private string weatherDescription;

        [ObservableProperty]
        private string location;

        [ObservableProperty]
        private int humidity;

        [ObservableProperty]
        private string cloudCoverLevel;

        [ObservableProperty]
        private string isDay;

        //[RelayCommand]
        //private async Task FetchWeatherInformation()
        //{
        //  var weatherApiResponse = await _weatherApiService.GetWeatherInformation(Latitude, Longitude);
        //  if (weatherApiResponse != null)
        //  {
        //      WeatherIcon = weatherApiResponse.Current.WeatherIcons[0];
        //      Temperature = $"{weatherApiResponse.Current.Temperature}°C";
        //      Location = $"{weatherApiResponse.Location.Name}, {weatherApiResponse.Location.Region}, {weatherApiResponse.Location.Country}";
        //      WeatherDescription = weatherApiResponse.Current.WeatherDescriptions[0];
        //      Humidity = weatherApiResponse.Current.Humidity;
        //      CloudCoverLevel = $"{weatherApiResponse.Current.CloudCoverLevel}%";
        //      IsDay = weatherApiResponse.Current.IsDay.ToUpper();
              
        //  }
        //}
        

    }
}

```
# 3. Bind view model to the Xaml code behind
```csharp

namespace MauiWeatherApp.Pages;

public partial class WeatherInfoPage : ContentPage
{
   
    
	public WeatherInfoPage()
    {
      
		InitializeComponent();
       BindingContext = new WeatherInfoPageViewModel();
    }
}
```
# 4. Create a static constant class for BaseURL and API Keys

```csharp
public static class Constants
 {
     public const string API_KEY = "3c4c6cc6e01ef5394e186eac0c53be45";
     public const string API_BASE_URL = "http://api.weatherstack.com/";
 }

```

# 5. Use `postman` to call the end point

# 6. Paste the data in Json2csharp.com to get the csharp classes. Rename the properties to with pascal casing and use the `JsonPropertyname` Attribute
in a WeatherApiResponse class

```csharp
using System.Text.Json.Serialization;

namespace MauiWeatherApp.Models.ApiModels;

public class WeatherApiResponse
{
    [JsonPropertyName("request")]
    public WeatherApiResponseRequest Request { get; set; }

    [JsonPropertyName("location")]
    public WeatherApiResponseLocation Location { get; set; }

    [JsonPropertyName("current")]
    public WeatherApiResponseCurrent Current { get; set; }
}

public class WeatherApiResponseRequest
{
    [JsonPropertyName("type")]
    public string Type { get; set; }

    [JsonPropertyName("query")]
    public string Query { get; set; }

    [JsonPropertyName("language")]
    public string Language { get; set; }

    [JsonPropertyName("unit")]
    public string Unit { get; set; }
}

public class WeatherApiResponseLocation
{
    [JsonPropertyName("name")]
    public string Name { get; set; }

    [JsonPropertyName("country")]
    public string Country { get; set; }

    [JsonPropertyName("region")]
    public string Region { get; set; }

    [JsonPropertyName("lat")]
    public string Latitude { get; set; }

    [JsonPropertyName("lon")]
    public string Longitude { get; set; }

    [JsonPropertyName("timezone_id")]
    public string TimeZoneId { get; set; }

    [JsonPropertyName("localtime")]
    public string LocalTime { get; set; }

    [JsonPropertyName("localtime_epoch")]
    public int LocalTimeEpoch { get; set; }

    [JsonPropertyName("utc_offset")]
    public string UtcOffset { get; set; }
}

public class WeatherApiResponseCurrent
{
    [JsonPropertyName("observation_time")]
    public string ObservationTime { get; set; }

    [JsonPropertyName("temperature")]
    public int Temperature { get; set; }

    [JsonPropertyName("weather_code")]
    public int WeatherCode { get; set; }

    [JsonPropertyName("weather_icons")]
    public string[] WeatherIcons { get; set; }

    [JsonPropertyName("weather_descriptions")]
    public string[] WeatherDescriptions { get; set; }

    [JsonPropertyName("wind_speed")]
    public int WindSpeed { get; set; }

    [JsonPropertyName("wind_degree")]
    public int WindDegree { get; set; }

    [JsonPropertyName("wind_dir")]
    public string WindDirection { get; set; }

    [JsonPropertyName("pressure")]
    public int Pressure { get; set; }

    [JsonPropertyName("precip")]
    public float PrecipitationLevel { get; set; }

    [JsonPropertyName("humidity")]
    public int Humidity { get; set; }

    [JsonPropertyName("cloudcover")]
    public int CloudCoverLevel { get; set; }

    [JsonPropertyName("feelslike")]
    public int FeelsLike { get; set; }

    [JsonPropertyName("uv_index")]
    public int UvIndex { get; set; }

    [JsonPropertyName("visibility")]
    public int Visibility { get; set; }

    [JsonPropertyName("is_day")]
    public string IsDay { get; set; }
}

```

# 7. create a service

```csharp



   public class WeatherApiService
    {
        private readonly HttpClient _httpClient;

        public WeatherApiService()
        {
            _httpClient = new HttpClient();
            _httpClient.BaseAddress = new Uri(Constants.API_BASE_URL);
        }
        
        public async Task<WeatherApiResponse?> GetWeatherInformation(string latitude, string longitude)
        {
            if (Connectivity.Current.NetworkAccess != NetworkAccess.Internet)
            {
                
                return null; 
            }
            return await _httpClient.GetFromJsonAsync<WeatherApiResponse>($"current?access_key={Constants.API_KEY}&query={latitude},{longitude}");
            
        }
        
    }


```
# call the service in the View Model 

```csharp
 [RelayCommand]
 private async Task FetchWeatherInformation()
 {
     var weatherApiResponse = await _weatherApiService.GetWeatherInformation(Latitude, Longitude);
     if (weatherApiResponse != null)
     {
         WeatherIcon = weatherApiResponse.Current.WeatherIcons[0];
         Temperature = $"{weatherApiResponse.Current.Temperature}°C";
         Location = $"{weatherApiResponse.Location.Name}, {weatherApiResponse.Location.Region}, {weatherApiResponse.Location.Country}";
         WeatherDescription = weatherApiResponse.Current.WeatherDescriptions[0];
         Humidity = weatherApiResponse.Current.Humidity;
         CloudCoverLevel = $"{weatherApiResponse.Current.CloudCoverLevel}%";
         IsDay = weatherApiResponse.Current.IsDay.ToUpper();

     }
 }


```

# The Full ViewModel Class

```csharp

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using CommunityToolkit.Mvvm.ComponentModel;
using CommunityToolkit.Mvvm.Input;
using MauiWeatherApp.Services;

namespace MauiWeatherApp.Models.ViewModels
{
    public partial class WeatherInfoPageViewModel : ObservableObject
    {
        private readonly WeatherApiService _weatherApiService;

        public WeatherInfoPageViewModel()
        {
            _weatherApiService = new WeatherApiService();
        }

        [ObservableProperty]
        private string latitude;

        [ObservableProperty]
        private string longitude;

        [ObservableProperty]
        private string weatherIcon;

        [ObservableProperty]
        private string temperature;

        [ObservableProperty]
        private string weatherDescription;

        [ObservableProperty]
        private string location;

        [ObservableProperty]
        private int humidity;

        [ObservableProperty]
        private string cloudCoverLevel;

        [ObservableProperty]
        private string isDay;

        [RelayCommand]
        private async Task FetchWeatherInformation()
        {
            var weatherApiResponse = await _weatherApiService.GetWeatherInformation(Latitude, Longitude);
            if (weatherApiResponse != null)
            {
                WeatherIcon = weatherApiResponse.Current.WeatherIcons[0];
                Temperature = $"{weatherApiResponse.Current.Temperature}°C";
                Location = $"{weatherApiResponse.Location.Name}, {weatherApiResponse.Location.Region}, {weatherApiResponse.Location.Country}";
                WeatherDescription = weatherApiResponse.Current.WeatherDescriptions[0];
                Humidity = weatherApiResponse.Current.Humidity;
                CloudCoverLevel = $"{weatherApiResponse.Current.CloudCoverLevel}%";
                IsDay = weatherApiResponse.Current.IsDay.ToUpper();

            }
        }


    }
}

```
