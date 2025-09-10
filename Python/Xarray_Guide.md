# Xarray Guide

## Initial Setting

Define header:

```python
import xarray as xr
import matplotlib.pyplot as plt
import numpy as np
```

## Load Data from NETCDF

For some NETCDF, the file must be decoded by the time variable, then `decode_times=True` or default setting. For the others, `decode_times=False`.

```python
dataset = xr.open_dataset(file_path)
dataset = xr.open_dataset(file_path, decode_times=False)
```

## Data in Dataset

If the dataset is unknown to you, you can see the information of it:

```python
dataset.info()
```

Take the LMD model output for example, you can then see:

```
xarray.Dataset {
dimensions:
	Time = 244 ;
	index = 100 ;
	latitude = 49 ;
	longitude = 65 ;
	altitude = 49 ;
	interlayer = 50 ;
	subsurface_layers = 18 ;

variables:
	float32 Time(Time) ;
		Time:long_name = Time ;
		Time:units = days since 0000-00-0 00:00:00 ;
	float64 controle(index) ;
		controle:title = Control parameters ;
	float64 latitude(latitude) ;
		latitude:units = degrees_north ;
		latitude:long_name = North latitude ;
	float64 longitude(longitude) ;
		longitude:long_name = East longitude ;
		longitude:units = degrees_east ;
	float64 altitude(altitude) ;
		altitude:long_name = pseudo-alt ;
		altitude:units = km ;
		altitude:positive = up ;
		
	blablabla......
	
	float32 temp(Time, altitude, latitude, longitude) ;
		temp:title = temperature ;
		temp:units = K ;
	
	blablabla......

// global attributes:
}
```

The data structure of this dataset you can consider it as many multidimension arrays (they dimension is not the same, which can be seen in information). If you want to choose one variable from the Dataset, you can use:

```python
Temperature = dataset.temp
Temperature2 = dataset["temp"]
```

## Use the data

We need to select the data from some known dimension.

For example, if we want to find the global temperature when time is at the index of 0:

```python
temp_global = dataset['temp'].isel(Time=0)
```

If we want to find the temperature on the equator(latitude=0) when time is at the index of 0, use the method "nearest" to find the nearest one:

```python
temp_global_equator = dataset['temp'].isel(Time=0).sel(latitude=0,method="nearest")
```

Or use the mean data:

```python
temp_longitude_equator=dataset['temp'].isel(Time=0).sel(latitude=0,method="nearest").mean(dim='altitude')
```

## Plot the data

Although the xarray has its own function for plot, I think it is less convenient and hard to play it freely. Matplotlib is still the best choice eg:

```python
plt.plot(dataset['longitude'],temp_longitude_equator)
plt.xlim(-180, 180)
plt.xticks(np.arange(-180, 181, 60))
plt.xlabel('Longitude (degrees)')
plt.ylabel('Temperature (K)')
plt.title('Mean Temperature Distribution along the Martian Equator')
plt.show()
```

## Additional Notes

There are also many knowledge about xarray like the interpolation and the change and write to NETCDF files. When I meet them, I will update this tutorial......

## Reference

 https://www.bilibili.com/video/BV1kv411u7yR

[https://gallery.pangeo.io/repos/pangeo-data/pangeo-tutorial-gallery/xarray.html](https://gallery.pangeo.io/repos/pangeo-data/pangeo-tutorial-gallery/xarray.html)

[https://docs.xarray.dev/en/latest/](https://docs.xarray.dev/en/latest/)

