## fiona-settings

Lightweight wrapper for `fiona` settings & schemas to make it a little easier to manipulate.
Fiona includes a lot of "magic strings" representings GDAL drivers, CRS values, data types, and more. 
While it's all quite intuitive and relatively simple, I have always wished there
was a neater way to manage them all, and so I wrote this.

## examples

```python
import fiona
from fiona_settings import Settings, Type, Driver, CRS

src = fiona.open('my_file.shp')
# from_collection copies the following settings from an already opened collection:
# - driver
# - schema
# - crs
# - encoding
settings = Settings.from_collection(src)
# add a string column with width 25
settings += ('my_column', Type.str(width=25))
settings += ('my_other_column', 'float')  # don't need to use Enum types
# Remove a column by name
settings -= 'column_i_dont_want'

# unpack the settings directly into the fiona.open() function
sink = fiona.open('my_output_file.shp', 'w', **settings)

# Override any option which would be copied with a kwarg. Either
# string or enum values will work.
new_settings = Settings.from_collection(src, driver=Driver.GeoJSON)
sink = fiona.open('my_file.geojson', 'w', **new_settings)
```

