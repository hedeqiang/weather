
<h1 align="center">Weather</h1>

<p align="center">基于高德开放平台的 PHP 天气信息组件。</p>

[![Build Status](https://travis-ci.org/hedeqiang/weather.svg?branch=master)](https://travis-ci.org/hedeqiang/weather)

## 安装

```sh
$ composer require hedeqiang/weather -vvv
```

## 配置

在使用本扩展之前，你需要去 [高德开放平台](https://lbs.amap.com/dev/id/newuser) 注册账号，然后创建应用，获取应用的 API Key。


## 使用

```php
use Hedeqiang\Weather\Weather;

$key = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx';

$weather = new Weather($key);
```

###  获取实时天气

```php
$response = $weather->getLiveWeather('北京');
```
示例：

```json
{
    "status": "1",
    "count": "1",
    "info": "OK",
    "infocode": "10000",
    "lives": [
        {
            "province": "北京",
            "city": "北京市",
            "adcode": "110000",
            "weather": "晴",
            "temperature": "22",
            "winddirection": "南",
            "windpower": "7",
            "humidity": "43",
            "reporttime": "2018-09-26 16:00:00"
        }
    ]
}
```

### 获取近期天气预报

```
$response = $weather->getForecastsWeather('北京');
```
示例：

```json
{
    "status": "1",
    "count": "1",
    "info": "OK",
    "infocode": "10000",
    "forecasts": [
        {
            "city": "北京市",
            "adcode": "110000",
            "province": "北京",
            "reporttime": "2018-09-26 11:00:00",
            "casts": [
                {
                    "date": "2018-09-26",
                    "week": "3",
                    "dayweather": "多云",
                    "nightweather": "多云",
                    "daytemp": "24",
                    "nighttemp": "14",
                    "daywind": "南",
                    "nightwind": "南",
                    "daypower": "≤3",
                    "nightpower": "≤3"
                },
                {
                    "date": "2018-09-27",
                    "week": "4",
                    "dayweather": "小雨",
                    "nightweather": "小雨",
                    "daytemp": "22",
                    "nighttemp": "12",
                    "daywind": "南",
                    "nightwind": "西南",
                    "daypower": "≤3",
                    "nightpower": "≤3"
                },
                {
                    "date": "2018-09-28",
                    "week": "5",
                    "dayweather": "多云",
                    "nightweather": "多云",
                    "daytemp": "24",
                    "nighttemp": "12",
                    "daywind": "西南",
                    "nightwind": "北",
                    "daypower": "4",
                    "nightpower": "≤3"
                },
                {
                    "date": "2018-09-29",
                    "week": "6",
                    "dayweather": "晴",
                    "nightweather": "晴",
                    "daytemp": "22",
                    "nighttemp": "11",
                    "daywind": "北",
                    "nightwind": "北",
                    "daypower": "4",
                    "nightpower": "4"
                }
            ]
        }
    ]
}
```

### 获取 XML 格式返回值

以上两个方法第二个参数为返回值类型，可选 `json` 与 `xml`，默认 `json`：

```php
$response = $weather->getLiveWeather('深圳', 'xml');
```

示例：

```xml
<response>
    <status>1</status>
    <count>1</count>
    <info>OK</info>
    <infocode>10000</infocode>
    <lives type="list">
        <live>
            <province>广东</province>
            <city>深圳市</city>
            <adcode>440300</adcode>
            <weather>中雨</weather>
            <temperature>27</temperature>
            <winddirection>西南</winddirection>
            <windpower>5</windpower>
            <humidity>94</humidity>
            <reporttime>2018-08-21 16:00:00</reporttime>
        </live>
    </lives>
</response>
```

### 参数说明

```
array | string   getLiveWeather(string $city, string $format = 'json')
array | string   getForecastsWeather(string $city, string $format = 'json')
```

> - `$city` - 城市名/[高德地址位置 adcode](https://lbs.amap.com/api/webservice/guide/api/district)，比如：“深圳” 或者（adcode：440300）；
> - `$format`  - 输出的数据格式，默认为 json 格式，当 output 设置为 “`xml`” 时，输出的为 XML 格式的数据。


### 在 Laravel 中使用

在 Laravel 中使用也是同样的安装方式，配置写在 `config/services.php` 中：

```php
    .
    .
    .
     'weather' => [
        'key' => env('WEATHER_API_KEY'),
    ],
```

然后在 `.env` 中配置 `WEATHER_API_KEY` ：

```env
WEATHER_API_KEY=xxxxxxxxxxxxxxxxxxxxx
```

可以用两种方式来获取 `Overtrue\Weather\Weather` 实例：

#### 方法参数注入

```php
    .
    .
    .
    public function edit(Weather $weather) 
    {
        $response = $weather->getLiveWeather('深圳');
    }
    .
    .
    .
```

#### 服务名访问

```php
    .
    .
    .
    public function edit() 
    {
        $response = app('weather')->getLiveWeather('深圳');
    }
    .
    .
    .

```

## 参考

- [高德开放平台天气接口](https://lbs.amap.com/api/webservice/guide/api/weatherinfo/)

## License

MIT