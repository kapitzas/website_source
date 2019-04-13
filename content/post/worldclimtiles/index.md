+++
title = "Downloading and merging WorldClim tiles"
date = 2019-03-26
draft = false
tags = ["R", "worldclim"]
summary = "A handy package to download and merge tiled WorldClim and SRTM data"
[image]
preview_only = true
+++

I am regularly working with the WorldClim data set and in some cases had to download and merge the individual high res (0.5 arcmin) tiles for my work. To make things easier I made a little R package out of my workflow that does that really quickly and I will briefly discribe here with examples how it works. I've since also added support for 90m SRTM tiles that are also downloadable via the raster package, please refer to help files fore details.

Download the package from GitHub:

```
devtools::install_github("kapitzas/WorldClimTiles")
```

Load the package and download a boundary file for France. France is a good example because it intersects with two WorldClim tiles.


```
require("WorldClimTiles")
require("raster")

#Download polygon feature for France
fra <- getData("GADM", country = "FRA", level = 0)
```

Get the [WorldClim tile names](https://gis.stackexchange.com/questions/263414/acquiring-and-understanding-data-from-world-clim tiles) that are intersected by the boundary shape of France.

```
#Get WorldClim tile names 
tilenames <- tile_name(fra)
tilenames
```

Download the two tiles ("15", "16"). The default download path is the current working directory.

```
#Download tiles
tiles <- tile_get(tilenames, "bio")
```
The downloaded tiles will look something like this if you plot them next to each other:
![Example image](/img/plotting.png)
Merge the downloaded tiles layer by layer. The function produces a raster stack that contains the merged layers with the correct variable names.
```
merged <- tile_merge(tiles)
```
Plotting the merged raster:
![merged](/img/plotting2.png)
