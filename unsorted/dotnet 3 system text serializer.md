# Dotnet 3.1 Json Serializer

https://devblogs.microsoft.com/dotnet/try-the-new-system-text-json-apis/

## TLDR:

Dotnet 3.1 come with a new Text Seriazlier instead of using Newtonsoft.

```
using System.Text.Json;
using System.Text.Json.Serialization;

class WeatherForecast
{
    public DateTimeOffset Date { get; set; }
    public int TemperatureC { get; set; }
    public string Summary { get; set; }
}

string Serialize(WeatherForecast value)
{
    return JsonSerializer.ToString<WeatherForecast>(value);
}

WeatherForecast Deserialize(string json)
{
    var options = new JsonSerializerOptions
    {
        AllowTrailingCommas = true
    };

    return JsonSerializer.Parse<WeatherForecast>(json, options);
}

```